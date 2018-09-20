---
title: Een virtuele Azure-machine vanuit Go implementeren
description: Implementeer een virtuele machine met de Azure SDK voor Go.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: quickstart
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: a7970be0857fd414d776241b033af0c23457790c
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059132"
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="34899-103">Snelstartgids: een virtuele Azure-machine implementeren vanuit een sjabloon met de Azure SDK voor Go</span><span class="sxs-lookup"><span data-stu-id="34899-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="34899-104">In deze snelstartgids leert u hoe u resources implementeert vanuit een Azure Resource Manager-sjabloon met behulp van de Azure SDK voor Go.</span><span class="sxs-lookup"><span data-stu-id="34899-104">This quickstart shows you how to deploy resources from an Azure Resource Manager template, using the Azure SDK for Go.</span></span> <span data-ttu-id="34899-105">Sjablonen zijn momentopnames van alle resources binnen een [Azure-resourcegroep](/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="34899-105">Templates are snapshots of all of the resources within an [Azure resource group](/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="34899-106">Gaandeweg raakt u bekend met de functionaliteiten en conventies van de SDK.</span><span class="sxs-lookup"><span data-stu-id="34899-106">Along the way, you'll become familiar with the functionality and conventions of the SDK.</span></span>

<span data-ttu-id="34899-107">Aan het einde van deze snelstartgids voert u een VM uit waarbij u zich aanmeldt met een gebruikersnaam en een wachtwoord.</span><span class="sxs-lookup"><span data-stu-id="34899-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

> [!NOTE]
> <span data-ttu-id="34899-108">Als u wilt zien hoe een VM in Go wordt gemaakt zonder een Resource Manager-sjabloon, is er een [essentieel voorbeeld](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) waarin wordt gedemonstreerd hoe u alle VM-resources met de SDK maakt en configureert.</span><span class="sxs-lookup"><span data-stu-id="34899-108">To see the creation of a VM in Go without the use of a Resource Manager template, there is an [imperative sample](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) demonstrating how to build and configure all VM resources with the SDK.</span></span> <span data-ttu-id="34899-109">Als u een sjabloon in dit voorbeeld gebruikt, kunnen we ons richten op SDK-conventies zonder dat we te gedetailleerd moeten ingaan op Azure-servicearchitectuur.</span><span class="sxs-lookup"><span data-stu-id="34899-109">Using a template in this sample allows a focus on SDK conventions without getting into too many details about Azure service architecture.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="34899-110">Als u een lokale installatie van Azure CLI gebruikt, vereist deze snelstart CLI-versie __2.0.28__ of hoger.</span><span class="sxs-lookup"><span data-stu-id="34899-110">If you use a local install of the Azure CLI, this quickstart requires CLI version __2.0.28__ or later.</span></span> <span data-ttu-id="34899-111">Voer `az --version` uit om te controleren of de installatie van uw CLI voldoet aan deze vereiste.</span><span class="sxs-lookup"><span data-stu-id="34899-111">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="34899-112">Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="34899-112">If you need to install or upgrade, see [Install the Azure CLI](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="34899-113">De Azure SDK voor Go installeren</span><span class="sxs-lookup"><span data-stu-id="34899-113">Install the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="34899-114">Een service-principal maken</span><span class="sxs-lookup"><span data-stu-id="34899-114">Create a service principal</span></span>

<span data-ttu-id="34899-115">Als u zich niet-interactief met een toepassing wilt aanmelden bij Azure, hebt u een service-principal nodig.</span><span class="sxs-lookup"><span data-stu-id="34899-115">To sign in non-interactively to Azure with an application, you need a service principal.</span></span> <span data-ttu-id="34899-116">Service-principals zijn onderdeel van op rollen gebaseerd toegangsbeheer (RBAC), waarmee een unieke gebruikersidentiteit wordt gemaakt.</span><span class="sxs-lookup"><span data-stu-id="34899-116">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="34899-117">Als u een nieuwe service-principal wilt maken met de CLI, voert u de volgende opdracht uit:</span><span class="sxs-lookup"><span data-stu-id="34899-117">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --sdk-auth > quickstart.auth
```

<span data-ttu-id="34899-118">Stel de omgevingsvariabele `AZURE_AUTH_LOCATION` in op het volledige pad naar dit bestand.</span><span class="sxs-lookup"><span data-stu-id="34899-118">Set the environment variable `AZURE_AUTH_LOCATION` to be the full path to this file.</span></span> <span data-ttu-id="34899-119">De SDK zoekt en leest vervolgens de referenties rechtstreeks uit dit bestand. U hoeft geen wijzigingen door te voeren of gegevens vast te leggen vanuit de service-principal.</span><span class="sxs-lookup"><span data-stu-id="34899-119">Then the SDK locates and reads the credentials directly from this file, without you having to make any changes or record information from the service principal.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="34899-120">Code ophalen</span><span class="sxs-lookup"><span data-stu-id="34899-120">Get the code</span></span>

<span data-ttu-id="34899-121">Haal de snelstartcode en alle afhankelijkheden ervan op met `go get`.</span><span class="sxs-lookup"><span data-stu-id="34899-121">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

<span data-ttu-id="34899-122">U hoeft geen wijzigingen in de broncode door te voeren als de variabele `AZURE_AUTH_LOCATION` correct is ingesteld.</span><span class="sxs-lookup"><span data-stu-id="34899-122">You don't need to make any source code modifications if the `AZURE_AUTH_LOCATION` variable is properly set.</span></span> <span data-ttu-id="34899-123">Wanneer het programma wordt uitgevoerd, worden de benodigde verificatiegegevens van daaruit geladen.</span><span class="sxs-lookup"><span data-stu-id="34899-123">When the program runs, it loads all the necessary authentication information from there.</span></span>

## <a name="running-the-code"></a><span data-ttu-id="34899-124">De code uitvoeren</span><span class="sxs-lookup"><span data-stu-id="34899-124">Running the code</span></span>

<span data-ttu-id="34899-125">Voer de snelstart uit met de opdracht `go run`.</span><span class="sxs-lookup"><span data-stu-id="34899-125">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

<span data-ttu-id="34899-126">Als de implementatie succesvol is, krijgt u een bericht met de gebruikersnaam, het IP-adres en het wachtwoord voor aanmelding bij de nieuw gemaakte virtuele machine.</span><span class="sxs-lookup"><span data-stu-id="34899-126">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="34899-127">Gebruik SSH om u aan te melden bij deze machine om te bekijken of deze actief is.</span><span class="sxs-lookup"><span data-stu-id="34899-127">SSH into this machine to see if it's up and running.</span></span> 

## <a name="cleaning-up"></a><span data-ttu-id="34899-128">Opschonen</span><span class="sxs-lookup"><span data-stu-id="34899-128">Cleaning up</span></span>

<span data-ttu-id="34899-129">Schoon de resources die tijdens deze snelstart zijn gemaakt op door de resourcegroep met de CLI te verwijderen.</span><span class="sxs-lookup"><span data-stu-id="34899-129">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

<span data-ttu-id="34899-130">Verwijder ook de service-principal die is gemaakt.</span><span class="sxs-lookup"><span data-stu-id="34899-130">Also delete the service principal that was created.</span></span> <span data-ttu-id="34899-131">In het bestand `quickstart.auth` staat een JSON-sleutel voor `clientId`.</span><span class="sxs-lookup"><span data-stu-id="34899-131">In the `quickstart.auth` file, there's a JSON key for `clientId`.</span></span> <span data-ttu-id="34899-132">Kopieer deze waarde in de omgevingsvariabele `CLIENT_ID_VALUE` en voer de volgende Azure CLI-opdracht uit:</span><span class="sxs-lookup"><span data-stu-id="34899-132">Copy this value to the `CLIENT_ID_VALUE` environment variable and run the following Azure CLI command:</span></span>

```azurecli-interactive
az ad sp delete --id ${CLIENT_ID_VALUE}
```

<span data-ttu-id="34899-133">Waar u de waarde voor `CLIENT_ID_VALUE` invoert vanuit `quickstart.auth`.</span><span class="sxs-lookup"><span data-stu-id="34899-133">Where you supply the value for `CLIENT_ID_VALUE` from `quickstart.auth`.</span></span>

> [!WARNING]
> <span data-ttu-id="34899-134">Als u de service-principal voor deze toepassing niet verwijdert, blijft deze actief in uw Azure Active Directory-tenant.</span><span class="sxs-lookup"><span data-stu-id="34899-134">Failing to delete the service principal for this application leaves it active in your Azure Active Directory tenant.</span></span>
> <span data-ttu-id="34899-135">Hoewel zowel de naam en het wachtwoord van de service-principal worden gegenereerd als UUID's, moet u ervoor zorgen dat u goede beveiligingsprocedures volgt door ongebruikte service-principals en Azure Active Directory-toepassingen te verwijderen.</span><span class="sxs-lookup"><span data-stu-id="34899-135">While both the name and password for the service principal are generated as UUIDs, make sure that you follow good security practices by deleting any unused service principals and Azure Active Directory Applications.</span></span>

## <a name="code-in-depth"></a><span data-ttu-id="34899-136">De code uitgesplitst</span><span class="sxs-lookup"><span data-stu-id="34899-136">Code in depth</span></span>

<span data-ttu-id="34899-137">Wat de snelstartcode doet, wordt uitgesplitst in een blok variabelen en verschillende kleine functies die hier allemaal worden behandeld.</span><span class="sxs-lookup"><span data-stu-id="34899-137">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variables-constants-and-types"></a><span data-ttu-id="34899-138">Variabelen, constanten en typen</span><span class="sxs-lookup"><span data-stu-id="34899-138">Variables, constants, and types</span></span>

<span data-ttu-id="34899-139">Aangezien dit een zelfstandige snelstart is, maakt deze gebruik van globale constanten en variabelen.</span><span class="sxs-lookup"><span data-stu-id="34899-139">Since quickstart is self-contained, it uses global constants and variables.</span></span>

```go
const (
    resourceGroupName     = "GoVMQuickstart"
    resourceGroupLocation = "eastus"

    deploymentName = "VMDeployQuickstart"
    templateFile   = "vm-quickstart-template.json"
    parametersFile = "vm-quickstart-params.json"
)

// Information loaded from the authorization file to identify the client
type clientInfo struct {
    SubscriptionID string
    VMPassword     string
}

var (
    ctx        = context.Background()
    clientData clientInfo
    authorizer autorest.Authorizer
)
```

<span data-ttu-id="34899-140">Er worden waarden opgegeven waardoor de gemaakte resources hun naam krijgen.</span><span class="sxs-lookup"><span data-stu-id="34899-140">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="34899-141">De locatie wordt hier ook opgegeven. Deze kunt u aanpassen om te zien hoe implementaties zich in andere datacenters gedragen.</span><span class="sxs-lookup"><span data-stu-id="34899-141">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="34899-142">Niet elk datacenter beschikt over alle vereiste resources.</span><span class="sxs-lookup"><span data-stu-id="34899-142">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="34899-143">Het type `clientInfo` bewaart de informatie die vanuit het verificatiebestand is geladen om clients in te stellen in de SDK en om het VM-wachtwoord in te stellen.</span><span class="sxs-lookup"><span data-stu-id="34899-143">The `clientInfo` type holds the information loaded from the authentication file to set up clients in the SDK and set the VM password.</span></span>

<span data-ttu-id="34899-144">De constanten `templateFile` en `parametersFile` verwijzen naar de bestanden die nodig zijn voor de implementatie.</span><span class="sxs-lookup"><span data-stu-id="34899-144">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="34899-145">De `authorizer` wordt geconfigureerd door de Go-SDK voor verificatie en de variabele `ctx` is een [Go-context](https://blog.golang.org/context) voor de netwerkbewerkingen.</span><span class="sxs-lookup"><span data-stu-id="34899-145">The `authorizer` will be configured by the Go SDK for authentication, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="authentication-and-initialization"></a><span data-ttu-id="34899-146">Verificatie en initialisatie</span><span class="sxs-lookup"><span data-stu-id="34899-146">Authentication and initialization</span></span>

<span data-ttu-id="34899-147">De `init`-functie stelt de verificatie in.</span><span class="sxs-lookup"><span data-stu-id="34899-147">The `init` function sets up authentication.</span></span> <span data-ttu-id="34899-148">Aangezien verificatie de hoofdvoorwaarde is voor alles in de snelstart, is het verstandig om dit als onderdeel van de initialisatie te gebruiken.</span><span class="sxs-lookup"><span data-stu-id="34899-148">Since authentication is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> <span data-ttu-id="34899-149">Ook wordt er bepaalde informatie uit het verificatiebestand geladen die nodig is om clients en de virtuele machine te configureren.</span><span class="sxs-lookup"><span data-stu-id="34899-149">It also loads some information needed from the authentication file to configure clients and the VM.</span></span>

```go
func init() {
    var err error
    authorizer, err = auth.NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
    if err != nil {
        log.Fatalf("Failed to get OAuth config: %v", err)
    }

    authInfo, err := readJSON(os.Getenv("AZURE_AUTH_LOCATION"))
    clientData.SubscriptionID = (*authInfo)["subscriptionId"].(string)
    clientData.VMPassword = (*authInfo)["clientSecret"].(string)
}
```

<span data-ttu-id="34899-150">Eerst wordt [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) aangeroepen om de verificatie-informatie te laden uit het bestand dat zich in `AZURE_AUTH_LOCATION` bevindt.</span><span class="sxs-lookup"><span data-stu-id="34899-150">First, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) is called to load the authentication information from the file located at `AZURE_AUTH_LOCATION`.</span></span> <span data-ttu-id="34899-151">Vervolgens wordt dit bestand handmatig geladen door de functie `readJSON` (hier weggelaten) om de twee waarden op te halen die nodig zijn om de rest van het programma uit te voeren: de abonnements-id van de client en het geheim van de service-principal dat ook wordt gebruikt voor het wachtwoord van de virtuele machine.</span><span class="sxs-lookup"><span data-stu-id="34899-151">Next, this file is loaded manually by the `readJSON` function (omitted here) to pull the two values needed to run the rest of the program: The subscription ID of the client, and the service principal's secret, which is also used for the VM's password.</span></span>

> [!WARNING]
> <span data-ttu-id="34899-152">Als u de snelstart eenvoudig wilt houden, wordt het wachtwoord van de service-principal opnieuw gebruikt.</span><span class="sxs-lookup"><span data-stu-id="34899-152">To keep the quickstart simple, the service principal password is reused.</span></span> <span data-ttu-id="34899-153">Let erop dat u __nooit__ een wachtwoord dat toegang geeft tot uw Azure-resources opnieuw gebruikt.</span><span class="sxs-lookup"><span data-stu-id="34899-153">In production, take care to __never__ reuse a password which gives access to your Azure resources.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="34899-154">Bewerkingsstroom in main()</span><span class="sxs-lookup"><span data-stu-id="34899-154">Flow of operations in main()</span></span>

<span data-ttu-id="34899-155">De functie `main` is eenvoudig: deze geeft enkel de bewerkingsstroom aan en voert een foutencontrole uit.</span><span class="sxs-lookup"><span data-stu-id="34899-155">The `main` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

```go
func main() {
    group, err := createGroup()
    if err != nil {
        log.Fatalf("failed to create group: %v", err)
    }
    log.Printf("Created group: %v", *group.Name)

    log.Printf("Starting deployment: %s", deploymentName)
    result, err := createDeployment()
    if err != nil {
        log.Fatalf("Failed to deploy: %v", err)
    }
    if result.Name != nil {
        log.Printf("Completed deployment %v: %v", deploymentName, *result.Properties.ProvisioningState)
    } else {
        log.Printf("Completed deployment %v (no data returned to SDK)", deploymentName)
    }
    getLogin()
}
```

<span data-ttu-id="34899-156">De stappen die door de code worden uitgevoerd, zijn in volgorde:</span><span class="sxs-lookup"><span data-stu-id="34899-156">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="34899-157">De resourcegroep maken om te implementeren in (`createGroup`)</span><span class="sxs-lookup"><span data-stu-id="34899-157">Create the resource group to deploy to (`createGroup`)</span></span>
* <span data-ttu-id="34899-158">De implementatie in deze groep maken (`createDeployment`)</span><span class="sxs-lookup"><span data-stu-id="34899-158">Create the deployment within this group (`createDeployment`)</span></span>
* <span data-ttu-id="34899-159">De aanmeldingsgegevens voor de geïmplementeerde VM ophalen en weergeven (`getLogin`)</span><span class="sxs-lookup"><span data-stu-id="34899-159">Get and display login information for the deployed VM (`getLogin`)</span></span>

### <a name="create-the-resource-group"></a><span data-ttu-id="34899-160">De resourcegroep maken</span><span class="sxs-lookup"><span data-stu-id="34899-160">Create the resource group</span></span>

<span data-ttu-id="34899-161">De functie `createGroup` maakt de resourcegroep.</span><span class="sxs-lookup"><span data-stu-id="34899-161">The `createGroup` function creates the resource group.</span></span> <span data-ttu-id="34899-162">Als u naar de aanroepstroom en argumenten kijkt, ziet u de manier waarop service-interacties binnen de SDK zijn gestructureerd.</span><span class="sxs-lookup"><span data-stu-id="34899-162">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

```go
func createGroup() (group resources.Group, err error) {
    groupsClient := resources.NewGroupsClient(clientData.SubscriptionID)
    groupsClient.Authorizer = authorizer

        return groupsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                resources.Group{
                        Location: to.StringPtr(resourceGroupLocation)})
}
```

<span data-ttu-id="34899-163">De algemene interactiestroom met een Azure-service is:</span><span class="sxs-lookup"><span data-stu-id="34899-163">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="34899-164">Maak de client die gebruikmaakt van de methode `service.New*Client()`, waarbij `*` het resourcetype is van de `service` waarmee u wilt communiceren.</span><span class="sxs-lookup"><span data-stu-id="34899-164">Create the client using the `service.New*Client()` method, where `*` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="34899-165">Deze functie haalt altijd een abonnements-id op.</span><span class="sxs-lookup"><span data-stu-id="34899-165">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="34899-166">Stel de autorisatiemethode voor de client in, waardoor deze kan communiceren met de externe API.</span><span class="sxs-lookup"><span data-stu-id="34899-166">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="34899-167">Laat de methode de client aanroepen die overeenkomt met de externe API.</span><span class="sxs-lookup"><span data-stu-id="34899-167">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="34899-168">De methoden van serviceclients gebruiken doorgaans de naam van de resource en een metagegevensobject.</span><span class="sxs-lookup"><span data-stu-id="34899-168">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="34899-169">De functie [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) wordt gebruikt om een typeconversie uit te voeren.</span><span class="sxs-lookup"><span data-stu-id="34899-169">The [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="34899-170">De parameters voor methoden van de SDK gebruiken bijna exclusief aanwijzers, dus worden er methoden voor gebruiksgemak gebruikt om de typeconversie eenvoudig te maken.</span><span class="sxs-lookup"><span data-stu-id="34899-170">The parameters for SDK methods almost exclusively take pointers, so convenience methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="34899-171">Raadpleeg de documentatie voor de module [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) voor de volledige lijst met eenvoudige conversieprogramma's en hun gedrag.</span><span class="sxs-lookup"><span data-stu-id="34899-171">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list of convenience converters and their behavior.</span></span>

<span data-ttu-id="34899-172">De methode `groupsClient.CreateOrUpdate` retourneert een aanwijzer naar een gegevenstype die de gehele resourcegroep vertegenwoordigt.</span><span class="sxs-lookup"><span data-stu-id="34899-172">The `groupsClient.CreateOrUpdate` method returns a pointer to a data type representing the resource group.</span></span> <span data-ttu-id="34899-173">Een rechtstreeks geretourneerde waarde van dit type geeft een korte bewerking aan die synchroon moet zijn.</span><span class="sxs-lookup"><span data-stu-id="34899-173">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="34899-174">In de volgende sectie ziet u een voorbeeld van een langdurige bewerking en ziet u hoe u daarmee communiceert.</span><span class="sxs-lookup"><span data-stu-id="34899-174">In the next section, you'll see an example of a long-running operation and how to interact with it.</span></span>

### <a name="perform-the-deployment"></a><span data-ttu-id="34899-175">De implementatie uitvoeren</span><span class="sxs-lookup"><span data-stu-id="34899-175">Perform the deployment</span></span>

<span data-ttu-id="34899-176">Nadat de resourcegroep is gemaakt, is het tijd om de implementatie uit te voeren.</span><span class="sxs-lookup"><span data-stu-id="34899-176">Once the resource group is created, it's time to run the deployment.</span></span> <span data-ttu-id="34899-177">Deze code wordt opgedeeld in kleinere secties om nadruk te leggen op verschillende onderdelen van de logica ervan.</span><span class="sxs-lookup"><span data-stu-id="34899-177">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>

```go
func createDeployment() (deployment resources.DeploymentExtended, err error) {
    template, err := readJSON(templateFile)
    if err != nil {
        return
    }
    params, err := readJSON(parametersFile)
    if err != nil {
        return
    }
    (*params)["vm_password"] = map[string]string{
        "value": clientData.VMPassword,
    }
        // ...
```

<span data-ttu-id="34899-178">De implementatiebestanden worden geladen door `readJSON`, waarvan de details in deze zelfstudie worden overgeslagen.</span><span class="sxs-lookup"><span data-stu-id="34899-178">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="34899-179">Deze functie retourneert een `*map[string]interface{}`, het type dat wordt gebruikt om de metagegevens van de aanroep van de resource-implementatie te bouwen.</span><span class="sxs-lookup"><span data-stu-id="34899-179">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span> <span data-ttu-id="34899-180">Het wachtwoord van de virtuele machine wordt ook handmatig ingesteld op de implementatieparameters.</span><span class="sxs-lookup"><span data-stu-id="34899-180">The VM's password is also set manually on the deployment parameters.</span></span>

```go
        // ...

    deploymentsClient := resources.NewDeploymentsClient(clientData.SubscriptionID)
    deploymentsClient.Authorizer = authorizer

    deploymentFuture, err := deploymentsClient.CreateOrUpdate(
        ctx,
        resourceGroupName,
        deploymentName,
        resources.Deployment{
            Properties: &resources.DeploymentProperties{
                Template:   template,
                Parameters: params,
                Mode:       resources.Incremental,
            },
        },
    )
    if err != nil {
        return
    }
```

<span data-ttu-id="34899-181">Deze code volgt hetzelfde patroon als het maken van de resourcegroep.</span><span class="sxs-lookup"><span data-stu-id="34899-181">This code follows the same pattern as creating the resource group.</span></span> <span data-ttu-id="34899-182">Er wordt een nieuwe client gemaakt die kan verifiëren met Azure, waarna een methode wordt aangeroepen.</span><span class="sxs-lookup"><span data-stu-id="34899-182">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span>
<span data-ttu-id="34899-183">De methode heeft zelfs dezelfde naam (`CreateOrUpdate`) als de bijbehorende methode voor resourcegroepen.</span><span class="sxs-lookup"><span data-stu-id="34899-183">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="34899-184">Dit patroon is zichtbaar in de gehele SDK.</span><span class="sxs-lookup"><span data-stu-id="34899-184">This pattern is seen throughout the SDK.</span></span>
<span data-ttu-id="34899-185">Methoden die gelijksoortig werk uitvoeren, hebben doorgaans dezelfde naam.</span><span class="sxs-lookup"><span data-stu-id="34899-185">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="34899-186">Het grootste verschil zit in de geretourneerde waarde van de methode `deploymentsClient.CreateOrUpdate`.</span><span class="sxs-lookup"><span data-stu-id="34899-186">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate` method.</span></span> <span data-ttu-id="34899-187">Deze waarde is een type [vertraging](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future), dat het [vertragingsontwerppatroon](https://en.wikipedia.org/wiki/Futures_and_promises) volgt.</span><span class="sxs-lookup"><span data-stu-id="34899-187">This value is of the [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) type, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="34899-188">Vertragingen vertegenwoordigen een langdurige bewerking in Azure die u kunt controleren, annuleren of blokkeren bij de voltooiing.</span><span class="sxs-lookup"><span data-stu-id="34899-188">Futures represent a long-running operation in Azure that you can poll, cancel, or block on their completion.</span></span>

```go
        //...
    err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
    if err != nil {
        return
    }
    return deploymentFuture.Result(deploymentsClient)
}
```

<span data-ttu-id="34899-189">Voor dit voorbeeld is het beste om te wachten tot de bewerking is voltooid.</span><span class="sxs-lookup"><span data-stu-id="34899-189">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="34899-190">Voor het wachten op een vertraging is zowel een [contextobject](https://blog.golang.org/context) nodig als de client die de `Future` heeft gemaakt.</span><span class="sxs-lookup"><span data-stu-id="34899-190">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the `Future`.</span></span> <span data-ttu-id="34899-191">Er zijn hier twee mogelijke foutbronnen: een fout die wordt veroorzaakt aan de clientzijde wanneer de methode wordt aangeroepen en een foutmelding vanaf de server.</span><span class="sxs-lookup"><span data-stu-id="34899-191">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="34899-192">De laatste wordt geretourneerd als onderdeel van de aanroep `deploymentFuture.Result`.</span><span class="sxs-lookup"><span data-stu-id="34899-192">The latter is returned as part of the `deploymentFuture.Result` call.</span></span>

### <a name="get-the-assigned-ip-address"></a><span data-ttu-id="34899-193">Het toegewezen IP-adres ophalen</span><span class="sxs-lookup"><span data-stu-id="34899-193">Get the assigned IP address</span></span>

<span data-ttu-id="34899-194">Als u iets wilt doen met de nieuw gemaakte VM, hebt u het toegewezen IP-adres nodig.</span><span class="sxs-lookup"><span data-stu-id="34899-194">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="34899-195">IP-adressen zijn afzonderlijke Azure-resources en gebonden aan Network Interface Controller-resources (NIC).</span><span class="sxs-lookup"><span data-stu-id="34899-195">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

```go
func getLogin() {
    params, err := readJSON(parametersFile)
    if err != nil {
        log.Fatalf("Unable to read parameters. Get login information with `az network public-ip list -g %s", resourceGroupName)
    }

    addressClient := network.NewPublicIPAddressesClient(clientData.SubscriptionID)
    addressClient.Authorizer = authorizer
    ipName := (*params)["publicIPAddresses_QuickstartVM_ip_name"].(map[string]interface{})
    ipAddress, err := addressClient.Get(ctx, resourceGroupName, ipName["value"].(string), "")
    if err != nil {
        log.Fatalf("Unable to get IP information. Try using `az network public-ip list -g %s", resourceGroupName)
    }

    vmUser := (*params)["vm_user"].(map[string]interface{})

    log.Printf("Log in with ssh: %s@%s, password: %s",
        vmUser["value"].(string),
        *ipAddress.PublicIPAddressPropertiesFormat.IPAddress,
        clientData.VMPassword)
}
```

<span data-ttu-id="34899-196">Deze methode is gebaseerd op informatie die in het parameterbestand wordt opgeslagen.</span><span class="sxs-lookup"><span data-stu-id="34899-196">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="34899-197">De code kan rechtstreeks een query uitvoeren op de VM om de NIC op te halen, de query uitvoeren op de NIC om de IP-resource op te halen en vervolgens rechtstreeks een query uitvoeren op de IP-resource.</span><span class="sxs-lookup"><span data-stu-id="34899-197">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="34899-198">Dat is een lange reeks aan afhankelijkheden en op te lossen bewerkingen, waardoor de uitvoering ervan duur wordt.</span><span class="sxs-lookup"><span data-stu-id="34899-198">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="34899-199">Aangezien de JSON-informatie lokaal is, kan deze worden geladen.</span><span class="sxs-lookup"><span data-stu-id="34899-199">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="34899-200">De waarde voor de gebruiker van de virtuele machine wordt ook geladen vanuit de JSON.</span><span class="sxs-lookup"><span data-stu-id="34899-200">The value for the VM user is also loaded from the JSON.</span></span> <span data-ttu-id="34899-201">Het wachtwoord van de virtuele machine is eerder geladen vanuit het verificatiebestand.</span><span class="sxs-lookup"><span data-stu-id="34899-201">The VM password was loaded earlier from the authentication file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="34899-202">Volgende stappen</span><span class="sxs-lookup"><span data-stu-id="34899-202">Next steps</span></span>

<span data-ttu-id="34899-203">In deze snelstartgids hebt u een bestaande sjabloon gebruikt en deze via Go geïmplementeerd.</span><span class="sxs-lookup"><span data-stu-id="34899-203">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="34899-204">U hebt vervolgens via SSH verbinding gemaakt met de nieuw gemaakte VM.</span><span class="sxs-lookup"><span data-stu-id="34899-204">Then you connected to the newly created VM via SSH.</span></span>

<span data-ttu-id="34899-205">Als u meer te weten wilt komen over werken met virtuele machines in de Azure-omgeving met Go, kunt u de [Azure-berekeningsvoorbeelden voor Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) of de [voorbeelden van Azure-resourcebeheer voor Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources) bekijken.</span><span class="sxs-lookup"><span data-stu-id="34899-205">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>

<span data-ttu-id="34899-206">Zie [Verificatie met de Azure SDK voor Go](azure-sdk-go-authorization.md) voor meer informatie over de beschikbare verificatiemethoden in de SDK en welke verificatietypen ze ondersteunen.</span><span class="sxs-lookup"><span data-stu-id="34899-206">To learn more about the available authentication methods in the SDK, and which authentication types they support, see [Authentication with the Azure SDK for Go](azure-sdk-go-authorization.md).</span></span>

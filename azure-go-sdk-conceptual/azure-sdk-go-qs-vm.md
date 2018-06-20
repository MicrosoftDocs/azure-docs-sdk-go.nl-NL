---
title: Een virtuele Azure-machine vanuit Go implementeren
description: Een virtuele machine implementeren met de Azure SDK voor Go.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 04/03/2018
ms.topic: quickstart
ms.prod: azure
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: 1fbcc54df2a2aebce56c5a5800361f3d3aed1ccc
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2018
ms.locfileid: "32319931"
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="885b7-103">Snelstartgids: een virtuele Azure-machine implementeren vanuit een sjabloon met de Azure SDK voor Go</span><span class="sxs-lookup"><span data-stu-id="885b7-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="885b7-104">Deze snelstartgids is gericht op de implementatie van resources vanuit een sjabloon met de Azure SDK voor Go.</span><span class="sxs-lookup"><span data-stu-id="885b7-104">This quickstart focuses on deploying resources from a template with the Azure SDK for Go.</span></span> <span data-ttu-id="885b7-105">Sjablonen zijn momentopnames van alle resources die in een [Azure-resourcegroep](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) zijn opgenomen.</span><span class="sxs-lookup"><span data-stu-id="885b7-105">Templates are snapshots of all of the resources contained within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="885b7-106">Gaandeweg raakt u bekend met de functionaliteiten en conventies van de SDK en ondertussen voert u een handige taak uit.</span><span class="sxs-lookup"><span data-stu-id="885b7-106">Along the way, you'll become familiar with the functionality and conventions of the SDK while performing a useful task.</span></span>

<span data-ttu-id="885b7-107">Aan het einde van deze snelstartgids voert u een VM uit waarbij u zich aanmeldt met een gebruikersnaam en een wachtwoord.</span><span class="sxs-lookup"><span data-stu-id="885b7-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="885b7-108">Als u een lokale installatie van Azure CLI gebruikt, vereist deze snelstart CLI-versie __2.0.28__ of hoger.</span><span class="sxs-lookup"><span data-stu-id="885b7-108">If you use a local install of the Azure CLI, this quickstart requires CLI version __2.0.28__ or later.</span></span> <span data-ttu-id="885b7-109">Voer `az --version` uit om te controleren of de installatie van uw CLI voldoet aan deze vereiste.</span><span class="sxs-lookup"><span data-stu-id="885b7-109">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="885b7-110">Als u uw CLI wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="885b7-110">If you need to install or upgrade, see [Install the Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="885b7-111">De Azure SDK voor Go installeren</span><span class="sxs-lookup"><span data-stu-id="885b7-111">Install the Azure SDK for Go</span></span> 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="885b7-112">Een service-principal maken</span><span class="sxs-lookup"><span data-stu-id="885b7-112">Create a service principal</span></span>


<span data-ttu-id="885b7-113">Als u zich niet-interactief aan wilt melden bij een toepassing, hebt u een service-principal nodig.</span><span class="sxs-lookup"><span data-stu-id="885b7-113">To log in non-interactively with an application, you need a service principal.</span></span> <span data-ttu-id="885b7-114">Service-principals zijn onderdeel van op rollen gebaseerd toegangsbeheer (RBAC), waarmee een unieke gebruikersidentiteit wordt gemaakt.</span><span class="sxs-lookup"><span data-stu-id="885b7-114">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="885b7-115">Als u een nieuwe service-principal wilt maken met de CLI, voert u de volgende opdracht uit:</span><span class="sxs-lookup"><span data-stu-id="885b7-115">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart --sdk-auth > quickstart.auth
```

<span data-ttu-id="885b7-116">Stel de omgevingsvariabele `AZURE_AUTH_LOCATION` in op het volledige pad naar dit bestand.</span><span class="sxs-lookup"><span data-stu-id="885b7-116">Set the environment variable `AZURE_AUTH_LOCATION` to be the full path to this file.</span></span> <span data-ttu-id="885b7-117">De SDK zoekt en leest vervolgens de referenties rechtstreeks uit dit bestand. U hoeft geen wijzigingen door te voeren of gegevens vast te leggen vanuit de service-principal.</span><span class="sxs-lookup"><span data-stu-id="885b7-117">Then the SDK locates and reads the credentials directly from this file, without you having to make any changes or record information from the service principal.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="885b7-118">Code ophalen</span><span class="sxs-lookup"><span data-stu-id="885b7-118">Get the code</span></span>

<span data-ttu-id="885b7-119">Haal de snelstartcode en alle afhankelijkheden ervan op met `go get`.</span><span class="sxs-lookup"><span data-stu-id="885b7-119">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

<span data-ttu-id="885b7-120">U hoeft geen wijzigingen in de broncode door te voeren als de variabele `AZURE_AUTH_LOCATION` correct is ingesteld.</span><span class="sxs-lookup"><span data-stu-id="885b7-120">You don't need to make any source code modifications if the `AZURE_AUTH_LOCATION` variable is properly set.</span></span> <span data-ttu-id="885b7-121">Wanneer het programma wordt uitgevoerd, worden de benodigde verificatiegegevens van daaruit geladen.</span><span class="sxs-lookup"><span data-stu-id="885b7-121">When the program runs, it loads all the necessary authentication information from there.</span></span>

## <a name="running-the-code"></a><span data-ttu-id="885b7-122">De code uitvoeren</span><span class="sxs-lookup"><span data-stu-id="885b7-122">Running the code</span></span>

<span data-ttu-id="885b7-123">Voer de snelstart uit met de opdracht `go run`.</span><span class="sxs-lookup"><span data-stu-id="885b7-123">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

<span data-ttu-id="885b7-124">Als er in de implementatie iets fout gaat, wordt er een bericht weergegeven waarin wordt vermeld dat er een probleem is opgetreden, maar mogelijk staat er niet genoeg informatie in.</span><span class="sxs-lookup"><span data-stu-id="885b7-124">If there is a failure in the deployment, you get a message indicating that there was an issue, but it may not include enough detail.</span></span> <span data-ttu-id="885b7-125">U kunt met de Azure CLI de volledige details van de implementatiefout ophalen met behulp van de volgende opdracht:</span><span class="sxs-lookup"><span data-stu-id="885b7-125">Using the Azure CLI, get the full details of the deployment failure with the following command:</span></span>

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

<span data-ttu-id="885b7-126">Als de implementatie succesvol is, krijgt u een bericht met de gebruikersnaam, het IP-adres en het wachtwoord voor aanmelding bij de nieuw gemaakte virtuele machine.</span><span class="sxs-lookup"><span data-stu-id="885b7-126">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="885b7-127">Gebruik SSH om u aan te melden bij deze machine en bevestig dat deze actief is.</span><span class="sxs-lookup"><span data-stu-id="885b7-127">SSH into this machine to confirm that it is up and running.</span></span>

## <a name="cleaning-up"></a><span data-ttu-id="885b7-128">Opschonen</span><span class="sxs-lookup"><span data-stu-id="885b7-128">Cleaning up</span></span>

<span data-ttu-id="885b7-129">Schoon de resources die tijdens deze snelstart zijn gemaakt op door de resourcegroep met de CLI te verwijderen.</span><span class="sxs-lookup"><span data-stu-id="885b7-129">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a><span data-ttu-id="885b7-130">De code uitgesplitst</span><span class="sxs-lookup"><span data-stu-id="885b7-130">Code in depth</span></span>

<span data-ttu-id="885b7-131">Wat de snelstartcode doet, wordt uitgesplitst in een blok variabelen en verschillende kleine functies die hier allemaal worden behandeld.</span><span class="sxs-lookup"><span data-stu-id="885b7-131">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variables-constants-and-types"></a><span data-ttu-id="885b7-132">Variabelen, constanten en typen</span><span class="sxs-lookup"><span data-stu-id="885b7-132">Variables, constants, and types</span></span>

<span data-ttu-id="885b7-133">Aangezien dit een zelfstandige snelstart is, maakt deze gebruik van globale constanten en variabelen.</span><span class="sxs-lookup"><span data-stu-id="885b7-133">Since quickstart is self-contained, it uses global constants and variables.</span></span>

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

<span data-ttu-id="885b7-134">Er worden waarden opgegeven waardoor de gemaakte resources hun naam krijgen.</span><span class="sxs-lookup"><span data-stu-id="885b7-134">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="885b7-135">De locatie wordt hier ook opgegeven. Deze kunt u aanpassen om te zien hoe implementaties zich in andere datacenters gedragen.</span><span class="sxs-lookup"><span data-stu-id="885b7-135">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="885b7-136">Niet elk datacenter beschikt over alle vereiste resources.</span><span class="sxs-lookup"><span data-stu-id="885b7-136">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="885b7-137">Het type `clientInfo` is gedeclareerd om alle informatie in te kapselen die onafhankelijk van het verificatiebestand moet worden geladen om clients in de SDK en het wachtwoord van de virtuele machine in te stellen.</span><span class="sxs-lookup"><span data-stu-id="885b7-137">The `clientInfo` type is declared to encapsulate all of the information that must be independently loaded from the authentication file to set up clients in the SDK and set the VM password.</span></span>

<span data-ttu-id="885b7-138">De constanten `templateFile` en `parametersFile` verwijzen naar de bestanden die nodig zijn voor de implementatie.</span><span class="sxs-lookup"><span data-stu-id="885b7-138">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="885b7-139">De `authorizer` wordt geconfigureerd door de Go-SDK voor verificatie en de variabele `ctx` is een [Go-context](https://blog.golang.org/context) voor de netwerkbewerkingen.</span><span class="sxs-lookup"><span data-stu-id="885b7-139">The `authorizer` will be configured by the Go SDK for authentication, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="authentication-and-initialization"></a><span data-ttu-id="885b7-140">Verificatie en initialisatie</span><span class="sxs-lookup"><span data-stu-id="885b7-140">Authentication and initialization</span></span>

<span data-ttu-id="885b7-141">De `init`-functie stelt de verificatie in.</span><span class="sxs-lookup"><span data-stu-id="885b7-141">The `init` function sets up authentication.</span></span> <span data-ttu-id="885b7-142">Aangezien verificatie de hoofdvoorwaarde is voor alles in de snelstart, is het verstandig om dit als onderdeel van de initialisatie te gebruiken.</span><span class="sxs-lookup"><span data-stu-id="885b7-142">Since authentication is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> <span data-ttu-id="885b7-143">Ook wordt er bepaalde informatie uit het verificatiebestand geladen die nodig is om clients en de virtuele machine te configureren.</span><span class="sxs-lookup"><span data-stu-id="885b7-143">It also loads some information needed from the authentication file to configure clients and the VM.</span></span>

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

<span data-ttu-id="885b7-144">Eerst wordt [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) aangeroepen om de verificatie-informatie te laden uit het bestand dat zich in `AZURE_AUTH_LOCATION` bevindt.</span><span class="sxs-lookup"><span data-stu-id="885b7-144">First, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) is called to load the authentication information from the file located at `AZURE_AUTH_LOCATION`.</span></span> <span data-ttu-id="885b7-145">Vervolgens wordt dit bestand handmatig geladen door de functie `readJSON` (hier weggelaten) om de twee waarden op te halen die nodig zijn om de rest van het programma uit te voeren: de abonnements-id van de client en het geheim van de service-principal dat ook wordt gebruikt voor het wachtwoord van de virtuele machine.</span><span class="sxs-lookup"><span data-stu-id="885b7-145">Next, this file is loaded manually by the `readJSON` function (omitted here) to pull the two values needed to run the rest of the program: The subscription ID of the client, and the service principal's secret, which is also used for the VM's password.</span></span>

> [!WARNING]
> <span data-ttu-id="885b7-146">Als u de snelstart eenvoudig wilt houden, wordt het wachtwoord van de service-principal opnieuw gebruikt.</span><span class="sxs-lookup"><span data-stu-id="885b7-146">To keep the quickstart simple, the service principal password is reused.</span></span> <span data-ttu-id="885b7-147">Let erop dat u __nooit__ een wachtwoord dat toegang geeft tot uw Azure-resources opnieuw gebruikt.</span><span class="sxs-lookup"><span data-stu-id="885b7-147">In production, take care to __never__ reuse a password which gives access to your Azure resources.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="885b7-148">Bewerkingsstroom in main()</span><span class="sxs-lookup"><span data-stu-id="885b7-148">Flow of operations in main()</span></span>

<span data-ttu-id="885b7-149">De functie `main` is eenvoudig: deze geeft enkel de bewerkingsstroom aan en voert een foutencontrole uit.</span><span class="sxs-lookup"><span data-stu-id="885b7-149">The `main` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

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

<span data-ttu-id="885b7-150">De stappen die door de code worden uitgevoerd, zijn in volgorde:</span><span class="sxs-lookup"><span data-stu-id="885b7-150">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="885b7-151">De resourcegroep maken om te implementeren in (`createGroup`)</span><span class="sxs-lookup"><span data-stu-id="885b7-151">Create the resource group to deploy to (`createGroup`)</span></span>
* <span data-ttu-id="885b7-152">De implementatie in deze groep maken (`createDeployment`)</span><span class="sxs-lookup"><span data-stu-id="885b7-152">Create the deployment within this group (`createDeployment`)</span></span>
* <span data-ttu-id="885b7-153">De aanmeldingsgegevens voor de geïmplementeerde VM verkrijgen en weergeven (`getLogin`)</span><span class="sxs-lookup"><span data-stu-id="885b7-153">Obtain and display login information for the deployed VM (`getLogin`)</span></span>

### <a name="creating-the-resource-group"></a><span data-ttu-id="885b7-154">De resourcegroep maken</span><span class="sxs-lookup"><span data-stu-id="885b7-154">Creating the resource group</span></span>

<span data-ttu-id="885b7-155">De functie `createGroup` maakt de resourcegroep.</span><span class="sxs-lookup"><span data-stu-id="885b7-155">The `createGroup` function creates the resource group.</span></span> <span data-ttu-id="885b7-156">Als u naar de aanroepstroom en argumenten kijkt, ziet u de manier waarop service-interacties binnen de SDK zijn gestructureerd.</span><span class="sxs-lookup"><span data-stu-id="885b7-156">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

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

<span data-ttu-id="885b7-157">De algemene interactiestroom met een Azure-service is:</span><span class="sxs-lookup"><span data-stu-id="885b7-157">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="885b7-158">Maak de client die gebruikmaakt van de methode `service.New*Client()`, waarbij `*` het resourcetype is van de `service` waarmee u wilt communiceren.</span><span class="sxs-lookup"><span data-stu-id="885b7-158">Create the client using the `service.New*Client()` method, where `*` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="885b7-159">Deze functie haalt altijd een abonnements-id op.</span><span class="sxs-lookup"><span data-stu-id="885b7-159">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="885b7-160">Stel de autorisatiemethode voor de client in, waardoor deze kan communiceren met de externe API.</span><span class="sxs-lookup"><span data-stu-id="885b7-160">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="885b7-161">Laat de methode de client aanroepen die overeenkomt met de externe API.</span><span class="sxs-lookup"><span data-stu-id="885b7-161">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="885b7-162">De methoden van serviceclients gebruiken doorgaans de naam van de resource en een metagegevensobject.</span><span class="sxs-lookup"><span data-stu-id="885b7-162">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="885b7-163">De functie [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) wordt gebruikt om een typeconversie uit te voeren.</span><span class="sxs-lookup"><span data-stu-id="885b7-163">The [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="885b7-164">De parameters voor methoden van de SDK gebruiken bijna exclusief aanwijzers, dus worden er methoden voor gebruiksgemak gebruikt om de typeconversie eenvoudig te maken.</span><span class="sxs-lookup"><span data-stu-id="885b7-164">The parameters for SDK methods almost exclusively take pointers, so convenience methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="885b7-165">Raadpleeg de documentatie voor de module [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) voor de volledige lijst met eenvoudige conversieprogramma's en hun gedrag.</span><span class="sxs-lookup"><span data-stu-id="885b7-165">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list of convenience converters and their behavior.</span></span>

<span data-ttu-id="885b7-166">De methode `groupsClient.CreateOrUpdate` retourneert een aanwijzer naar een gegevenstype die de gehele resourcegroep vertegenwoordigt.</span><span class="sxs-lookup"><span data-stu-id="885b7-166">The `groupsClient.CreateOrUpdate` method returns a pointer to a data type representing the resource group.</span></span> <span data-ttu-id="885b7-167">Een rechtstreeks geretourneerde waarde van dit type geeft een korte bewerking aan die synchroon moet zijn.</span><span class="sxs-lookup"><span data-stu-id="885b7-167">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="885b7-168">In de volgende sectie ziet u een voorbeeld van een langdurige bewerking en ziet u hoe u daarmee communiceert.</span><span class="sxs-lookup"><span data-stu-id="885b7-168">In the next section, you'll see an example of a long-running operation and how to interact with it.</span></span>

### <a name="performing-the-deployment"></a><span data-ttu-id="885b7-169">De implementatie uitvoeren</span><span class="sxs-lookup"><span data-stu-id="885b7-169">Performing the deployment</span></span>

<span data-ttu-id="885b7-170">Nadat de resourcegroep is gemaakt, is het tijd om de implementatie uit te voeren.</span><span class="sxs-lookup"><span data-stu-id="885b7-170">Once the resource group is created, it's time to run the deployment.</span></span> <span data-ttu-id="885b7-171">Deze code wordt opgedeeld in kleinere secties om nadruk te leggen op verschillende onderdelen van de logica ervan.</span><span class="sxs-lookup"><span data-stu-id="885b7-171">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>

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

<span data-ttu-id="885b7-172">De implementatiebestanden worden geladen door `readJSON`, waarvan de details in deze zelfstudie worden overgeslagen.</span><span class="sxs-lookup"><span data-stu-id="885b7-172">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="885b7-173">Deze functie retourneert een `*map[string]interface{}`, het type dat wordt gebruikt om de metagegevens van de aanroep van de resource-implementatie te bouwen.</span><span class="sxs-lookup"><span data-stu-id="885b7-173">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span> <span data-ttu-id="885b7-174">Het wachtwoord van de virtuele machine wordt ook handmatig ingesteld op de implementatieparameters.</span><span class="sxs-lookup"><span data-stu-id="885b7-174">The VM's password is also set manually on the deployment parameters.</span></span>

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

<span data-ttu-id="885b7-175">Deze code volgt hetzelfde patroon als het maken van de resourcegroep.</span><span class="sxs-lookup"><span data-stu-id="885b7-175">This code follows the same pattern as creating the resource group.</span></span> <span data-ttu-id="885b7-176">Er wordt een nieuwe client gemaakt die kan verifiëren met Azure, waarna een methode wordt aangeroepen.</span><span class="sxs-lookup"><span data-stu-id="885b7-176">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span> <span data-ttu-id="885b7-177">De methode heeft zelfs dezelfde naam (`CreateOrUpdate`) als de bijbehorende methode voor resourcegroepen.</span><span class="sxs-lookup"><span data-stu-id="885b7-177">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="885b7-178">Dit patroon is zichtbaar in de gehele SDK.</span><span class="sxs-lookup"><span data-stu-id="885b7-178">This pattern is seen throughout the SDK.</span></span> <span data-ttu-id="885b7-179">Methoden die gelijksoortig werk uitvoeren, hebben doorgaans dezelfde naam.</span><span class="sxs-lookup"><span data-stu-id="885b7-179">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="885b7-180">Het grootste verschil zit in de geretourneerde waarde van de methode `deploymentsClient.CreateOrUpdate`.</span><span class="sxs-lookup"><span data-stu-id="885b7-180">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate` method.</span></span> <span data-ttu-id="885b7-181">Deze waarde is een type [vertraging](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future), dat het [vertragingsontwerppatroon](https://en.wikipedia.org/wiki/Futures_and_promises) volgt.</span><span class="sxs-lookup"><span data-stu-id="885b7-181">This value is of the [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) type, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="885b7-182">Vertragingen vertegenwoordigen een langdurige bewerking in Azure die u kunt controleren, annuleren of blokkeren bij de voltooiing.</span><span class="sxs-lookup"><span data-stu-id="885b7-182">Futures represent a long-running operation in Azure that you can poll, cancel, or block on their completion.</span></span>

```go
        //...
    err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
    if err != nil {
        return
    }
    deployment, err = deploymentFuture.Result(deploymentsClient)

    // Work around possible bugs or late-stage failures
    if deployment.Name == nil || err != nil {
        deployment, _ = deploymentsClient.Get(ctx, resourceGroupName, deploymentName)
    }
    return
```

<span data-ttu-id="885b7-183">Voor dit voorbeeld is het beste om te wachten tot de bewerking is voltooid.</span><span class="sxs-lookup"><span data-stu-id="885b7-183">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="885b7-184">Voor het wachten op een vertraging is zowel een [contextobject](https://blog.golang.org/context) nodig als de client die de `Future` heeft gemaakt.</span><span class="sxs-lookup"><span data-stu-id="885b7-184">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the `Future`.</span></span> <span data-ttu-id="885b7-185">Er zijn hier twee mogelijke foutbronnen: een fout die wordt veroorzaakt aan de clientzijde wanneer de methode wordt aangeroepen en een foutmelding vanaf de server.</span><span class="sxs-lookup"><span data-stu-id="885b7-185">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="885b7-186">De laatste wordt geretourneerd als onderdeel van de aanroep `deploymentFuture.Result`.</span><span class="sxs-lookup"><span data-stu-id="885b7-186">The latter is returned as part of the `deploymentFuture.Result` call.</span></span>

<span data-ttu-id="885b7-187">Zodra de implementatie-informatie is opgehaald, is er een tijdelijke oplossing voor mogelijke fouten waarbij de implementatie-informatie mogelijk leeg is met een handmatige aanroep naar `deploymentsClient.Get` om ervoor te zorgen dat de gegevens worden ingevuld.</span><span class="sxs-lookup"><span data-stu-id="885b7-187">Once the deployment information is retrieved, there is a workaround for possible bugs where the deployment information may be empty with a manual call to `deploymentsClient.Get` to ensure that the data is populated.</span></span>

### <a name="obtaining-the-assigned-ip-address"></a><span data-ttu-id="885b7-188">Het toegewezen IP-adres verkrijgen</span><span class="sxs-lookup"><span data-stu-id="885b7-188">Obtaining the assigned IP address</span></span>

<span data-ttu-id="885b7-189">Als u iets wilt doen met de nieuw gemaakte VM, hebt u het toegewezen IP-adres nodig.</span><span class="sxs-lookup"><span data-stu-id="885b7-189">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="885b7-190">IP-adressen zijn afzonderlijke Azure-resources en gebonden aan Network Interface Controller-resources (NIC).</span><span class="sxs-lookup"><span data-stu-id="885b7-190">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

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

<span data-ttu-id="885b7-191">Deze methode is gebaseerd op informatie die in het parameterbestand wordt opgeslagen.</span><span class="sxs-lookup"><span data-stu-id="885b7-191">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="885b7-192">De code kan rechtstreeks een query uitvoeren op de VM om de NIC op te halen, de query uitvoeren op de NIC om de IP-resource op te halen en vervolgens rechtstreeks een query uitvoeren op de IP-resource.</span><span class="sxs-lookup"><span data-stu-id="885b7-192">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="885b7-193">Dat is een lange reeks aan afhankelijkheden en op te lossen bewerkingen, waardoor de uitvoering ervan duur wordt.</span><span class="sxs-lookup"><span data-stu-id="885b7-193">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="885b7-194">Aangezien de JSON-informatie lokaal is, kan deze worden geladen.</span><span class="sxs-lookup"><span data-stu-id="885b7-194">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="885b7-195">De waarde voor de gebruiker van de virtuele machine wordt ook geladen vanuit de JSON.</span><span class="sxs-lookup"><span data-stu-id="885b7-195">The value for the VM user is also loaded from the JSON.</span></span> <span data-ttu-id="885b7-196">Het wachtwoord van de virtuele machine is eerder geladen vanuit het verificatiebestand.</span><span class="sxs-lookup"><span data-stu-id="885b7-196">The VM password was loaded earlier from the authentication file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="885b7-197">Volgende stappen</span><span class="sxs-lookup"><span data-stu-id="885b7-197">Next steps</span></span>

<span data-ttu-id="885b7-198">In deze snelstartgids hebt u een bestaande sjabloon gebruikt en deze via Go geïmplementeerd.</span><span class="sxs-lookup"><span data-stu-id="885b7-198">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="885b7-199">U hebt vervolgens via SSH verbinding gemaakt met de nieuw gemaakte VM om te controleren of deze werd uitgevoerd.</span><span class="sxs-lookup"><span data-stu-id="885b7-199">Then you connected to the newly created VM via SSH to ensure that it's running.</span></span>

<span data-ttu-id="885b7-200">Als u meer te weten wilt komen over werken met virtuele machines in de Azure-omgeving met Go, kunt u de [Azure-berekeningsvoorbeelden voor Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) of de [voorbeelden van Azure-resourcebeheer voor Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources) bekijken.</span><span class="sxs-lookup"><span data-stu-id="885b7-200">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>

<span data-ttu-id="885b7-201">Zie [Verificatie met de Azure SDK voor Go](azure-sdk-go-authorization.md) voor meer informatie over de beschikbare verificatiemethoden in de SDK en welke verificatietypen ze ondersteunen.</span><span class="sxs-lookup"><span data-stu-id="885b7-201">To learn more about the available authentication methods in the SDK, and which authentication types they support, see [Authentication with the Azure SDK for Go](azure-sdk-go-authorization.md).</span></span>

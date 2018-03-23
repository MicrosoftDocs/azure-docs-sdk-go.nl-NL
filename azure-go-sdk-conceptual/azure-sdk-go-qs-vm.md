---
title: Een virtuele Azure-machine vanuit Go implementeren
description: Een virtuele machine implementeren met de Azure SDK voor Go.
author: sptramer
ms.author: sttramer
ms.date: 02/08/2018
ms.topic: quickstart
ms.devlang: go
manager: carmonm
ms.openlocfilehash: 46a1243ff2ff6bfcf3831e2cea3137c1f6051c78
ms.sourcegitcommit: fcc1786d59d2e32c97a9a8e0748e06f564a961bd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="18d5e-103">Snelstartgids: een virtuele Azure-machine implementeren vanuit een sjabloon met de Azure SDK voor Go</span><span class="sxs-lookup"><span data-stu-id="18d5e-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="18d5e-104">Deze snelstartgids is gericht op de implementatie van resources vanuit een sjabloon met de Azure SDK voor Go.</span><span class="sxs-lookup"><span data-stu-id="18d5e-104">This quickstart focuses on deploying resources from a template with the Azure SDK for Go.</span></span> <span data-ttu-id="18d5e-105">Sjablonen zijn momentopnames van alle resources die in een [Azure-resourcegroep](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) zijn opgenomen.</span><span class="sxs-lookup"><span data-stu-id="18d5e-105">Templates are snapshots of all of the resources contained within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="18d5e-106">Gaandeweg raakt u bekend met de functionaliteiten en conventies van de SDK en ondertussen voert u een handige taak uit.</span><span class="sxs-lookup"><span data-stu-id="18d5e-106">Along the way, you'll become familiar with the functionality and conventions of the SDK while performing a useful task.</span></span>

<span data-ttu-id="18d5e-107">Aan het einde van deze snelstartgids voert u een VM uit waarbij u zich aanmeldt met een gebruikersnaam en een wachtwoord.</span><span class="sxs-lookup"><span data-stu-id="18d5e-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="18d5e-108">Als u een lokale installatie van Azure CLI gebruikt, vereist deze snelstartgids CLI-versie 2.0.24 of hoger.</span><span class="sxs-lookup"><span data-stu-id="18d5e-108">If you use a local install of the Azure CLI, this quickstart requires CLI version 2.0.24 or later.</span></span> <span data-ttu-id="18d5e-109">Voer `az --version` uit om te controleren of de installatie van uw CLI voldoet aan deze vereiste.</span><span class="sxs-lookup"><span data-stu-id="18d5e-109">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="18d5e-110">Als u uw CLI wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="18d5e-110">If you need to install or upgrade, see [Install the Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="18d5e-111">De Azure SDK voor Go installeren</span><span class="sxs-lookup"><span data-stu-id="18d5e-111">Install the Azure SDK for Go</span></span> 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="18d5e-112">Een service-principal maken</span><span class="sxs-lookup"><span data-stu-id="18d5e-112">Create a service principal</span></span>

<span data-ttu-id="18d5e-113">Als u zich niet-interactief aan wilt melden bij een toepassing, hebt u een service-principal nodig.</span><span class="sxs-lookup"><span data-stu-id="18d5e-113">To log in non-interactively with an application, you need a service principal.</span></span> <span data-ttu-id="18d5e-114">Service-principals zijn onderdeel van op rollen gebaseerd toegangsbeheer (RBAC), waarmee een unieke gebruikersidentiteit wordt gemaakt.</span><span class="sxs-lookup"><span data-stu-id="18d5e-114">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="18d5e-115">Als u een nieuwe service-principal wilt maken met de CLI, voert u de volgende opdracht uit:</span><span class="sxs-lookup"><span data-stu-id="18d5e-115">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

<span data-ttu-id="18d5e-116">__Zorg ervoor__ dat u de waarden `appId`, `password` en `tenant` in de uitvoer vastlegt.</span><span class="sxs-lookup"><span data-stu-id="18d5e-116">__Make sure__ to record the `appId`, `password`, and `tenant` values in the output.</span></span> <span data-ttu-id="18d5e-117">Deze waarden worden door de toepassing gebruikt om te verifiëren bij Azure.</span><span class="sxs-lookup"><span data-stu-id="18d5e-117">These values are used by the application to authenticate with Azure.</span></span>

<span data-ttu-id="18d5e-118">Bekijk [Een Azure-service-principal maken met de Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli) voor meer informatie over het maken en beheren van service-principals met Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="18d5e-118">For more information on creating and managing service principals with the Azure CLI 2.0, see [Create an Azure service principal with Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="18d5e-119">Code ophalen</span><span class="sxs-lookup"><span data-stu-id="18d5e-119">Get the code</span></span>

<span data-ttu-id="18d5e-120">Haal de snelstartcode en alle afhankelijkheden ervan op met `go get`.</span><span class="sxs-lookup"><span data-stu-id="18d5e-120">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

<span data-ttu-id="18d5e-121">Deze code wordt gecompileerd, maar wordt niet goed uitgevoerd tot u deze de informatie levert over uw Azure-account en de gemaakte service-principal.</span><span class="sxs-lookup"><span data-stu-id="18d5e-121">This code compiles, but doesn't run correctly until you provide it information about your Azure account and the created service principal.</span></span> <span data-ttu-id="18d5e-122">In `main.go` is er een variabele, `config`, waarin zich een `authInfo`-struct bevindt.</span><span class="sxs-lookup"><span data-stu-id="18d5e-122">In `main.go` there is a variable, `config`, which contains an `authInfo` struct.</span></span> <span data-ttu-id="18d5e-123">In de struct moeten de veldwaarden worden vervangen om ervoor te zorgen dat de verificatie correct werkt.</span><span class="sxs-lookup"><span data-stu-id="18d5e-123">This struct needs to have its field values replaced in order to authenticate correctly.</span></span>

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* <span data-ttu-id="18d5e-124">`SubscriptionID`: uw abonnements-id die met de CLI-opdracht kan worden verkregen</span><span class="sxs-lookup"><span data-stu-id="18d5e-124">`SubscriptionID`: Your subscription ID, which can be obtained from the CLI command</span></span>

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* <span data-ttu-id="18d5e-125">`TenantID`: uw tenant-id, de waarde `tenant` die wordt geregistreerd wanneer u de service-principal maakt</span><span class="sxs-lookup"><span data-stu-id="18d5e-125">`TenantID`: Your tenant ID, the `tenant` value recorded when creating the service principal</span></span>
* <span data-ttu-id="18d5e-126">`ServicePrincipalID`: de waarde `appId` die word geregistreerd wanneer u de service-principal maakt</span><span class="sxs-lookup"><span data-stu-id="18d5e-126">`ServicePrincipalID`: The `appId` value recorded when creating the service principal</span></span>
* <span data-ttu-id="18d5e-127">`ServicePrincipalSecret`: de waarde `password` die word geregistreerd wanneer u de service-principal maakt</span><span class="sxs-lookup"><span data-stu-id="18d5e-127">`ServicePrincipalSecret`: The `password` value recorded when creating the service principal</span></span>

<span data-ttu-id="18d5e-128">U moet ook een waarde in het bestand `vm-quickstart-params.json` aanpassen.</span><span class="sxs-lookup"><span data-stu-id="18d5e-128">You also need to edit a value in the `vm-quickstart-params.json` file.</span></span>

```json
    "vm_password": {
        "value": "_"
    }
```

* <span data-ttu-id="18d5e-129">`vm_password`: het wachtwoord van het gebruikersaccount van de VM.</span><span class="sxs-lookup"><span data-stu-id="18d5e-129">`vm_password`: The password for the VM user account.</span></span> <span data-ttu-id="18d5e-130">Het moet uit 12-72 tekens bestaan en het moet 3 van de volgende tekens bevatten:</span><span class="sxs-lookup"><span data-stu-id="18d5e-130">It must be 12-72 characters in length and contain 3 of the following characters:</span></span>
  * <span data-ttu-id="18d5e-131">Een kleine letter</span><span class="sxs-lookup"><span data-stu-id="18d5e-131">A lowercase letter</span></span>
  * <span data-ttu-id="18d5e-132">Een hoofdletter</span><span class="sxs-lookup"><span data-stu-id="18d5e-132">An uppercase letter</span></span>
  * <span data-ttu-id="18d5e-133">Een getal</span><span class="sxs-lookup"><span data-stu-id="18d5e-133">A number</span></span>
  * <span data-ttu-id="18d5e-134">Een symbool</span><span class="sxs-lookup"><span data-stu-id="18d5e-134">A symbol</span></span>

## <a name="running-the-code"></a><span data-ttu-id="18d5e-135">De code uitvoeren</span><span class="sxs-lookup"><span data-stu-id="18d5e-135">Running the code</span></span>

<span data-ttu-id="18d5e-136">Voer de snelstart uit met de opdracht `go run`.</span><span class="sxs-lookup"><span data-stu-id="18d5e-136">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

<span data-ttu-id="18d5e-137">Als er in de implementatie iets fout gaat, krijgt u een bericht waarin staat dat er een probleem was, maar er staat geen specifieke informatie in.</span><span class="sxs-lookup"><span data-stu-id="18d5e-137">If there is a failure in the deployment, you get a message indicating that there was an issue, but without any specific details.</span></span> <span data-ttu-id="18d5e-138">U kunt met de Azure CLI de details van de implementatiefout ophalen met behulp van de volgende opdracht:</span><span class="sxs-lookup"><span data-stu-id="18d5e-138">Using the Azure CLI, get the details of the deployment failure with the following command:</span></span>

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

<span data-ttu-id="18d5e-139">Als de implementatie succesvol is, krijgt u een bericht met de gebruikersnaam, het IP-adres en het wachtwoord voor aanmelding bij de nieuw gemaakte virtuele machine.</span><span class="sxs-lookup"><span data-stu-id="18d5e-139">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="18d5e-140">Gebruik SSH om u aan te melden bij deze machine en bevestig dat deze actief is.</span><span class="sxs-lookup"><span data-stu-id="18d5e-140">SSH into this machine to confirm that it is up and running.</span></span>

## <a name="cleaning-up"></a><span data-ttu-id="18d5e-141">Opschonen</span><span class="sxs-lookup"><span data-stu-id="18d5e-141">Cleaning up</span></span>

<span data-ttu-id="18d5e-142">Schoon de resources die tijdens deze snelstart zijn gemaakt op door de resourcegroep met de CLI te verwijderen.</span><span class="sxs-lookup"><span data-stu-id="18d5e-142">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a><span data-ttu-id="18d5e-143">De code uitgesplitst</span><span class="sxs-lookup"><span data-stu-id="18d5e-143">Code in depth</span></span>

<span data-ttu-id="18d5e-144">Wat de snelstartcode doet, wordt uitgesplitst in een blok variabelen en verschillende kleine functies die hier allemaal worden behandeld.</span><span class="sxs-lookup"><span data-stu-id="18d5e-144">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variable-assignments-and-structs"></a><span data-ttu-id="18d5e-145">Variabeletoewijzingen en structs</span><span class="sxs-lookup"><span data-stu-id="18d5e-145">Variable assignments and structs</span></span>

<span data-ttu-id="18d5e-146">Aangezien de snelstartcode zelfstandig is, worden er globale variabelen gebruikt in plaats van opdrachtregelopties of omgevingsvariabelen.</span><span class="sxs-lookup"><span data-stu-id="18d5e-146">Since quickstart is self-contained, it uses global variables rather than command-line options or environment variables.</span></span>

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

<span data-ttu-id="18d5e-147">De struct `authInfo` wordt opgegeven om alle informatie in te kapselen die nodig is voor de autorisatie met Azure-services.</span><span class="sxs-lookup"><span data-stu-id="18d5e-147">The `authInfo` struct is declared to encapsulate all of the information needed for authorization with Azure services.</span></span>

```go
const (
    resourceGroupName     = "GoVMQuickstart"
    resourceGroupLocation = "eastus"

    deploymentName = "VMDeployQuickstart"
    templateFile   = "vm-quickstart-template.json"
    parametersFile = "vm-quickstart-params.json"
)

var (
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }

    ctx = context.Background()

    token *adal.ServicePrincipalToken
)
```

<span data-ttu-id="18d5e-148">Er worden waarden opgegeven waardoor de gemaakte resources hun naam krijgen.</span><span class="sxs-lookup"><span data-stu-id="18d5e-148">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="18d5e-149">De locatie wordt hier ook opgegeven. Deze kunt u aanpassen om te zien hoe implementaties zich in andere datacenters gedragen.</span><span class="sxs-lookup"><span data-stu-id="18d5e-149">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="18d5e-150">Niet elk datacenter beschikt over alle vereiste resources.</span><span class="sxs-lookup"><span data-stu-id="18d5e-150">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="18d5e-151">De constanten `templateFile` en `parametersFile` verwijzen naar de bestanden die nodig zijn voor de implementatie.</span><span class="sxs-lookup"><span data-stu-id="18d5e-151">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="18d5e-152">De token van de service-principal wordt later behandeld en de variabele `ctx` is een [Go-context](https://blog.golang.org/context) voor de netwerkbewerkingen.</span><span class="sxs-lookup"><span data-stu-id="18d5e-152">The service principal token is covered later, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="init-and-authorization"></a><span data-ttu-id="18d5e-153">init() en autorisatie</span><span class="sxs-lookup"><span data-stu-id="18d5e-153">init() and authorization</span></span>

<span data-ttu-id="18d5e-154">De methode `init()` voor de code stelt autorisatie in.</span><span class="sxs-lookup"><span data-stu-id="18d5e-154">The `init()` method for the code sets up authorization.</span></span> <span data-ttu-id="18d5e-155">Aangezien autorisatie de hoofdvoorwaarde is voor alles in de snelstart, is het verstandig om dit als onderdeel van de initialisatie te gebruiken.</span><span class="sxs-lookup"><span data-stu-id="18d5e-155">Since authorization is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> 

```go
// Authenticate with the Azure services over OAuth, using a service principal.
func init() {
    oauthConfig, err := adal.NewOAuthConfig(azure.PublicCloud.ActiveDirectoryEndpoint, config.TenantID)
    if err != nil {
        log.Fatalf("Failed to get OAuth config: %v\n", err)
    }
    token, err = adal.NewServicePrincipalToken(
        *oauthConfig,
        config.ServicePrincipalID,
        config.ServicePrincipalSecret,
        azure.PublicCloud.ResourceManagerEndpoint)
    if err != nil {
        log.Fatalf("faled to get token: %v\n", err)
    }
}
```

<span data-ttu-id="18d5e-156">Deze code voltooit twee stappen voor autorisatie:</span><span class="sxs-lookup"><span data-stu-id="18d5e-156">This code completes two steps for authorization:</span></span>

* <span data-ttu-id="18d5e-157">OAuth-configuratiegegevens voor de `TenantID` worden opgehaald tijdens de communicatie met Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="18d5e-157">OAuth configuration information for the `TenantID` is retrieved by interfacing with Azure Active Directory.</span></span> <span data-ttu-id="18d5e-158">Het object [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) bevat de eindpunten die worden gebruikt in de standaard Azure-configuratie.</span><span class="sxs-lookup"><span data-stu-id="18d5e-158">The [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) object contains endpoints used in the standard Azure configuration.</span></span>
* <span data-ttu-id="18d5e-159">De functie [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) wordt aangeroepen.</span><span class="sxs-lookup"><span data-stu-id="18d5e-159">The [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) function is called.</span></span> <span data-ttu-id="18d5e-160">Deze functie haalt de OAuth-gegevens samen met de referenties van de service-principal op, evenals de stijl van het gebruikte Azure-beheer.</span><span class="sxs-lookup"><span data-stu-id="18d5e-160">This function takes the OAuth information along with the service principal login, as well as which style of Azure management is being used.</span></span> <span data-ttu-id="18d5e-161">Tenzij u specifieke vereisten hebt en u weet wat u doet, zou deze waarde altijd `.ResourceManagerEndpoint` moeten zijn.</span><span class="sxs-lookup"><span data-stu-id="18d5e-161">Unless you have specific requirements and know what you're doing, this value should always be `.ResourceManagerEndpoint`.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="18d5e-162">Bewerkingsstroom in main()</span><span class="sxs-lookup"><span data-stu-id="18d5e-162">Flow of operations in main()</span></span>

<span data-ttu-id="18d5e-163">De functie `main()` is eenvoudig: deze geeft enkel de bewerkingsstroom aan en voert een foutencontrole uit.</span><span class="sxs-lookup"><span data-stu-id="18d5e-163">The `main()` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

```go
func main() {
    group, err := createGroup()
    if err != nil {
        log.Fatalf("failed to create group: %v", err)
    }
    log.Printf("created group: %v\n", *group.Name)

    log.Println("starting deployment")
    result, err := createDeployment()
    if err != nil {
        log.Fatalf("Failed to deploy correctly: %v", err)
    }
    log.Printf("Completed deployment: %v", *result.Name)
    getLogin()
}
```

<span data-ttu-id="18d5e-164">De stappen die door de code worden uitgevoerd, zijn in volgorde:</span><span class="sxs-lookup"><span data-stu-id="18d5e-164">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="18d5e-165">De resourcegroep maken om te implementeren in (`createGroup()`)</span><span class="sxs-lookup"><span data-stu-id="18d5e-165">Create the resource group to deploy to (`createGroup()`)</span></span>
* <span data-ttu-id="18d5e-166">De implementatie in deze groep maken (`createDeployment()`)</span><span class="sxs-lookup"><span data-stu-id="18d5e-166">Create the deployment within this group (`createDeployment()`)</span></span>
* <span data-ttu-id="18d5e-167">De aanmeldingsgegevens voor de geïmplementeerde VM verkrijgen en weergeven (`getLogin()`)</span><span class="sxs-lookup"><span data-stu-id="18d5e-167">Obtain and display login information for the deployed VM (`getLogin()`)</span></span>

### <a name="creating-the-resource-group"></a><span data-ttu-id="18d5e-168">De resourcegroep maken</span><span class="sxs-lookup"><span data-stu-id="18d5e-168">Creating the resource group</span></span>

<span data-ttu-id="18d5e-169">De functie `createGroup()` maakt de resourcegroep.</span><span class="sxs-lookup"><span data-stu-id="18d5e-169">The `createGroup()` function creates the resource group.</span></span> <span data-ttu-id="18d5e-170">Als u naar de aanroepstroom en argumenten kijkt, ziet u de manier waarop service-interacties binnen de SDK zijn gestructureerd.</span><span class="sxs-lookup"><span data-stu-id="18d5e-170">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

```go
func createGroup() (group resources.Group, err error) {
        groupsClient := resources.NewGroupsClient(config.SubscriptionID)
        groupsClient.Authorizer = autorest.NewBearerAuthorizer(token)

        return groupsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                resources.Group{
                        Location: to.StringPtr(resourceGroupLocation)})
}
```

<span data-ttu-id="18d5e-171">De algemene interactiestroom met een Azure-service is:</span><span class="sxs-lookup"><span data-stu-id="18d5e-171">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="18d5e-172">Maak de client die gebruikmaakt van de methode `service.NewXClient()`, waarbij `X` het resourcetype is van de `service` waarmee u wilt communiceren.</span><span class="sxs-lookup"><span data-stu-id="18d5e-172">Create the client using the `service.NewXClient()` method, where `X` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="18d5e-173">Deze functie haalt altijd een abonnements-id op.</span><span class="sxs-lookup"><span data-stu-id="18d5e-173">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="18d5e-174">Stel de autorisatiemethode voor de client in, waardoor deze kan communiceren met de externe API.</span><span class="sxs-lookup"><span data-stu-id="18d5e-174">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="18d5e-175">Laat de methode de client aanroepen die overeenkomt met de externe API.</span><span class="sxs-lookup"><span data-stu-id="18d5e-175">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="18d5e-176">De methoden van serviceclients gebruiken doorgaans de naam van de resource en een metagegevensobject.</span><span class="sxs-lookup"><span data-stu-id="18d5e-176">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="18d5e-177">De functie [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) wordt gebruikt om een typeconversie uit te voeren.</span><span class="sxs-lookup"><span data-stu-id="18d5e-177">The [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="18d5e-178">De parameterstructs voor methoden van de SDK gebruiken bijna exclusief aanwijzers, dus deze methoden worden gebruikt om de typeconversie eenvoudig te maken.</span><span class="sxs-lookup"><span data-stu-id="18d5e-178">The parameters structs for methods of the SDK almost exclusively take pointers, so these methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="18d5e-179">Raadpleeg de documentatie voor de module [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) voor de volledige lijst en het gedrag van eenvoudige conversieprogramma's.</span><span class="sxs-lookup"><span data-stu-id="18d5e-179">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list and behavior of convenience converters.</span></span>

<span data-ttu-id="18d5e-180">De bewerking `groupsClient.CreateOrUpdate()` retourneert een aanwijzer naar een gegevensstruct die de gehele resourcegroep vertegenwoordigt.</span><span class="sxs-lookup"><span data-stu-id="18d5e-180">The `groupsClient.CreateOrUpdate()` operation returns a pointer to a data struct representing the resource group.</span></span> <span data-ttu-id="18d5e-181">Een rechtstreeks geretourneerde waarde van dit type geeft een korte bewerking aan die synchroon moet zijn.</span><span class="sxs-lookup"><span data-stu-id="18d5e-181">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="18d5e-182">In de volgende sectie ziet u een exemplaar van een langdurige bewerking en ziet u hoe u daarmee communiceert.</span><span class="sxs-lookup"><span data-stu-id="18d5e-182">In the next section, you'll see an example of a long-running operation and how to interact with them.</span></span>

### <a name="performing-the-deployment"></a><span data-ttu-id="18d5e-183">De implementatie uitvoeren</span><span class="sxs-lookup"><span data-stu-id="18d5e-183">Performing the deployment</span></span>

<span data-ttu-id="18d5e-184">Nadat de groep is gemaakt waarin de resources worden bewaard, is het tijd om de implementatie uit te voeren.</span><span class="sxs-lookup"><span data-stu-id="18d5e-184">Once the group to contain its resources is created, it's time to run the deployment.</span></span> <span data-ttu-id="18d5e-185">Deze code wordt opgedeeld in kleinere secties om nadruk te leggen op verschillende onderdelen van de logica ervan.</span><span class="sxs-lookup"><span data-stu-id="18d5e-185">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>


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

        // ...
```

<span data-ttu-id="18d5e-186">De implementatiebestanden worden geladen door `readJSON`, waarvan de details in deze zelfstudie worden overgeslagen.</span><span class="sxs-lookup"><span data-stu-id="18d5e-186">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="18d5e-187">Deze functie retourneert een `*map[string]interface{}`, het type dat wordt gebruikt om de metagegevens van de aanroep van de resource-implementatie te bouwen.</span><span class="sxs-lookup"><span data-stu-id="18d5e-187">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span>

```go
        // ...
        
        deploymentsClient := resources.NewDeploymentsClient(config.SubscriptionID)
        deploymentsClient.Authorizer = autorest.NewBearerAuthorizer(token)

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
                log.Fatalf("Failed to create deployment: %v", err)
        }
        //...
```

<span data-ttu-id="18d5e-188">Deze code volgt hetzelfde patroon als het maken van de resourcegroep.</span><span class="sxs-lookup"><span data-stu-id="18d5e-188">This code follows the same pattern as with creating the resource group.</span></span> <span data-ttu-id="18d5e-189">Er wordt een nieuwe client gemaakt die kan verifiëren met Azure, waarna een methode wordt aangeroepen.</span><span class="sxs-lookup"><span data-stu-id="18d5e-189">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span> <span data-ttu-id="18d5e-190">De methode heeft zelfs dezelfde naam (`CreateOrUpdate`) als de bijbehorende methode voor resourcegroepen.</span><span class="sxs-lookup"><span data-stu-id="18d5e-190">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="18d5e-191">Dit patroon komt steeds opnieuw terug in de SDK.</span><span class="sxs-lookup"><span data-stu-id="18d5e-191">This pattern is seen again and again in the SDK.</span></span> <span data-ttu-id="18d5e-192">Methoden die gelijksoortig werk uitvoeren, hebben doorgaans dezelfde naam.</span><span class="sxs-lookup"><span data-stu-id="18d5e-192">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="18d5e-193">Het grootste verschil zit in de geretourneerde waarde van de methode `deploymentsClient.CreateOrUpdate()`.</span><span class="sxs-lookup"><span data-stu-id="18d5e-193">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate()` method.</span></span> <span data-ttu-id="18d5e-194">Deze waarde is een `Future`-object, dat het [vertragingsontwerppatroon](https://en.wikipedia.org/wiki/Futures_and_promises) volgt.</span><span class="sxs-lookup"><span data-stu-id="18d5e-194">This value is a `Future` object, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="18d5e-195">Vertragingen vertegenwoordigen een langdurige bewerking in Azure en het is handig deze zo nu en dan te peilen terwijl u ander werk uitvoert.</span><span class="sxs-lookup"><span data-stu-id="18d5e-195">Futures represent a long-running operation in Azure that you may want to occasionally poll while performing other work.</span></span>

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

<span data-ttu-id="18d5e-196">Voor dit voorbeeld is het beste om te wachten tot de bewerking is voltooid.</span><span class="sxs-lookup"><span data-stu-id="18d5e-196">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="18d5e-197">Voor het wachten op een vertraging is zowel een [contextobject](https://blog.golang.org/context) nodig als de client die het vertragingsobject heeft gemaakt.</span><span class="sxs-lookup"><span data-stu-id="18d5e-197">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the Future object.</span></span> <span data-ttu-id="18d5e-198">Er zijn hier twee mogelijke foutbronnen: een fout die wordt veroorzaakt aan de clientzijde wanneer de methode wordt aangeroepen en een foutmelding vanaf de server.</span><span class="sxs-lookup"><span data-stu-id="18d5e-198">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="18d5e-199">De laatste wordt geretourneerd als onderdeel van de aanroep `deploymentFuture.Result()`.</span><span class="sxs-lookup"><span data-stu-id="18d5e-199">The latter is returned as part of the `deploymentFuture.Result()` call.</span></span>

### <a name="obtaining-the-assigned-ip-address"></a><span data-ttu-id="18d5e-200">Het toegewezen IP-adres verkrijgen</span><span class="sxs-lookup"><span data-stu-id="18d5e-200">Obtaining the assigned IP address</span></span>

<span data-ttu-id="18d5e-201">Als u iets wilt doen met de nieuw gemaakte VM, hebt u het toegewezen IP-adres nodig.</span><span class="sxs-lookup"><span data-stu-id="18d5e-201">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="18d5e-202">IP-adressen zijn afzonderlijke Azure-resources en gebonden aan Network Interface Controller-resources (NIC).</span><span class="sxs-lookup"><span data-stu-id="18d5e-202">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

```go
func getLogin() {
        params, err := readJSON(parametersFile)
        if err != nil {
                log.Fatalf("Unable to read parameters. Get login information with `az network public-ip list -g %s", resourceGroupName)
        }

        addressClient := network.NewPublicIPAddressesClient(config.SubscriptionID)
        addressClient.Authorizer = autorest.NewBearerAuthorizer(token)
        ipName := (*params)["publicIPAddresses_QuickstartVM_ip_name"].(map[string]interface{})
        ipAddress, err := addressClient.Get(ctx, resourceGroupName, ipName["value"].(string), "")
        if err != nil {
                log.Fatalf("Unable to get IP information. Try using `az network public-ip list -g %s", resourceGroupName)
        }

        vmUser := (*params)["vm_user"].(map[string]interface{})
        vmPass := (*params)["vm_password"].(map[string]interface{})

        log.Printf("Log in with ssh: %s@%s, password: %s",
                vmUser["value"].(string),
                *ipAddress.PublicIPAddressPropertiesFormat.IPAddress,
                vmPass["value"].(string))
}
```

<span data-ttu-id="18d5e-203">Deze methode is gebaseerd op informatie die in het parameterbestand wordt opgeslagen.</span><span class="sxs-lookup"><span data-stu-id="18d5e-203">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="18d5e-204">De code kan rechtstreeks een query uitvoeren op de VM om de NIC op te halen, de query uitvoeren op de NIC om de IP-resource op te halen en vervolgens rechtstreeks een query uitvoeren op de IP-resource.</span><span class="sxs-lookup"><span data-stu-id="18d5e-204">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="18d5e-205">Dat is een lange reeks aan afhankelijkheden en op te lossen bewerkingen, waardoor de uitvoering ervan duur wordt.</span><span class="sxs-lookup"><span data-stu-id="18d5e-205">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="18d5e-206">Aangezien de JSON-informatie lokaal is, kan deze worden geladen.</span><span class="sxs-lookup"><span data-stu-id="18d5e-206">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="18d5e-207">De waarden voor de gebruiker en het wachtwoord van de VM worden eveneens vanuit de JSON geladen.</span><span class="sxs-lookup"><span data-stu-id="18d5e-207">The values for the VM user and password are likewise loaded from the JSON.</span></span>

## <a name="next-steps"></a><span data-ttu-id="18d5e-208">Volgende stappen</span><span class="sxs-lookup"><span data-stu-id="18d5e-208">Next steps</span></span>

<span data-ttu-id="18d5e-209">In deze snelstartgids hebt u een bestaande sjabloon gebruikt en deze via Go geïmplementeerd.</span><span class="sxs-lookup"><span data-stu-id="18d5e-209">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="18d5e-210">U hebt vervolgens via SSH verbinding gemaakt met de nieuw gemaakte VM om te controleren of deze werd uitgevoerd.</span><span class="sxs-lookup"><span data-stu-id="18d5e-210">Then you connected to the newly created VM via SSH to ensure that it's running.</span></span>

<span data-ttu-id="18d5e-211">Als u meer te weten wilt komen over werken met virtuele machines in de Azure-omgeving met Go, kunt u de [Azure-berekeningsvoorbeelden voor Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) of de [voorbeelden van Azure-resourcebeheer voor Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources) bekijken.</span><span class="sxs-lookup"><span data-stu-id="18d5e-211">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>

---
title: Een virtuele Azure-machine vanuit Go implementeren
description: Implementeer een virtuele machine met de Azure SDK voor Go.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 07/13/2018
ms.topic: quickstart
ms.prod: azure
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: 6b1de35748fb7694d45715fa7f028d5730530d2e
ms.sourcegitcommit: d1790b317a8fcb4d672c654dac2a925a976589d4
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39039553"
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a>Snelstartgids: een virtuele Azure-machine implementeren vanuit een sjabloon met de Azure SDK voor Go

Deze snelstartgids is gericht op de implementatie van resources vanuit een sjabloon met de Azure SDK voor Go. Sjablonen zijn momentopnames van alle resources die in een [Azure-resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) zijn opgenomen. Gaandeweg raakt u bekend met de functionaliteiten en conventies van de SDK en ondertussen voert u een handige taak uit.

Aan het einde van deze snelstartgids voert u een VM uit waarbij u zich aanmeldt met een gebruikersnaam en een wachtwoord.

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

Als u een lokale installatie van Azure CLI gebruikt, vereist deze snelstart CLI-versie __2.0.28__ of hoger. Voer `az --version` uit om te controleren of de installatie van uw CLI voldoet aan deze vereiste. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli).

## <a name="install-the-azure-sdk-for-go"></a>De Azure SDK voor Go installeren

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a>Een service-principal maken

Als u zich niet-interactief met een toepassing wilt aanmelden bij Azure, hebt u een service-principal nodig. Service-principals zijn onderdeel van op rollen gebaseerd toegangsbeheer (RBAC), waarmee een unieke gebruikersidentiteit wordt gemaakt. Als u een nieuwe service-principal wilt maken met de CLI, voert u de volgende opdracht uit:

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart --sdk-auth > quickstart.auth
```

Stel de omgevingsvariabele `AZURE_AUTH_LOCATION` in op het volledige pad naar dit bestand. De SDK zoekt en leest vervolgens de referenties rechtstreeks uit dit bestand. U hoeft geen wijzigingen door te voeren of gegevens vast te leggen vanuit de service-principal.

## <a name="get-the-code"></a>Code ophalen

Haal de snelstartcode en alle afhankelijkheden ervan op met `go get`.

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

U hoeft geen wijzigingen in de broncode door te voeren als de variabele `AZURE_AUTH_LOCATION` correct is ingesteld. Wanneer het programma wordt uitgevoerd, worden de benodigde verificatiegegevens van daaruit geladen.

## <a name="running-the-code"></a>De code uitvoeren

Voer de snelstart uit met de opdracht `go run`.

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

Als er in de implementatie iets fout gaat, wordt er een bericht weergegeven waarin wordt vermeld dat er een probleem is opgetreden, maar mogelijk staat er niet genoeg informatie in. U kunt met de Azure CLI de volledige details van de implementatiefout ophalen met behulp van de volgende opdracht:

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

Als de implementatie succesvol is, krijgt u een bericht met de gebruikersnaam, het IP-adres en het wachtwoord voor aanmelding bij de nieuw gemaakte virtuele machine. Gebruik SSH om u aan te melden bij deze machine en bevestig dat deze actief is.

## <a name="cleaning-up"></a>Opschonen

Schoon de resources die tijdens deze snelstart zijn gemaakt op door de resourcegroep met de CLI te verwijderen.

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a>De code uitgesplitst

Wat de snelstartcode doet, wordt uitgesplitst in een blok variabelen en verschillende kleine functies die hier allemaal worden behandeld.

### <a name="variables-constants-and-types"></a>Variabelen, constanten en typen

Aangezien dit een zelfstandige snelstart is, maakt deze gebruik van globale constanten en variabelen.

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

Er worden waarden opgegeven waardoor de gemaakte resources hun naam krijgen. De locatie wordt hier ook opgegeven. Deze kunt u aanpassen om te zien hoe implementaties zich in andere datacenters gedragen. Niet elk datacenter beschikt over alle vereiste resources.

Het type `clientInfo` is gedeclareerd om alle informatie in te kapselen die onafhankelijk van het verificatiebestand moet worden geladen om clients in de SDK en het wachtwoord van de virtuele machine in te stellen.

De constanten `templateFile` en `parametersFile` verwijzen naar de bestanden die nodig zijn voor de implementatie. De `authorizer` wordt geconfigureerd door de Go-SDK voor verificatie en de variabele `ctx` is een [Go-context](https://blog.golang.org/context) voor de netwerkbewerkingen.

### <a name="authentication-and-initialization"></a>Verificatie en initialisatie

De `init`-functie stelt de verificatie in. Aangezien verificatie de hoofdvoorwaarde is voor alles in de snelstart, is het verstandig om dit als onderdeel van de initialisatie te gebruiken. Ook wordt er bepaalde informatie uit het verificatiebestand geladen die nodig is om clients en de virtuele machine te configureren.

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

Eerst wordt [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) aangeroepen om de verificatie-informatie te laden uit het bestand dat zich in `AZURE_AUTH_LOCATION` bevindt. Vervolgens wordt dit bestand handmatig geladen door de functie `readJSON` (hier weggelaten) om de twee waarden op te halen die nodig zijn om de rest van het programma uit te voeren: de abonnements-id van de client en het geheim van de service-principal dat ook wordt gebruikt voor het wachtwoord van de virtuele machine.

> [!WARNING]
> Als u de snelstart eenvoudig wilt houden, wordt het wachtwoord van de service-principal opnieuw gebruikt. Let erop dat u __nooit__ een wachtwoord dat toegang geeft tot uw Azure-resources opnieuw gebruikt.

### <a name="flow-of-operations-in-main"></a>Bewerkingsstroom in main()

De functie `main` is eenvoudig: deze geeft enkel de bewerkingsstroom aan en voert een foutencontrole uit.

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

De stappen die door de code worden uitgevoerd, zijn in volgorde:

* De resourcegroep maken om te implementeren in (`createGroup`)
* De implementatie in deze groep maken (`createDeployment`)
* De aanmeldingsgegevens voor de geïmplementeerde VM verkrijgen en weergeven (`getLogin`)

### <a name="creating-the-resource-group"></a>De resourcegroep maken

De functie `createGroup` maakt de resourcegroep. Als u naar de aanroepstroom en argumenten kijkt, ziet u de manier waarop service-interacties binnen de SDK zijn gestructureerd.

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

De algemene interactiestroom met een Azure-service is:

* Maak de client die gebruikmaakt van de methode `service.New*Client()`, waarbij `*` het resourcetype is van de `service` waarmee u wilt communiceren. Deze functie haalt altijd een abonnements-id op.
* Stel de autorisatiemethode voor de client in, waardoor deze kan communiceren met de externe API.
* Laat de methode de client aanroepen die overeenkomt met de externe API. De methoden van serviceclients gebruiken doorgaans de naam van de resource en een metagegevensobject.

De functie [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) wordt gebruikt om een typeconversie uit te voeren. De parameters voor methoden van de SDK gebruiken bijna exclusief aanwijzers, dus worden er methoden voor gebruiksgemak gebruikt om de typeconversie eenvoudig te maken. Raadpleeg de documentatie voor de module [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) voor de volledige lijst met eenvoudige conversieprogramma's en hun gedrag.

De methode `groupsClient.CreateOrUpdate` retourneert een aanwijzer naar een gegevenstype die de gehele resourcegroep vertegenwoordigt. Een rechtstreeks geretourneerde waarde van dit type geeft een korte bewerking aan die synchroon moet zijn. In de volgende sectie ziet u een voorbeeld van een langdurige bewerking en ziet u hoe u daarmee communiceert.

### <a name="performing-the-deployment"></a>De implementatie uitvoeren

Nadat de resourcegroep is gemaakt, is het tijd om de implementatie uit te voeren. Deze code wordt opgedeeld in kleinere secties om nadruk te leggen op verschillende onderdelen van de logica ervan.

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

De implementatiebestanden worden geladen door `readJSON`, waarvan de details in deze zelfstudie worden overgeslagen. Deze functie retourneert een `*map[string]interface{}`, het type dat wordt gebruikt om de metagegevens van de aanroep van de resource-implementatie te bouwen. Het wachtwoord van de virtuele machine wordt ook handmatig ingesteld op de implementatieparameters.

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

Deze code volgt hetzelfde patroon als het maken van de resourcegroep. Er wordt een nieuwe client gemaakt die kan verifiëren met Azure, waarna een methode wordt aangeroepen.
De methode heeft zelfs dezelfde naam (`CreateOrUpdate`) als de bijbehorende methode voor resourcegroepen. Dit patroon is zichtbaar in de gehele SDK.
Methoden die gelijksoortig werk uitvoeren, hebben doorgaans dezelfde naam.

Het grootste verschil zit in de geretourneerde waarde van de methode `deploymentsClient.CreateOrUpdate`. Deze waarde is een type [vertraging](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future), dat het [vertragingsontwerppatroon](https://en.wikipedia.org/wiki/Futures_and_promises) volgt. Vertragingen vertegenwoordigen een langdurige bewerking in Azure die u kunt controleren, annuleren of blokkeren bij de voltooiing.

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

Voor dit voorbeeld is het beste om te wachten tot de bewerking is voltooid. Voor het wachten op een vertraging is zowel een [contextobject](https://blog.golang.org/context) nodig als de client die de `Future` heeft gemaakt. Er zijn hier twee mogelijke foutbronnen: een fout die wordt veroorzaakt aan de clientzijde wanneer de methode wordt aangeroepen en een foutmelding vanaf de server. De laatste wordt geretourneerd als onderdeel van de aanroep `deploymentFuture.Result`.

Zodra de implementatie-informatie is opgehaald, is er een tijdelijke oplossing voor mogelijke fouten waarbij de implementatie-informatie mogelijk leeg is met een handmatige aanroep naar `deploymentsClient.Get` om ervoor te zorgen dat de gegevens worden ingevuld.

### <a name="obtaining-the-assigned-ip-address"></a>Het toegewezen IP-adres verkrijgen

Als u iets wilt doen met de nieuw gemaakte VM, hebt u het toegewezen IP-adres nodig. IP-adressen zijn afzonderlijke Azure-resources en gebonden aan Network Interface Controller-resources (NIC).

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

Deze methode is gebaseerd op informatie die in het parameterbestand wordt opgeslagen. De code kan rechtstreeks een query uitvoeren op de VM om de NIC op te halen, de query uitvoeren op de NIC om de IP-resource op te halen en vervolgens rechtstreeks een query uitvoeren op de IP-resource. Dat is een lange reeks aan afhankelijkheden en op te lossen bewerkingen, waardoor de uitvoering ervan duur wordt. Aangezien de JSON-informatie lokaal is, kan deze worden geladen.

De waarde voor de gebruiker van de virtuele machine wordt ook geladen vanuit de JSON. Het wachtwoord van de virtuele machine is eerder geladen vanuit het verificatiebestand.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u een bestaande sjabloon gebruikt en deze via Go geïmplementeerd. U hebt vervolgens via SSH verbinding gemaakt met de nieuw gemaakte VM om te controleren of deze werd uitgevoerd.

Als u meer te weten wilt komen over werken met virtuele machines in de Azure-omgeving met Go, kunt u de [Azure-berekeningsvoorbeelden voor Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) of de [voorbeelden van Azure-resourcebeheer voor Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources) bekijken.

Zie [Verificatie met de Azure SDK voor Go](azure-sdk-go-authorization.md) voor meer informatie over de beschikbare verificatiemethoden in de SDK en welke verificatietypen ze ondersteunen.

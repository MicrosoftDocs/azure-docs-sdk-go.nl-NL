---
title: Een virtuele Azure-machine vanuit Go implementeren
description: Een virtuele machine implementeren met de Azure SDK voor Go.
keywords: azure, virtuele machine, vm, go, golang, azure sdk
author: sptramer
ms.author: sttramer
ms.date: 02/08/2018
ms.topic: quickstart
ms.devlang: go
manager: routlaw
ms.openlocfilehash: e530d944deca40e9e6c29b6c2768e2367822714e
ms.sourcegitcommit: aaa8c37880332625f858a38f5918e6cf581bf48d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a>Snelstartgids: een virtuele Azure-machine implementeren vanuit een sjabloon met de Azure SDK voor Go

Deze snelstartgids is gericht op de implementatie van resources vanuit een sjabloon met de Azure SDK voor Go. Sjablonen zijn momentopnames van alle resources die in een [Azure-resourcegroep](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) zijn opgenomen. Gaandeweg raakt u bekend met de functionaliteiten en conventies van de SDK en ondertussen voert u een handige taak uit.

Aan het einde van deze snelstartgids voert u een VM uit waarbij u zich aanmeldt met een gebruikersnaam en een wachtwoord.

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

Als u een lokale installatie van Azure CLI gebruikt, vereist deze snelstartgids CLI-versie 2.0.24 of hoger. Voer `az --version` uit om te controleren of de installatie van uw CLI voldoet aan deze vereiste. Als u uw CLI wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli).

## <a name="install-the-azure-sdk-for-go"></a>De Azure SDK voor Go installeren 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a>Een service-principal maken

Als u zich niet-interactief aan wilt melden bij een toepassing, hebt u een service-principal nodig. Service-principals zijn onderdeel van op rollen gebaseerde verificatie (RBAC), waarmee een unieke gebruikersidentiteit wordt gemaakt. Als u een nieuwe service-principal wilt maken met de CLI, voert u de volgende opdracht uit:

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

__Zorg ervoor__ dat u de waarden `appId`, `password` en `tenant` in de uitvoer vastlegt. Deze waarden worden door de toepassing gebruikt om te verifiëren bij Azure.

Bekijk [Een Azure-service-principal maken met de Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli) voor meer informatie over het maken en beheren van service-principals met Azure CLI 2.0.

## <a name="get-the-code"></a>Code ophalen

Haal de snelstartcode en alle afhankelijkheden ervan op met `go get`.

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

Deze code wordt gecompileerd, maar wordt niet goed uitgevoerd tot u deze de informatie levert over uw Azure-account en de gemaakte service-principal. In `main.go` is er een variabele, `config`, waarin zich een `authInfo`-struct bevindt. In de struct moeten de veldwaarden worden vervangen om ervoor te zorgen dat de verificatie correct werkt.

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* `SubscriptionID`: uw abonnements-id die met de CLI-opdracht kan worden verkregen

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* `TenantID`: uw tenant-id, de waarde `tenant` die wordt geregistreerd wanneer u de service-principal maakt
* `ServicePrincipalID`: de waarde `appId` die word geregistreerd wanneer u de service-principal maakt
* `ServicePrincipalSecret`: de waarde `password` die word geregistreerd wanneer u de service-principal maakt

U moet ook een waarde in het bestand `vm-quickstart-params.json` aanpassen.

```json
    "vm_password": {
        "value": "_"
    }
```

* `vm_password`: het wachtwoord van het gebruikersaccount van de VM. Het moet uit 6-72 tekens bestaan en het moet 3 van de volgende tekens bevatten:
  * Een kleine letter
  * Een hoofdletter
  * Een getal
  * Een symbool

## <a name="running-the-code"></a>De code uitvoeren

Voer de snelstart uit met de opdracht `go run`.

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

Als er in de implementatie iets fout gaat, krijgt u een bericht waarin staat dat er een probleem was, maar er staat geen specifieke informatie in. U kunt met de Azure CLI de details van de implementatiefout ophalen met behulp van de volgende opdracht:

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

### <a name="variable-assignments-and-structs"></a>Variabeletoewijzingen en structs

Aangezien de snelstartcode zelfstandig is, worden er globale variabelen gebruikt in plaats van opdrachtregelopties of omgevingsvariabelen.

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

De struct `authInfo` wordt opgegeven om alle informatie in te kapselen die nodig is voor de autorisatie met Azure-services.

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

Er worden waarden opgegeven waardoor de gemaakte resources hun naam krijgen. De locatie wordt hier ook opgegeven. Deze kunt u aanpassen om te zien hoe implementaties zich in andere datacenters gedragen. Niet elk datacenter beschikt over alle vereiste resources.

De constanten `templateFile` en `parametersFile` verwijzen naar de bestanden die nodig zijn voor de implementatie. De token van de service-principal wordt later behandeld en de variabele `ctx` is een [Go-context](https://blog.golang.org/context) voor de netwerkbewerkingen.

### <a name="init-and-authorization"></a>init() en autorisatie

De methode `init()` voor de code stelt autorisatie in. Aangezien autorisatie de hoofdvoorwaarde is voor alles in de snelstart, is het verstandig om dit als onderdeel van de initialisatie te gebruiken. 

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

Deze code voltooit twee stappen voor autorisatie:

* OAuth-configuratiegegevens voor de `TenantID` worden opgehaald tijdens de communicatie met Azure Active Directory. Het object [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) bevat de eindpunten die worden gebruikt in de standaard Azure-configuratie.
* De functie [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) wordt aangeroepen. Deze functie haalt de OAuth-gegevens samen met de referenties van de service-principal op, evenals de stijl van het gebruikte Azure-beheer. Tenzij u specifieke vereisten hebt en u weet wat u doet, zou deze waarde altijd `.ResourceManagerEndpoint` moeten zijn.

### <a name="flow-of-operations-in-main"></a>Bewerkingsstroom in main()

De functie `main()` is eenvoudig: deze geeft enkel de bewerkingsstroom aan en voert een foutencontrole uit.

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

De stappen die door de code worden uitgevoerd, zijn in volgorde:

* De resourcegroep maken om te implementeren in (`createGroup()`)
* De implementatie in deze groep maken (`createDeployment()`)
* De aanmeldingsgegevens voor de geïmplementeerde VM verkrijgen en weergeven (`getLogin()`)

### <a name="creating-the-resource-group"></a>De resourcegroep maken

De functie `createGroup()` maakt de resourcegroep. Als u naar de aanroepstroom en argumenten kijkt, ziet u de manier waarop service-interacties binnen de SDK zijn gestructureerd.

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

De algemene interactiestroom met een Azure-service is:

* Maak de client die gebruikmaakt van de methode `service.NewXClient()`, waarbij `X` het resourcetype is van de `service` waarmee u wilt communiceren. Deze functie haalt altijd een abonnements-id op.
* Stel de autorisatiemethode voor de client in, waardoor deze kan communiceren met de externe API.
* Laat de methode de client aanroepen die overeenkomt met de externe API. De methoden van serviceclients gebruiken doorgaans de naam van de resource en een metagegevensobject.

De functie [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) wordt gebruikt om een typeconversie uit te voeren. De parameterstructs voor methoden van de SDK gebruiken bijna exclusief aanwijzers, dus deze methoden worden gebruikt om de typeconversie eenvoudig te maken. Raadpleeg de documentatie voor de module [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) voor de volledige lijst en het gedrag van eenvoudige conversieprogramma's.

De bewerking `groupsClient.CreateOrUpdate()` retourneert een aanwijzer naar een gegevensstruct die de gehele resourcegroep vertegenwoordigt. Een rechtstreeks geretourneerde waarde van dit type geeft een korte bewerking aan die synchroon moet zijn. In de volgende sectie ziet u een exemplaar van een langdurige bewerking en ziet u hoe u daarmee communiceert.

### <a name="performing-the-deployment"></a>De implementatie uitvoeren

Nadat de groep is gemaakt waarin de resources worden bewaard, is het tijd om de implementatie uit te voeren. Deze code wordt opgedeeld in kleinere secties om nadruk te leggen op verschillende onderdelen van de logica ervan.


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

De implementatiebestanden worden geladen door `readJSON`, waarvan de details in deze zelfstudie worden overgeslagen. Deze functie retourneert een `*map[string]interface{}`, het type dat wordt gebruikt om de metagegevens van de aanroep van de resource-implementatie te bouwen.

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

Deze code volgt hetzelfde patroon als het maken van de resourcegroep. Er wordt een nieuwe client gemaakt die kan verifiëren met Azure, waarna een methode wordt aangeroepen. De methode heeft zelfs dezelfde naam (`CreateOrUpdate`) als de bijbehorende methode voor resourcegroepen. Dit patroon komt steeds opnieuw terug in de SDK. Methoden die gelijksoortig werk uitvoeren, hebben doorgaans dezelfde naam.

Het grootste verschil zit in de geretourneerde waarde van de methode `deploymentsClient.CreateOrUpdate()`. Deze waarde is een `Future`-object, dat het [vertragingsontwerppatroon](https://en.wikipedia.org/wiki/Futures_and_promises) volgt. Vertragingen vertegenwoordigen een langdurige bewerking in Azure en het is handig deze zo nu en dan te peilen terwijl u ander werk uitvoert.

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

Voor dit voorbeeld is het beste om te wachten tot de bewerking is voltooid. Voor het wachten op een vertraging is zowel een [contextobject](https://blog.golang.org/context) nodig als de client die het vertragingsobject heeft gemaakt. Er zijn hier twee mogelijke foutbronnen: een fout die wordt veroorzaakt aan de clientzijde wanneer de methode wordt aangeroepen en een foutmelding vanaf de server. De laatste wordt geretourneerd als onderdeel van de aanroep `deploymentFuture.Result()`.

### <a name="obtaining-the-assigned-ip-address"></a>Het toegewezen IP-adres verkrijgen

Als u iets wilt doen met de nieuw gemaakte VM, hebt u het toegewezen IP-adres nodig. IP-adressen zijn afzonderlijke Azure-resources en gebonden aan Network Interface Controller-resources (NIC).

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

Deze methode is gebaseerd op informatie die in het parameterbestand wordt opgeslagen. De code kan rechtstreeks een query uitvoeren op de VM om de NIC op te halen, de query uitvoeren op de NIC om de IP-resource op te halen en vervolgens rechtstreeks een query uitvoeren op de IP-resource. Dat is een lange reeks aan afhankelijkheden en op te lossen bewerkingen, waardoor de uitvoering ervan duur wordt. Aangezien de JSON-informatie lokaal is, kan deze worden geladen.

De waarden voor de gebruiker en het wachtwoord van de VM worden eveneens vanuit de JSON geladen.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u een bestaande sjabloon gebruikt en deze via Go geïmplementeerd. U hebt vervolgens via SSH verbinding gemaakt met de nieuw gemaakte VM om te controleren of deze werd uitgevoerd.

Als u meer te weten wilt komen over werken met virtuele machines in de Azure-omgeving met Go, kunt u de [Azure-berekeningsvoorbeelden voor Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) of de [voorbeelden van Azure-resourcebeheer voor Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources) bekijken.

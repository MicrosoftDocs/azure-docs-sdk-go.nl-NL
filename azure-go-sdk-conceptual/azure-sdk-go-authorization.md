---
title: Verificatie met de Azure SDK voor Go
description: Lees meer over de verificatiemethoden die beschikbaar zijn in de Azure SDK voor Go en hoe u ze gebruikt.
services: azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 04/03/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.service: active-directory
ms.component: authentication
ms.openlocfilehash: c7970167070bdf1f3fc75692f3e34268801c65df
ms.sourcegitcommit: 181d4e0b164cf39b3feac346f559596bd19c94db
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38066996"
---
# <a name="authentication-methods-in-the-azure-sdk-for-go"></a>Verificatiemethoden in de Azure SDK voor Go

De Azure SDK voor Go biedt tal van verificatietypen en -methoden die in uw toepassing kunnen worden gebruikt. Ondersteunde verificatiemethoden variëren van het ophalen van gegevens uit omgevingsvariabelen tot interactieve webgebaseerde verificatie. In dit artikel vindt u de beschikbare typen verificatie in de SDK en de methoden voor het gebruik ervan. U maakt ook kennis met aanbevolen procedures die u helpen te bepalen welk verificatietype geschikt is voor uw toepassing.

## <a name="available-authentication-types-and-methods"></a>Beschikbare verificatietypen en -methoden

De Azure SDK voor Go biedt verschillende typen verificatie met verschillende referentiesets. Al deze verificatietypen zijn beschikbaar via verschillende verificatiemethoden, die bepalen hoe SDK deze referenties gebruikt als invoer. In de volgende tabel worden de beschikbare typen verificatie beschreven, evenals situaties waarin ze worden aanbevolen voor uw toepassing.

| Verificatietype | Aanbevolen |
|---------------------|---------------------|
| Verificatie op basis van certificaat | U hebt een X509-certificaat dat is geconfigureerd voor een AAD-gebruiker (Azure Active Directory) of service-principal. Zie [Aan de slag met verificatie op basis van certificaten in Azure Active Directory] voor meer informatie. |
| Clientreferenties | U hebt een geconfigureerde service-principal die is ingesteld voor deze toepassing of een klasse toepassingen waar deze onderdeel van uitmaakt. Zie [Een service-principal maken met Azure CLI 2.0] voor meer informatie. |
| Managed Service Identity (MSI) | Uw toepassing wordt uitgevoerd op een Azure-resource die is geconfigureerd met Managed Service Identity (MSI). Zie [Managed Service Identity (MSI) voor Azure-resources] voor meer informatie. |
| Apparaattoken | Uw toepassing is __uitsluitend__ bedoeld om interactief te worden gebruikt en heeft een groot aantal gebruikers, die mogelijk afkomstig zijn van meerdere AAD-tenants. Gebruikers hebben toegang tot een webbrowser om zich aan te melden. Zie [Apparaattokenverificatie gebruiken](#use-device-token-authentication) voor meer informatie.|
| Gebruikersnaam en wachtwoord | U hebt een interactieve toepassing die geen gebruik van maken van een andere verificatiemethode. Multi-Factor Authentication is niet ingeschakeld voor de AAD-aanmelding van uw gebruikers. |

> [!IMPORTANT]
> Als u een ander verificatietype dan clientreferenties gebruikt, moet uw toepassing worden geregistreerd in Azure Active Directory. Zie [Toepassingen integreren met Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications) voor meer informatie.

> [!NOTE]
> Voorkom verificatie aan de hand van gebruikersnaam en wachtwoord, tenzij u speciale vereisten hebt. In situaties waarin aanmelding op basis van gebruikers geschikt is, kan doorgaans in plaats hiervan apparaattokenverificatie worden gebruikt.

[Aan de slag met verificatie op basis van certificaten in Azure Active Directory]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Een service-principal maken met Azure CLI 2.0]: /cli/azure/create-an-azure-service-principal-azure-cli
[Managed Service Identity (MSI) voor Azure-resources]: /azure/active-directory/managed-service-identity/overview

Deze verificatietypen zijn beschikbaar via verschillende methoden. Met [_verificatie op basis van de omgeving_](#use-environment-based-authentication) worden referenties rechtstreeks uit de omgeving van het programma gelezen. Met [_verificatie op basis van het bestand_](#use-file-based-authentication) wordt er een bestand geladen met de referenties van de service-principal. Met [_verificatie op basis van de client_](#use-an-authentication-client) wordt er gebruikgemaakt van een object in Go-code en bent u verantwoordelijk voor de levering van de referenties wanneer het programma wordt uitgevoerd. Met [_apparaattokenverificatie_](#use-device-token-authentication) moeten gebruikers zich interactief met een token aanmelden via een webbrowser. Deze methode kan niet worden gebruikt met omgevings- of bestandsgebaseerde verificatie.

Alle verificatiefuncties en -typen zijn beschikbaar in het `github.com/Azure/go-autorest/autorest/azure/auth`-pakket.

> [!NOTE]
> Voorkom clientgebaseerde verificatie, tenzij u speciale vereisten hebt. Deze verificatiemethode werkt onjuiste handelwijzen in de hand. Vooral clientgebaseerde verificatie maakt het verleidelijk om referenties vast te leggen. Het schrijven van aangepaste code voor verificatie kan ook worden onderbroken bij het gebruik van toekomstige SDK-versies als de verificatievereisten veranderen.

## <a name="use-environment-based-authentication"></a>Verificatie op basis van de omgeving gebruiken

Als uw toepassing wordt uitgevoerd in een strak beheerde omgeving, zoals in een container, is verificatie op basis van de omgeving is een logische keuze. U kunt de shell-omgeving configureren voordat u uw toepassing uitvoert en de Go SDK deze omgevingsvariabelen tijdens runtime leest om te verifiëren bij Azure. 

Verificatie op basis van de omgeving biedt ondersteuning voor alle verificatiemethoden behalve apparaattokens, die in de volgende volgorde worden geëvalueerd: clientreferenties, certificaten, gebruikersnaam en wachtwoord en Managed Service Identity (MSI). Als een vereiste omgevingsvariabele is uitgeschakeld of de SDK een weigering van de verificatieservice ontvangt, wordt het volgende verificatietype geprobeerd. Als de SDK kan niet worden geverifieerd vanuit de omgeving, wordt er een fout geretourneerd.

In de volgende tabel worden de omgevingsvariabelen vermeld die moeten worden ingesteld voor elk verificatietype dat wordt ondersteund door de verificatie op basis van de omgeving.

| Verificatietype | Omgevingsvariabele | Beschrijving |
| ------------------- | -------------------- | ----------- |
| __Clientreferenties__ | `AZURE_TENANT_ID` | De id voor de Active Directory-tenant waar de service-principal bij hoort. |
| | `AZURE_CLIENT_ID` | De naam of id van de service-principal. |
| | `AZURE_CLIENT_SECRET` | Het geheim dat is gekoppeld aan de service-principal. |
| __Certificaat__ | `AZURE_TENANT_ID` | De id voor de Active Directory-tenant waarvoor het certificaat is geregistreerd. |
| | `AZURE_CLIENT_ID` | De client-id van de toepassing die is gekoppeld aan het certificaat. |
| | `AZURE_CERTIFICATE_PATH` | Het pad naar het clientcertificaatbestand. |
| | `AZURE_CERTIFICATE_PASSWORD` | Het wachtwoord voor het clientcertificaat. |
| __Gebruikersnaam en wachtwoord__ | `AZURE_TENANT_ID` | De id voor de Active Directory-tenant waar de gebruiker bij hoort. |
| | `AZURE_CLIENT_ID` | De client-id voor de toepassing. |
| | `AZURE_USERNAME` | De gebruikersnaam waarmee moet worden aangemeld. |
| | `AZURE_PASSWORD` | Het wachtwoord waarmee moet worden aangemeld. |
| __MSI__ | | Voor MSI hoeven geen referenties te worden ingesteld. De toepassing moet worden uitgevoerd op een Azure-resource die is geconfigureerd voor het gebruik van MSI. Zie [Managed Service Identity (MSI) voor Azure-resources] voor meer informatie. |

Als u verbinding moet maken met een andere cloud of een ander beheereindpunt dan de standaard openbare Azure-cloud, kunt u ook de volgende omgevingsvariabelen instellen. De meestvoorkomende redenen om deze in te stellen, zijn als u Azure Stack, een cloud in een andere geografische regio of het klassieke Azure-implementatiemodel gebruikt.

| Omgevingsvariabele | Beschrijving  |
|----------------------|--------------|
| `AZURE_ENVIRONMENT` | De naam van de cloudomgeving waarmee verbinding wordt gemaakt. |
| `AZURE_AD_RESOURCE` | De Active Directory-resource-id die moet worden gebruikt om verbinding te maken. Dit moet een URI zijn die verwijst naar het beheereindpunt. |

Wanneer u gebruikmaakt van verificatie op basis van de omgeving, roept u de functie [NewAuthorizerFromEnvironment](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromEnvironment) aan om uw verificatieobject op te halen. Dit object wordt vervolgens ingesteld op de `Authorizer`-eigenschap van clients zodat deze toegang krijgen tot Azure.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := auth.NewAuthorizerFromEnvironment()
```

### <a name="authentication-on-azure-stack"></a>Verificatie in Azure Stack

Als u verificatie wilt uitvoeren in Azure Stack, moet u de volgende variabelen instellen:

| Omgevingsvariabele | Beschrijving  |
|----------------------|--------------|
| `AZURE_AD_ENDPOINT` | Het Azure Active Directory-eindpunt. |
| `AZURE_AD_RESOURCE` | De resource-id voor Active Directory. |

Deze variabelen kunnen worden opgehaald uit de Azure Stack-metagegevens. Als u de metagegevens wilt ophalen, opent u een webbrowser in uw Azure Stack-omgeving en volgt u deze URL: `(ResourceManagerURL)/metadata/endpoints?api-version=1.0`

De `ResourceManagerURL` varieert op basis van de regionaam, de computernaam en de externe volledig gekwalificeerde domeinnaam (FQDN) van uw Azure Stack-implementatie:

| Omgeving | ResourceManagerURL |
|----------------------|--------------|
| Development Kit | `https://management.local.azurestack.external/` |
| Geïntegreerde systemen | `https://management.(region).ext-(machine-name).(FQDN)` |

Voor meer informatie over het gebruik van de Azure-SDK voor Go in Azure Stack raadpleegt u [Use API version profiles with Go in Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-version-profiles-go) (API-versieprofielen met Go gebruiken in Azure Stack).


## <a name="use-file-based-authentication"></a>Verificatie op basis van een bestand gebruiken

Verificatie op basis van een bestand werkt alleen met clientreferenties wanneer deze zijn opgeslagen in een lokale bestandsindeling die is gegenereerd door [Azure CLI 2.0](/cli/azure). U kunt dit bestand gemakkelijk maken wanneer u een nieuwe service-principal met de parameter `--sdk-auth` maakt. Als u van plan bent verificatie op basis van een bestand te gebruiken, zorgt u ervoor dat dit argument wordt opgegeven bij het maken van een service-principal. Aangezien de CLI de uitvoer afdrukt naar `stdout`, moet de uitvoer worden omgeleid naar een bestand.

```azurecli
az ad sp create-for-rbac --sdk-auth > azure.auth
```

Stel de omgevingsvariabele `AZURE_AUTH_LOCATION` in op waar het verificatiebestand zich bevindt. Deze omgevingsvariabele wordt gelezen door de toepassing en de aanwezige referenties worden geparseerd. Als u het verficatiebestand tijdens runtime moet selecteren, bewerkt u de omgeving van het programma met behulp van de [os.SETENV](https://golang.org/pkg/os/#Setenv)-functie.

De functie [NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) moet worden aangeroepen om de verificatiegegevens te laden. In tegenstelling tot verificatie op basis van de omgeving is voor verificatie op basis van een bestand een resource-eindpunt vereist.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
```

Zie [Een service-principal maken met Azure CLI 2.0] voor meer informatie over het gebruik van service-principals en het beheer van de toegangsmachtigingen.

## <a name="use-device-token-authentication"></a>Apparaattokenverificatie gebruiken

Als u wilt dat gebruikers zich interactief aanmelden, is apparaattokenverificatie de beste manier om dit mogelijk te maken. Deze verificatiestroom geeft gebruikers een token die in een aanmeldsite van Microsoft kan worden geplakt, waar ze zich vervolgens verifiëren met een AAD-account (Azure Active Directory). Deze verificatiemethode biedt ondersteuning voor accounts waarvoor Multi-Factor Authentication is ingeschakeld, in tegenstelling tot de standaardverificatie met gebruik van gebruikersnaam en wachtwoord.

Als u apparaattokenverificatie wilt gebruiken, maakt u een [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig)-authorizer met de functie [NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig). Roep [Authorizer](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer) aan op het resulterende object om het verificatieproces te starten. Bij apparaatstroomverificatie wordt het programma niet uitgevoerd totdat de gehele verificatiestroom is voltooid.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
deviceConfig := auth.NewDeviceFlowConfig(applicationID, tenantID)
authorizer, err := deviceConfig.Authorizer()
```

## <a name="use-an-authentication-client"></a>Een verificatieclient gebruiken

Als u een specifiek type verificatie vereist en bereid bent om het programma het werk te laten doen om de verificatiegegevens van de gebruiker te laden, kunt u een willekeurige client gebruiken die voldoet aan de [auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig)-interface. Gebruik een type waarmee deze interface wordt geïmplementeerd, als u een interactief programma wilt, gebruik gespecialiseerde configuratiebestanden of gebruik een vereiste waarmee u voorkomt dat er een andere verificatiemethode wordt gebruikt.

> [!WARNING]
> Leg Azure-referenties nooit vast in een toepassing. Als geheimen in een binair bestand voor een toepassing worden opgeslagen, is het eenvoudiger voor een aanvaller om deze uit te pakken, ongeacht of de toepassing wordt uitgevoerd. Hiermee lopen alle Azure-resources waarvoor de referenties worden geverifieerd risico.

De volgende tabel bevat de typen in de SDK die voldoen aan de `AuthorizerConfig`-interface.

| Verificatietype | Type authorizer |
|---------------------|-----------------------|
| Verificatie op basis van certificaat | [ClientCertificateConfig] |
| Clientreferenties | [ClientCredentialsConfig] |
| Managed Service Identity (MSI) | [MSIConfig] |
| Gebruikersnaam en wachtwoord | [UsernamePasswordConfig] |

[ClientCertificateConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCertificateConfig
[ClientCredentialsConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCredentialsConfig
[MSIConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#MSIConfig
[DeviceFlowConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig
[UsernamePasswordConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#UsernamePasswordConfig

Maak een verificator met de bijbehorende functie `New` en roep vervolgens `Authorize` aan op het resulterende object om de verificatie uit te voeren. Als u bijvoorbeeld verificatie op basis van een certificaat wilt gebruiken:

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
certificateAuthorizer := auth.NewClientCertificateConfig(certificatePath, certificatePassword, clientID, tenantID)
authorizerToken, err := certificateAuthorizer.Authorize()
```

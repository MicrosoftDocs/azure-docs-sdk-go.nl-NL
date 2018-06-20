---
title: De Azure SDK voor Go installeren
description: De Azure SDK voor Go installeren, configureren en in de map Vendor bewaren.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/14/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 3388359bba791c87025b6ffd0e6b476f95589f73
ms.sourcegitcommit: 81e97407e6139375bf7357045e818c87a17dcde1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36262973"
---
# <a name="install-the-azure-sdk-for-go"></a>De Azure SDK voor Go installeren

Welkom bij de Azure SDK voor Go. Met de SDK kunt u Azure-services beheren en ermee communiceren vanuit uw Go-toepassingen.

## <a name="get-the-azure-sdk-for-go"></a>De Azure SDK voor Go ophalen

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

Sommige Azure-services hebben hun eigen SDK voor Go en zijn niet opgenomen in het basispakket Azure SDK voor Go. De volgende tabel toont de services met hun eigen SDK's en hun pakketnamen. Deze pakketten worden allemaal beschouwd als Preview-versie.

| Service | Pakket |
|---------|---------|
| Blob Storage | [github.com/Azure/azure-storage-blob-go](https://github.com/Azure/azure-storage-blob-go) |
| File Storage | [github.com/Azure/azure-storage-file-go](https://github.com/Azure/azure-storage-file-go) |
| Opslagwachtrij | [github.com/Azure/azure-storage-queue-go](https://github.com/Azure/azure-storage-queue-go) |
| Event Hub | [github.com/Azure/azure-event-hubs-go](https://github.com/Azure/azure-event-hubs-go) |
| Service Bus | [github.com/Azure/azure-service-bus-go](https://github.com/Azure/azure-service-bus-go) |
| Application Insights | [github.com/Microsoft/ApplicationInsights-go](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a>De Azure SDK voor Go bewaren in Vendor

Het is mogelijk om de Azure SDK voor Go in de map Vendor te bewaren via [dep](https://github.com/golang/dep). Ten behoeve van de stabiliteit is het aanbevolen om Vendor te gebruiken. Als u ondersteuning voor `dep` wilt gebruiken, voegt u `github.com/Azure/azure-sdk-for-go` aan een `[[constraint]]`-sectie van uw `Gopkg.toml`. Als u bijvoorbeeld gebruik wilt maken van Vendor op versie `14.0.0`, voegt u de volgende vermelding toe:

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a>De Azure SDK voor Go opnemen in uw project

Als u Azure-services vanuit uw Go-code wilt gebruiken, importeert u de services waarmee u communiceert en de vereiste `autorest`-modules.
U krijgt een volledige lijst te zien van de beschikbare modules van GoDoc voor [beschikbare services](https://godoc.org/github.com/Azure/azure-sdk-for-go) en [AutoRest-pakketten](https://godoc.org/github.com/Azure/go-autorest). De meestvoorkomende pakketten die u van `go-autorest` nodig hebt, zijn:

| Pakket | Beschrijving |
|---------|-------------|
| [github.com/Azure/go-autorest/autorest][autorest] | Objecten om de service-clientverificatie te verwerken |
| [github.com/Azure/go-autorest/autorest/azure][autorest/azure] | Constanten voor interactie met Azure-services |
| [github.com/Azure/go-autorest/autorest/adal][autorest/adal] | Verificatiemechanismen voor toegang tot Azure-services |
| [github.com/Azure/go-autorest/autorest/to][autorest/to] | Type assertiehulpprogramma's om met Azure SDK-gegevensstructuren te werken |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

Modules voor Azure-services krijgen afzonderlijk van hun SDK-API een versienummer. Deze versies zijn onderdeel van het importpad van de module en zijn afkomstig van een _serviceversie_ of een _profiel_. Het is momenteel aanbevolen dat u een specifieke serviceversie gebruikt voor zowel de ontwikkeling als de release. Services bevinden zich in de module `services`. Het volledige pad voor importeren is de naam van de service, gevolgd door de versie in de indeling `YYYY-MM-DD` en weer gevolgd door de naam van de service. Als u bijvoorbeeld de versie van `2017-03-30` wilt opnemen in de Compute-service, gebruikt u:

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

Het is momenteel aanbevolen dat u de laatste versie van een service gebruikt, tenzij u een goede reden hebt om dat niet te doen.

U kunt ook één profielversie selecteren als u een collectieve momentopname van de services nodig hebt. Het enige vergrendelde profiel is momenteel `2017-03-09`, dat wellicht niet over de nieuwste functies of services beschikt. Profielen bevinden zich in de module `profiles` met de versie in de indeling `YYYY-MM-DD`. Services worden op basis van hun profielversie gegroepeerd. Als u bijvoorbeeld de Azure Resources-beheermodule wilt importeren uit het profiel `2017-03-09`, gebruikt u:

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> De profielen `preview` en `latest` zijn ook beschikbaar. Het gebruik ervan is niet aanbevolen. Deze profielen zijn doorlopende versies en het servicegedrag kan op elk moment veranderen.

## <a name="next-steps"></a>Volgende stappen

Gebruik een snelstartgids om aan de slag te gaan met de Azure SDK voor Go.

* [Een virtuele machine implementeren vanuit een sjabloon](azure-sdk-go-qs-vm.md)
* [Objecten overdragen naar Azure Blob-opslag met de Azure Blob SDK voor Go](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [Verbinding maken met Azure Database for PostgreSQL](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

Als u onmiddellijk aan de slag wilt gaan met andere services in de Go SDK, kunt u een aantal van de beschikbare voorbeeldcodes bekijken.

* [Verifiëren met Azure-services](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [Nieuwe virtuele machines met SSH-verificatie implementeren](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [Een containerinstallatiekopie implementeren in Azure Container Instances](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [Een cluster in Azure Kubernetes Service maken](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [Werken met Azure Storage-services](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [Alle voorbeelden voor de Azure SDK voor Go](https://github.com/azure-samples/azure-sdk-for-go-samples)

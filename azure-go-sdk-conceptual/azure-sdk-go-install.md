---
title: De Azure SDK voor Go installeren
description: De Azure SDK voor Go installeren, configureren en in de map Vendor bewaren.
author: sptramer
ms.author: sttramer
ms.date: 03/14/2018
ms.topic: article
ms.devlang: go
manager: carmonm
ms.openlocfilehash: a6a92e080aea1a92f47a9d7083f133ca05a47541
ms.sourcegitcommit: 26520a8c6e812facb5b9432d68c370fa23c99888
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="3ca6c-103">De Azure SDK voor Go installeren</span><span class="sxs-lookup"><span data-stu-id="3ca6c-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="3ca6c-104">Welkom bij de Azure SDK voor Go.</span><span class="sxs-lookup"><span data-stu-id="3ca6c-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="3ca6c-105">Met de SDK kunt u Azure-services beheren en ermee communiceren vanuit uw Go-toepassingen.</span><span class="sxs-lookup"><span data-stu-id="3ca6c-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="3ca6c-106">De Azure SDK voor Go ophalen</span><span class="sxs-lookup"><span data-stu-id="3ca6c-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="3ca6c-107">Als u met Azure Storage Blobs werkt, hebt u een afzonderlijke SDK nodig.</span><span class="sxs-lookup"><span data-stu-id="3ca6c-107">Working with Azure Storage Blobs requires a separate SDK.</span></span>

```bash
go get -u -d github.com/Azure/azure-storage-blob-go/...
```

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="3ca6c-108">De Azure SDK voor Go bewaren in Vendor</span><span class="sxs-lookup"><span data-stu-id="3ca6c-108">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="3ca6c-109">Het is mogelijk om de Azure SDK voor Go in de map Vendor te bewaren via [dep](https://github.com/golang/dep).</span><span class="sxs-lookup"><span data-stu-id="3ca6c-109">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="3ca6c-110">Ten behoeve van de stabiliteit is het aanbevolen om Vendor te gebruiken.</span><span class="sxs-lookup"><span data-stu-id="3ca6c-110">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="3ca6c-111">Als u ondersteuning voor `dep` wilt gebruiken, voegt u `github.com/Azure/azure-sdk-for-go` aan een `[[constraint]]`-sectie van uw `Gopkg.toml`.</span><span class="sxs-lookup"><span data-stu-id="3ca6c-111">In order to use `dep` support, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="3ca6c-112">Als u bijvoorbeeld gebruik wilt maken van Vendor op versie `14.0.0`, voegt u de volgende vermelding toe:</span><span class="sxs-lookup"><span data-stu-id="3ca6c-112">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="3ca6c-113">De Azure SDK voor Go opnemen in uw project</span><span class="sxs-lookup"><span data-stu-id="3ca6c-113">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="3ca6c-114">Als u Azure-services vanuit uw Go-code wilt gebruiken, importeert u de services waarmee u communiceert en de vereiste `autorest`-modules.</span><span class="sxs-lookup"><span data-stu-id="3ca6c-114">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="3ca6c-115">U krijgt een volledige lijst te zien van de beschikbare modules van GoDoc voor [beschikbare services](https://godoc.org/github.com/Azure/azure-sdk-for-go) en [AutoRest-pakketten](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="3ca6c-115">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="3ca6c-116">De meestvoorkomende pakketten die u van `go-autorest` nodig hebt, zijn:</span><span class="sxs-lookup"><span data-stu-id="3ca6c-116">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="3ca6c-117">Pakket</span><span class="sxs-lookup"><span data-stu-id="3ca6c-117">Package</span></span> | <span data-ttu-id="3ca6c-118">Beschrijving</span><span class="sxs-lookup"><span data-stu-id="3ca6c-118">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="3ca6c-119">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="3ca6c-119">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="3ca6c-120">Objecten om de service-clientverificatie te verwerken</span><span class="sxs-lookup"><span data-stu-id="3ca6c-120">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="3ca6c-121">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="3ca6c-121">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="3ca6c-122">Constanten voor interactie met Azure-services</span><span class="sxs-lookup"><span data-stu-id="3ca6c-122">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="3ca6c-123">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="3ca6c-123">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="3ca6c-124">Verificatiemechanismen voor toegang tot Azure-services</span><span class="sxs-lookup"><span data-stu-id="3ca6c-124">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="3ca6c-125">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="3ca6c-125">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="3ca6c-126">Type assertiehulpprogramma's om met Azure SDK-gegevensstructuren te werken</span><span class="sxs-lookup"><span data-stu-id="3ca6c-126">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="3ca6c-127">Modules voor Azure-services krijgen afzonderlijk van hun SDK-API een versienummer.</span><span class="sxs-lookup"><span data-stu-id="3ca6c-127">Modules for Azure services are versioned independently from the SDK APIs for them.</span></span> <span data-ttu-id="3ca6c-128">Deze versies zijn onderdeel van het importpad van de module en zijn afkomstig van een _serviceversie_ of een _profiel_.</span><span class="sxs-lookup"><span data-stu-id="3ca6c-128">These versions are part of the module import path, and are from either a _service version_ or a _profile_.</span></span> <span data-ttu-id="3ca6c-129">Het is momenteel aanbevolen dat u een specifieke serviceversie gebruikt voor zowel de ontwikkeling als de release.</span><span class="sxs-lookup"><span data-stu-id="3ca6c-129">Currently, it is recommended that you use a specific service version for both development and release.</span></span> <span data-ttu-id="3ca6c-130">Services bevinden zich in de module `services`.</span><span class="sxs-lookup"><span data-stu-id="3ca6c-130">Services are located under the `services` module.</span></span> <span data-ttu-id="3ca6c-131">Het volledige pad voor importeren is de naam van de service, gevolgd door de versie in de indeling `YYYY-MM-DD` en weer gevolgd door de naam van de service.</span><span class="sxs-lookup"><span data-stu-id="3ca6c-131">The full path for the import is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="3ca6c-132">Als u bijvoorbeeld de versie van `2017-03-30` wilt opnemen in de Compute-service, gebruikt u:</span><span class="sxs-lookup"><span data-stu-id="3ca6c-132">For example, to include the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="3ca6c-133">Het is momenteel aanbevolen dat u de laatste versie van een service gebruikt, tenzij u een goede reden hebt om dat niet te doen.</span><span class="sxs-lookup"><span data-stu-id="3ca6c-133">Right now it is recommended that you use the latest version of a service, unless you have a reason to do otherwise.</span></span>

<span data-ttu-id="3ca6c-134">U kunt ook één profielversie selecteren als u een collectieve momentopname van de services nodig hebt.</span><span class="sxs-lookup"><span data-stu-id="3ca6c-134">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="3ca6c-135">Het enige vergrendelde profiel is momenteel `2017-03-30`, dat wellicht niet over de nieuwste functies of services beschikt.</span><span class="sxs-lookup"><span data-stu-id="3ca6c-135">Right now, the only locked profile is version `2017-03-30`, which may not have the latest features of services.</span></span> <span data-ttu-id="3ca6c-136">Profielen bevinden zich in de module `profiles` met de versie in de indeling `YYYY-MM-DD`.</span><span class="sxs-lookup"><span data-stu-id="3ca6c-136">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="3ca6c-137">Services worden op basis van hun profielversie gegroepeerd.</span><span class="sxs-lookup"><span data-stu-id="3ca6c-137">Services are grouped under their profile version.</span></span> <span data-ttu-id="3ca6c-138">Als u bijvoorbeeld de Azure Resources-beheermodule wilt importeren uit het profiel `2017-03-09`, gebruikt u:</span><span class="sxs-lookup"><span data-stu-id="3ca6c-138">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="3ca6c-139">De profielen `preview` en `latest` zijn ook beschikbaar.</span><span class="sxs-lookup"><span data-stu-id="3ca6c-139">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="3ca6c-140">Het gebruik ervan is niet aanbevolen.</span><span class="sxs-lookup"><span data-stu-id="3ca6c-140">Using them is not recommended.</span></span> <span data-ttu-id="3ca6c-141">Deze profielen zijn doorlopende versies en het servicegedrag kan op elk moment veranderen.</span><span class="sxs-lookup"><span data-stu-id="3ca6c-141">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3ca6c-142">Volgende stappen</span><span class="sxs-lookup"><span data-stu-id="3ca6c-142">Next steps</span></span>

<span data-ttu-id="3ca6c-143">Gebruik een snelstartgids om aan de slag te gaan met de Azure SDK voor Go.</span><span class="sxs-lookup"><span data-stu-id="3ca6c-143">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="3ca6c-144">Een virtuele machine implementeren vanuit een sjabloon</span><span class="sxs-lookup"><span data-stu-id="3ca6c-144">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="3ca6c-145">Objecten overdragen naar Azure Blob-opslag met de Azure Blob SDK voor Go</span><span class="sxs-lookup"><span data-stu-id="3ca6c-145">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="3ca6c-146">Verbinding maken met Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="3ca6c-146">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="3ca6c-147">Als u onmiddellijk aan de slag wilt gaan met andere services in de Go SDK, kunt u een aantal van de beschikbare voorbeeldcodes bekijken.</span><span class="sxs-lookup"><span data-stu-id="3ca6c-147">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="3ca6c-148">Verifiëren met Azure-services</span><span class="sxs-lookup"><span data-stu-id="3ca6c-148">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [<span data-ttu-id="3ca6c-149">Nieuwe virtuele machines met SSH-verificatie implementeren</span><span class="sxs-lookup"><span data-stu-id="3ca6c-149">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="3ca6c-150">Een containerinstallatiekopie implementeren in Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="3ca6c-150">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="3ca6c-151">Een cluster in Azure Kubernetes Service maken</span><span class="sxs-lookup"><span data-stu-id="3ca6c-151">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="3ca6c-152">Werken met Azure Storage-services</span><span class="sxs-lookup"><span data-stu-id="3ca6c-152">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="3ca6c-153">Alle voorbeelden voor de Azure SDK voor Go</span><span class="sxs-lookup"><span data-stu-id="3ca6c-153">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)

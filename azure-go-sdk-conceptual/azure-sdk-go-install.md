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
ms.openlocfilehash: 2799e3a6c637036eeaf7b20adf8aa55a8a4ab400
ms.sourcegitcommit: 4db332f5e43a5b43032ff9017805d5fd5a650d86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55145529"
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="cb855-103">De Azure SDK voor Go installeren</span><span class="sxs-lookup"><span data-stu-id="cb855-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="cb855-104">Welkom bij de Azure SDK voor Go.</span><span class="sxs-lookup"><span data-stu-id="cb855-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="cb855-105">Met de SDK kunt u Azure-services beheren en ermee communiceren vanuit uw Go-toepassingen.</span><span class="sxs-lookup"><span data-stu-id="cb855-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="cb855-106">De Azure SDK voor Go ophalen</span><span class="sxs-lookup"><span data-stu-id="cb855-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="cb855-107">Sommige Azure-services hebben hun eigen SDK voor Go en zijn niet opgenomen in het basispakket Azure SDK voor Go.</span><span class="sxs-lookup"><span data-stu-id="cb855-107">Some Azure services have their own Go SDK and aren't included in the core Azure SDK for Go package.</span></span> <span data-ttu-id="cb855-108">De volgende tabel toont de services met hun eigen SDK's en hun pakketnamen.</span><span class="sxs-lookup"><span data-stu-id="cb855-108">The following table lists the services with their own SDKs and their package names.</span></span> <span data-ttu-id="cb855-109">Deze pakketten worden allemaal beschouwd als Preview-versie.</span><span class="sxs-lookup"><span data-stu-id="cb855-109">These packages are all considered to be in preview.</span></span>

| <span data-ttu-id="cb855-110">Service</span><span class="sxs-lookup"><span data-stu-id="cb855-110">Service</span></span> | <span data-ttu-id="cb855-111">Pakket</span><span class="sxs-lookup"><span data-stu-id="cb855-111">Package</span></span> |
|---------|---------|
| <span data-ttu-id="cb855-112">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="cb855-112">Blob Storage</span></span> | [<span data-ttu-id="cb855-113">github.com/Azure/azure-storage-blob-go</span><span class="sxs-lookup"><span data-stu-id="cb855-113">github.com/Azure/azure-storage-blob-go</span></span>](https://github.com/Azure/azure-storage-blob-go) |
| <span data-ttu-id="cb855-114">File Storage</span><span class="sxs-lookup"><span data-stu-id="cb855-114">File Storage</span></span> | [<span data-ttu-id="cb855-115">github.com/Azure/azure-storage-file-go</span><span class="sxs-lookup"><span data-stu-id="cb855-115">github.com/Azure/azure-storage-file-go</span></span>](https://github.com/Azure/azure-storage-file-go) |
| <span data-ttu-id="cb855-116">Opslagwachtrij</span><span class="sxs-lookup"><span data-stu-id="cb855-116">Storage Queue</span></span> | [<span data-ttu-id="cb855-117">github.com/Azure/azure-storage-queue-go</span><span class="sxs-lookup"><span data-stu-id="cb855-117">github.com/Azure/azure-storage-queue-go</span></span>](https://github.com/Azure/azure-storage-queue-go) |
| <span data-ttu-id="cb855-118">Event Hub</span><span class="sxs-lookup"><span data-stu-id="cb855-118">Event Hub</span></span> | [<span data-ttu-id="cb855-119">github.com/Azure/azure-event-hubs-go</span><span class="sxs-lookup"><span data-stu-id="cb855-119">github.com/Azure/azure-event-hubs-go</span></span>](https://github.com/Azure/azure-event-hubs-go) |
| <span data-ttu-id="cb855-120">Service Bus</span><span class="sxs-lookup"><span data-stu-id="cb855-120">Service Bus</span></span> | [<span data-ttu-id="cb855-121">github.com/Azure/azure-service-bus-go</span><span class="sxs-lookup"><span data-stu-id="cb855-121">github.com/Azure/azure-service-bus-go</span></span>](https://github.com/Azure/azure-service-bus-go) |
| <span data-ttu-id="cb855-122">Application Insights</span><span class="sxs-lookup"><span data-stu-id="cb855-122">Application Insights</span></span> | [<span data-ttu-id="cb855-123">github.com/Microsoft/ApplicationInsights-go</span><span class="sxs-lookup"><span data-stu-id="cb855-123">github.com/Microsoft/ApplicationInsights-go</span></span>](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="cb855-124">De Azure SDK voor Go bewaren in Vendor</span><span class="sxs-lookup"><span data-stu-id="cb855-124">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="cb855-125">Het is mogelijk om de Azure SDK voor Go in de map Vendor te bewaren via [dep](https://github.com/golang/dep).</span><span class="sxs-lookup"><span data-stu-id="cb855-125">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="cb855-126">Ten behoeve van de stabiliteit is het aanbevolen om Vendor te gebruiken.</span><span class="sxs-lookup"><span data-stu-id="cb855-126">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="cb855-127">Gebruik `dep` in uw eigen project, voeg `github.com/Azure/azure-sdk-for-go` toe aan een sectie `[[constraint]]` van uw `Gopkg.toml`.</span><span class="sxs-lookup"><span data-stu-id="cb855-127">To use `dep` in your own project, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="cb855-128">Als u bijvoorbeeld gebruik wilt maken van Vendor op versie `14.0.0`, voegt u de volgende vermelding toe:</span><span class="sxs-lookup"><span data-stu-id="cb855-128">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```toml
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="cb855-129">De Azure SDK voor Go opnemen in uw project</span><span class="sxs-lookup"><span data-stu-id="cb855-129">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="cb855-130">Als u Azure-services vanuit uw Go-code wilt gebruiken, importeert u de services waarmee u communiceert en de vereiste `autorest`-modules.</span><span class="sxs-lookup"><span data-stu-id="cb855-130">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="cb855-131">U krijgt een volledige lijst te zien van de beschikbare modules van GoDoc voor [beschikbare services](https://godoc.org/github.com/Azure/azure-sdk-for-go) en [AutoRest-pakketten](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="cb855-131">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="cb855-132">De meestvoorkomende pakketten die u van `go-autorest` nodig hebt, zijn:</span><span class="sxs-lookup"><span data-stu-id="cb855-132">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="cb855-133">Pakket</span><span class="sxs-lookup"><span data-stu-id="cb855-133">Package</span></span> | <span data-ttu-id="cb855-134">Beschrijving</span><span class="sxs-lookup"><span data-stu-id="cb855-134">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="cb855-135">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="cb855-135">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="cb855-136">Objecten om de service-clientverificatie te verwerken</span><span class="sxs-lookup"><span data-stu-id="cb855-136">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="cb855-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="cb855-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="cb855-138">Constanten voor interactie met Azure-services</span><span class="sxs-lookup"><span data-stu-id="cb855-138">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="cb855-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="cb855-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="cb855-140">Verificatiemechanismen voor toegang tot Azure-services</span><span class="sxs-lookup"><span data-stu-id="cb855-140">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="cb855-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="cb855-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="cb855-142">Type assertiehulpprogramma's om met Azure SDK-gegevensstructuren te werken</span><span class="sxs-lookup"><span data-stu-id="cb855-142">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="cb855-143">Go-pakketten en Azure-services krijgen onafhankelijk van elkaar een versienummer.</span><span class="sxs-lookup"><span data-stu-id="cb855-143">Go packages and Azure services are versioned independently.</span></span> <span data-ttu-id="cb855-144">De serviceversies zijn deel van het importpad van de module, onder de module `services`.</span><span class="sxs-lookup"><span data-stu-id="cb855-144">The service versions are part of the module import path, underneath the `services` module.</span></span> <span data-ttu-id="cb855-145">Het volledige pad voor de module is de naam van de service, gevolgd door de versie in de indeling `YYYY-MM-DD` en weer gevolgd door de naam van de service.</span><span class="sxs-lookup"><span data-stu-id="cb855-145">The full path for the module is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="cb855-146">Als u bijvoorbeeld de versie van `2017-03-30` wilt importeren in de Compute-service, gebruikt u:</span><span class="sxs-lookup"><span data-stu-id="cb855-146">For example, to import the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="cb855-147">Het wordt aanbevolen dat u de nieuwste versie van een service gebruikt wanneer u begint te ontwikkelen en dat u die behoudt.</span><span class="sxs-lookup"><span data-stu-id="cb855-147">It's recommended that you use the latest version of a service when starting development and keep it consistent.</span></span>
<span data-ttu-id="cb855-148">Servicevereisten kunnen verschillen tussen versies en dat kan uw code breken, zelfs als er op dat moment geen updates van de Go SDK zijn.</span><span class="sxs-lookup"><span data-stu-id="cb855-148">Service requirements may change between versions that could break your code, even if there are no Go SDK updates during that time.</span></span>

<span data-ttu-id="cb855-149">U kunt ook één profielversie selecteren als u een collectieve momentopname van de services nodig hebt.</span><span class="sxs-lookup"><span data-stu-id="cb855-149">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="cb855-150">Het enige vergrendelde profiel is momenteel `2017-03-09`, dat wellicht niet over de nieuwste functies of services beschikt.</span><span class="sxs-lookup"><span data-stu-id="cb855-150">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="cb855-151">Profielen bevinden zich in de module `profiles` met de versie in de indeling `YYYY-MM-DD`.</span><span class="sxs-lookup"><span data-stu-id="cb855-151">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="cb855-152">Services worden op basis van hun profielversie gegroepeerd.</span><span class="sxs-lookup"><span data-stu-id="cb855-152">Services are grouped under their profile version.</span></span> <span data-ttu-id="cb855-153">Als u bijvoorbeeld de Azure Resources-beheermodule wilt importeren uit het profiel `2017-03-09`, gebruikt u:</span><span class="sxs-lookup"><span data-stu-id="cb855-153">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="cb855-154">De profielen `preview` en `latest` zijn ook beschikbaar.</span><span class="sxs-lookup"><span data-stu-id="cb855-154">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="cb855-155">Het gebruik ervan is niet aanbevolen.</span><span class="sxs-lookup"><span data-stu-id="cb855-155">Using them is not recommended.</span></span> <span data-ttu-id="cb855-156">Deze profielen zijn doorlopende versies en het servicegedrag kan op elk moment veranderen.</span><span class="sxs-lookup"><span data-stu-id="cb855-156">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb855-157">Volgende stappen</span><span class="sxs-lookup"><span data-stu-id="cb855-157">Next steps</span></span>

<span data-ttu-id="cb855-158">Gebruik een snelstartgids om aan de slag te gaan met de Azure SDK voor Go.</span><span class="sxs-lookup"><span data-stu-id="cb855-158">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="cb855-159">Een virtuele machine implementeren vanuit een sjabloon</span><span class="sxs-lookup"><span data-stu-id="cb855-159">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="cb855-160">Objecten overdragen naar Azure Blob-opslag met de Azure Blob SDK voor Go</span><span class="sxs-lookup"><span data-stu-id="cb855-160">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="cb855-161">Verbinding maken met Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="cb855-161">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="cb855-162">Als u onmiddellijk aan de slag wilt gaan met andere services in de Go SDK, kunt u een aantal van de beschikbare voorbeeldcodes bekijken.</span><span class="sxs-lookup"><span data-stu-id="cb855-162">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="cb855-163">Verifiëren met Azure-services</span><span class="sxs-lookup"><span data-stu-id="cb855-163">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/internal/iam)
* [<span data-ttu-id="cb855-164">Nieuwe virtuele machines met SSH-verificatie implementeren</span><span class="sxs-lookup"><span data-stu-id="cb855-164">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="cb855-165">Een containerinstallatiekopie implementeren in Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="cb855-165">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="cb855-166">Een cluster in Azure Kubernetes Service maken</span><span class="sxs-lookup"><span data-stu-id="cb855-166">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute)
* [<span data-ttu-id="cb855-167">Werken met Azure Storage-services</span><span class="sxs-lookup"><span data-stu-id="cb855-167">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="cb855-168">Alle voorbeelden voor de Azure SDK voor Go</span><span class="sxs-lookup"><span data-stu-id="cb855-168">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)

---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 5df14f939efdd0550b49261c88c8dc6518ada459
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059259"
---
<span data-ttu-id="93232-101">De [Azure SDK voor Go](https://github.com/Azure/azure-sdk-for-go) is compatibel met Go-versie 1.8 en hoger.</span><span class="sxs-lookup"><span data-stu-id="93232-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and higher.</span></span> <span data-ttu-id="93232-102">Voor omgevingen die gebruikmaken van [Azure Stack-profielen](/azure/azure-stack/user/azure-stack-version-profiles-go) is Go-versie 1.9 de minimale vereiste.</span><span class="sxs-lookup"><span data-stu-id="93232-102">For environments using [Azure Stack Profiles](/azure/azure-stack/user/azure-stack-version-profiles-go), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="93232-103">Volg [de installatie-instructies voor Go](https://golang.org/doc/install) als u Go moet installeren.</span><span class="sxs-lookup"><span data-stu-id="93232-103">If you need to install Go, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="93232-104">U downloadt de Azure SDK voor Go en de afhankelijkheden ervan via `go get`.</span><span class="sxs-lookup"><span data-stu-id="93232-104">You can download the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="93232-105">Zorg ervoor dat u `Azure` in de URL in hoofdletters schrijft.</span><span class="sxs-lookup"><span data-stu-id="93232-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="93232-106">Als u dit niet doet, kan dit importproblemen opleveren wanneer u aan het werk bent met de SDK.</span><span class="sxs-lookup"><span data-stu-id="93232-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="93232-107">U moet `Azure` ook in uw importinstructies in hoofdletters schrijven.</span><span class="sxs-lookup"><span data-stu-id="93232-107">You also need to capitalize `Azure` in your import statements.</span></span>

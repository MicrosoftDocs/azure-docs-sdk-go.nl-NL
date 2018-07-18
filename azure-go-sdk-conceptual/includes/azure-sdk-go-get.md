---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: d021dd8ef4744b7c50b296b231bf63481f92411a
ms.sourcegitcommit: 2a3bd491e087a1d0e7d269bed896c029357d62a6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38988063"
---
<span data-ttu-id="21516-101">De [Azure SDK voor Go](https://github.com/Azure/azure-sdk-for-go) is compatibel met Go-versie 1.8 en hoger.</span><span class="sxs-lookup"><span data-stu-id="21516-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and later.</span></span> <span data-ttu-id="21516-102">Voor omgevingen die gebruikmaken van [Azure Stack-profielen](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles) is Go-versie 1.9 de minimale vereiste.</span><span class="sxs-lookup"><span data-stu-id="21516-102">For environments using [Azure Stack Profiles](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="21516-103">Als u Go niet op uw systeem hebt staan, volgt u [de installatie-instructies voor Go](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="21516-103">If you do not have Go available on your system, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="21516-104">U kunt de Azure SDK voor Go en de afhankelijkheden ervan via `go get` verkrijgen.</span><span class="sxs-lookup"><span data-stu-id="21516-104">You can obtain the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="21516-105">Zorg ervoor dat u `Azure` in de URL in hoofdletters schrijft.</span><span class="sxs-lookup"><span data-stu-id="21516-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="21516-106">Als u dit niet doet, kan dit importproblemen opleveren wanneer u aan het werk bent met de SDK.</span><span class="sxs-lookup"><span data-stu-id="21516-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="21516-107">U moet `Azure` ook in uw importinstructies in hoofdletters schrijven.</span><span class="sxs-lookup"><span data-stu-id="21516-107">You also need to capitalize `Azure` in your import statements.</span></span>

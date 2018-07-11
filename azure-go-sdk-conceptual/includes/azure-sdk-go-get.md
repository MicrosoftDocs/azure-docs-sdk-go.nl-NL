---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 0febc2cc42ad95a1b7a5032c7987e37cc82f374e
ms.sourcegitcommit: 181d4e0b164cf39b3feac346f559596bd19c94db
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38067030"
---
De [Azure SDK voor Go](https://github.com/Azure/azure-sdk-for-go) is compatibel met Go-versie 1.8 en hoger. Voor omgevingen die gebruikmaken van [Azure Stack-profielen](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles) is Go-versie 1.9 de minimale vereiste.
Als u Go niet op uw systeem hebt staan, volgt u [de installatie-instructies voor Go](https://golang.org/doc/install).

U kunt de Azure SDK voor Go en de afhankelijkheden ervan via `go get` verkrijgen.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Zorg ervoor dat u `Azure` in de URL in hoofdletters schrijft. Als u dit niet doet, kan dit importproblemen opleveren wanneer u aan het werk bent met de SDK. U moet `Azure` ook in uw importinstructies in hoofdletters schrijven.


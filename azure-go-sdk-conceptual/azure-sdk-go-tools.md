---
title: Hulpprogramma's voor Go-ontwikkelaars
description: Hulpprogramma's om met de Azure SDK voor Go en Azure-services te werken
author: sptramer
ms.author: sttramer
ms.date: 01/30/2018
ms.topic: article
ms.devlang: go
manager: carmonm
ms.openlocfilehash: 054965eb1ea4f1a7556e2968dfbe07b2db69d26f
ms.sourcegitcommit: fcc1786d59d2e32c97a9a8e0748e06f564a961bd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Hulpprogramma's voor ontwikkelaars die gebruikmaken van de Azure SDK voor Go

Als u effectief wilt coderen met Go en u wilt dat Go perfect werkt met Azure-services, vindt u hieronder een aantal aanbevolen hulpprogramma's.

## <a name="azure-cli-20"></a>Azure CLI 2.0

Azure CLI 2.0 biedt een opdrachtregelinterface om Azure-resources te maken en te configureren in uw abonnementen. De CLI kan u op weg helpen om snel algemene en gedeelde Azure-resources te maken, zodat u zich kunt richten op het complexere gebruik van de services. De CLI heeft query- en filterfuncties, zodat u uitvoer rechtstreeks kunt gebruiken in uw favoriete opdrachtregelprogramma's. De CLI is ter installatie beschikbaar op uw lokale systeem, als Docker-installatiekopie, of via [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).

> [!div class="nextstepaction"]
> [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code is een lichtgewicht editor die via extensies uitgebreide ondersteuning biedt voor Go. Deze extensies omvatten ondersteuning voor functies als automatisch aanvullen, `impl`-sjablonen, herstructurering en foutopsporing. Visual Studio Code biedt ook veel extensies voor algemene hulpprogramma's voor ontwikkelaars, zoals broncodebeheer, en biedt zelfs extensies voor rechtstreekse communicatie met Azure-services. Microsoft onderhoudt een officiële meta-extensie, inclusief deze Azure-extensies, waaronder een interactieve interface voor de Azure CLI.

* [Visual Studio Code installeren](https://code.visualstudio.com/Download)
* [De Go-extensie voor Visual Studio Code downloaden](https://code.visualstudio.com/docs/languages/go)
* [De extensie voor Azure Tools downloaden](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a>Beheer van afhankelijkheden met dep

Met Go zijn er vele manieren om uw pakketafhankelijkheden te beheren en in de map Vendor te bewaren, aangezien er nog geen officiële oplossing is. De aanbevolen manier voor dit beheer is met de afhankelijkheidsbeheerder `dep`. De Azure SDK voor Go gebruikt dep om zichzelf in Vendor te bewaren en haalt gegarandeerd afhankelijkheden voor elk project op de juiste manier op met behulp van dep.

> [!div class="nextstepaction"]
> [De afhankelijkheidsbeheerder dep downloaden](https://github.com/tools/godep)

## <a name="telemetry-with-application-insights"></a>Telemetrie met Azure Insights

[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) is een analyseproduct waarmee u eenvoudig telemetriegegevens van uw toepassingen kunt verzamelen en integreren met het Azure-ecosysteem, Visual Studio Team Services en GitHub. Het kan in veel toepassingen worden gebruikt en Microsoft biedt een Go SDK om met Application Insights te werken.

> [!div class="nextstepaction"]
> [De Go SDK voor Application Insights ophalen](https://github.com/Microsoft/ApplicationInsights-Go) 

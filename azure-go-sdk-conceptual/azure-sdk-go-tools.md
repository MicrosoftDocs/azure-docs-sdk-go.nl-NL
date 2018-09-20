---
title: Hulpprogramma's voor ontwikkelaars die gebruikmaken van de Azure SDK voor Go
description: Hulpprogramma's om met de Azure SDK voor Go en Azure-services te werken
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 70cf7d645f47df29e8e42599a0acd75858144783
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059200"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Hulpprogramma's voor ontwikkelaars die gebruikmaken van de Azure SDK voor Go

Als u effectief wilt coderen met Go en u wilt dat Go perfect werkt met Azure-services, vindt u hieronder een aantal aanbevolen hulpprogramma's.

## <a name="azure-cli"></a>Azure-CLI

Azure CLI biedt een opdrachtregelinterface om Azure-resources te maken en te configureren in uw abonnementen. De CLI kan u op weg helpen om snel algemene en gedeelde Azure-resources te maken, zodat u zich kunt richten op het complexere gebruik van de services. De CLI heeft query- en filterfuncties, zodat u uitvoer rechtstreeks kunt gebruiken in uw favoriete opdrachtregelprogramma's. De CLI is ter installatie beschikbaar op uw lokale systeem, als Docker-installatiekopie, of via [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!div class="nextstepaction"]
> [Azure CLI installeren](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code is een lichtgewicht editor die ondersteuning voor Go biedt. Deze extensies biedt functies als automatisch aanvullen `impl`-sjablonen, herstructurering en foutopsporing. Visual Studio Code biedt ook ondersteuning voor toegang tot broncodebeheer via de editor en extensies om met Azure-services te kunnen werken.

* [Visual Studio Code installeren](https://code.visualstudio.com/Download)
* [De Go-extensie voor Visual Studio Code downloaden](https://code.visualstudio.com/docs/languages/go)
* [De Visual Studio Code Azure Tools-extensie ophalen](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a>CI/CD met Azure DevOps Project

Met Azure DevOpjs Project-pijplijnen stelt u een systeem voor continue integratie in voor uw Go-toepassingen. Alles wat u nodig hebt is een git-opslagplaats, daarna kunt u meteen implementeren en testen in Azure.

> [!div class="nextstepaction"]
> [Leer hoe u een CI/CD-pijplijn maakt met Azure DevOps Project](/azure/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a>Beheer van afhankelijkheden met dep

De Azure SDK voor Go gebruikt dep voor het beheer van afhankelijkheden. Met de dep-opdracht kunt u vereisten voor uw Go-toepassing opvragen en plaatsen, waarmee u versieconflicten vermijdt en ervoor zorgt dat uw project juist werkt.

> [!div class="nextstepaction"]
> [De afhankelijkheidsbeheerder dep downloaden](https://github.com/golang/dep)

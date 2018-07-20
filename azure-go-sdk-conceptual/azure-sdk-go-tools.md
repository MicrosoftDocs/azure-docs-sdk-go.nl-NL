---
title: Hulpprogramma's voor Go-ontwikkelaars
description: Hulpprogramma's om met de Azure SDK voor Go en Azure-services te werken
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 07/13/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: dfa3912ac13e6f6d52d607f9dcc150f3a5b57602
ms.sourcegitcommit: d1790b317a8fcb4d672c654dac2a925a976589d4
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39039502"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Hulpprogramma's voor ontwikkelaars die gebruikmaken van de Azure SDK voor Go

Als u effectief wilt coderen met Go en u wilt dat Go perfect werkt met Azure-services, vindt u hieronder een aantal aanbevolen hulpprogramma's.

## <a name="azure-cli"></a>Azure-CLI

Azure CLI biedt een opdrachtregelinterface om Azure-resources te maken en te configureren in uw abonnementen. De CLI kan u op weg helpen om snel algemene en gedeelde Azure-resources te maken, zodat u zich kunt richten op het complexere gebruik van de services. De CLI heeft query- en filterfuncties, zodat u uitvoer rechtstreeks kunt gebruiken in uw favoriete opdrachtregelprogramma's. De CLI is ter installatie beschikbaar op uw lokale systeem, als Docker-installatiekopie, of via [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!div class="nextstepaction"]
> [Azure CLI installeren](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code is een lichtgewicht editor die via extensies uitgebreide ondersteuning biedt voor Go. Deze extensies omvatten ondersteuning voor functies als automatisch aanvullen, `impl`-sjablonen, herstructurering en foutopsporing. Visual Studio Code biedt ook veel extensies voor algemene hulpprogramma's voor ontwikkelaars, zoals broncodebeheer, en biedt zelfs extensies voor rechtstreekse communicatie met Azure-services. Microsoft onderhoudt een officiële meta-extensie, inclusief deze Azure-extensies, waaronder een interactieve interface voor de Azure CLI.

* [Visual Studio Code installeren](https://code.visualstudio.com/Download)
* [De Go-extensie voor Visual Studio Code downloaden](https://code.visualstudio.com/docs/languages/go)
* [De extensie voor Azure Tools downloaden](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a>CI/CD met Azure DevOps Project

Met de Azure DevOps Project-pijplijn kunt u continue compilatie en implementatie instellen voor uw Go-toepassingen. U hebt hiervoor alleen een beschikbare git-opslagplaats nodig. Implementatie en tests kunt u rechtstreeks op uw Azure-resources uitvoeren. De configuratiepijplijn is eenvoudig te maken en beheren. Aangezien de pijplijn rechtstreeks in Azure is ingericht, kunt u deze op dezelfde manier als al uw andere Azure-resources beheren.

> [!div class="nextstepaction"]
> [Leer hoe u een CI/CD-pijplijn maakt met Azure DevOps Project](/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a>Beheer van afhankelijkheden met dep

Met Go zijn er vele manieren om uw pakketafhankelijkheden te beheren en in de map Vendor te bewaren, aangezien er nog geen officiële oplossing is. De aanbevolen manier voor dit beheer is met de afhankelijkheidsbeheerder `dep`. De Azure SDK voor Go gebruikt dep om zichzelf in Vendor te bewaren en haalt gegarandeerd afhankelijkheden voor elk project op de juiste manier op met behulp van dep.

> [!div class="nextstepaction"]
> [De afhankelijkheidsbeheerder dep downloaden](https://github.com/golang/dep)

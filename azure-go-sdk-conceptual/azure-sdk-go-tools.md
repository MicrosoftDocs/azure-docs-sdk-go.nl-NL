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
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="ed1db-103">Hulpprogramma's voor ontwikkelaars die gebruikmaken van de Azure SDK voor Go</span><span class="sxs-lookup"><span data-stu-id="ed1db-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="ed1db-104">Als u effectief wilt coderen met Go en u wilt dat Go perfect werkt met Azure-services, vindt u hieronder een aantal aanbevolen hulpprogramma's.</span><span class="sxs-lookup"><span data-stu-id="ed1db-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="ed1db-105">Azure-CLI</span><span class="sxs-lookup"><span data-stu-id="ed1db-105">Azure CLI</span></span>

<span data-ttu-id="ed1db-106">Azure CLI biedt een opdrachtregelinterface om Azure-resources te maken en te configureren in uw abonnementen.</span><span class="sxs-lookup"><span data-stu-id="ed1db-106">The Azure CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="ed1db-107">De CLI kan u op weg helpen om snel algemene en gedeelde Azure-resources te maken, zodat u zich kunt richten op het complexere gebruik van de services.</span><span class="sxs-lookup"><span data-stu-id="ed1db-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="ed1db-108">De CLI heeft query- en filterfuncties, zodat u uitvoer rechtstreeks kunt gebruiken in uw favoriete opdrachtregelprogramma's.</span><span class="sxs-lookup"><span data-stu-id="ed1db-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="ed1db-109">De CLI is ter installatie beschikbaar op uw lokale systeem, als Docker-installatiekopie, of via [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="ed1db-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ed1db-110">Azure CLI installeren</span><span class="sxs-lookup"><span data-stu-id="ed1db-110">Install the Azure CLI</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="ed1db-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ed1db-111">Visual Studio Code</span></span>

<span data-ttu-id="ed1db-112">Visual Studio Code is een lichtgewicht editor die via extensies uitgebreide ondersteuning biedt voor Go.</span><span class="sxs-lookup"><span data-stu-id="ed1db-112">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="ed1db-113">Deze extensies omvatten ondersteuning voor functies als automatisch aanvullen, `impl`-sjablonen, herstructurering en foutopsporing.</span><span class="sxs-lookup"><span data-stu-id="ed1db-113">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="ed1db-114">Visual Studio Code biedt ook veel extensies voor algemene hulpprogramma's voor ontwikkelaars, zoals broncodebeheer, en biedt zelfs extensies voor rechtstreekse communicatie met Azure-services.</span><span class="sxs-lookup"><span data-stu-id="ed1db-114">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="ed1db-115">Microsoft onderhoudt een officiële meta-extensie, inclusief deze Azure-extensies, waaronder een interactieve interface voor de Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="ed1db-115">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="ed1db-116">Visual Studio Code installeren</span><span class="sxs-lookup"><span data-stu-id="ed1db-116">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="ed1db-117">De Go-extensie voor Visual Studio Code downloaden</span><span class="sxs-lookup"><span data-stu-id="ed1db-117">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="ed1db-118">De extensie voor Azure Tools downloaden</span><span class="sxs-lookup"><span data-stu-id="ed1db-118">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a><span data-ttu-id="ed1db-119">CI/CD met Azure DevOps Project</span><span class="sxs-lookup"><span data-stu-id="ed1db-119">CI/CD with Azure DevOps Project</span></span>

<span data-ttu-id="ed1db-120">Met de Azure DevOps Project-pijplijn kunt u continue compilatie en implementatie instellen voor uw Go-toepassingen.</span><span class="sxs-lookup"><span data-stu-id="ed1db-120">With the Azure DevOps Project pipeline, you can set up a continuous build and deploy for your Go applications.</span></span> <span data-ttu-id="ed1db-121">U hebt hiervoor alleen een beschikbare git-opslagplaats nodig. Implementatie en tests kunt u rechtstreeks op uw Azure-resources uitvoeren.</span><span class="sxs-lookup"><span data-stu-id="ed1db-121">All you need is an available git repo, and you can set up to deploy and test directly on your Azure resources.</span></span> <span data-ttu-id="ed1db-122">De configuratiepijplijn is eenvoudig te maken en beheren. Aangezien de pijplijn rechtstreeks in Azure is ingericht, kunt u deze op dezelfde manier als al uw andere Azure-resources beheren.</span><span class="sxs-lookup"><span data-stu-id="ed1db-122">The configuration pipeline is easy to create and manage, and since it's provisioned directly on Azure, you can control it in the same way that you handle your other Azure resources.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ed1db-123">Leer hoe u een CI/CD-pijplijn maakt met Azure DevOps Project</span><span class="sxs-lookup"><span data-stu-id="ed1db-123">Learn how to create a CI/CD pipeline with Azure DevOps Project</span></span>](/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="ed1db-124">Beheer van afhankelijkheden met dep</span><span class="sxs-lookup"><span data-stu-id="ed1db-124">Dependency management with dep</span></span>

<span data-ttu-id="ed1db-125">Met Go zijn er vele manieren om uw pakketafhankelijkheden te beheren en in de map Vendor te bewaren, aangezien er nog geen officiële oplossing is.</span><span class="sxs-lookup"><span data-stu-id="ed1db-125">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="ed1db-126">De aanbevolen manier voor dit beheer is met de afhankelijkheidsbeheerder `dep`.</span><span class="sxs-lookup"><span data-stu-id="ed1db-126">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="ed1db-127">De Azure SDK voor Go gebruikt dep om zichzelf in Vendor te bewaren en haalt gegarandeerd afhankelijkheden voor elk project op de juiste manier op met behulp van dep.</span><span class="sxs-lookup"><span data-stu-id="ed1db-127">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ed1db-128">De afhankelijkheidsbeheerder dep downloaden</span><span class="sxs-lookup"><span data-stu-id="ed1db-128">Get the dep dependency manager</span></span>](https://github.com/golang/dep)

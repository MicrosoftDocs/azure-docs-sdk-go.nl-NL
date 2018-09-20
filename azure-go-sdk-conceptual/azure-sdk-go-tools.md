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
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="39af4-103">Hulpprogramma's voor ontwikkelaars die gebruikmaken van de Azure SDK voor Go</span><span class="sxs-lookup"><span data-stu-id="39af4-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="39af4-104">Als u effectief wilt coderen met Go en u wilt dat Go perfect werkt met Azure-services, vindt u hieronder een aantal aanbevolen hulpprogramma's.</span><span class="sxs-lookup"><span data-stu-id="39af4-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="39af4-105">Azure-CLI</span><span class="sxs-lookup"><span data-stu-id="39af4-105">Azure CLI</span></span>

<span data-ttu-id="39af4-106">Azure CLI biedt een opdrachtregelinterface om Azure-resources te maken en te configureren in uw abonnementen.</span><span class="sxs-lookup"><span data-stu-id="39af4-106">The Azure CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="39af4-107">De CLI kan u op weg helpen om snel algemene en gedeelde Azure-resources te maken, zodat u zich kunt richten op het complexere gebruik van de services.</span><span class="sxs-lookup"><span data-stu-id="39af4-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="39af4-108">De CLI heeft query- en filterfuncties, zodat u uitvoer rechtstreeks kunt gebruiken in uw favoriete opdrachtregelprogramma's.</span><span class="sxs-lookup"><span data-stu-id="39af4-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="39af4-109">De CLI is ter installatie beschikbaar op uw lokale systeem, als Docker-installatiekopie, of via [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="39af4-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="39af4-110">Azure CLI installeren</span><span class="sxs-lookup"><span data-stu-id="39af4-110">Install the Azure CLI</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="39af4-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="39af4-111">Visual Studio Code</span></span>

<span data-ttu-id="39af4-112">Visual Studio Code is een lichtgewicht editor die ondersteuning voor Go biedt.</span><span class="sxs-lookup"><span data-stu-id="39af4-112">Visual Studio Code is a lightweight editor that offers Go support.</span></span> <span data-ttu-id="39af4-113">Deze extensies biedt functies als automatisch aanvullen `impl`-sjablonen, herstructurering en foutopsporing.</span><span class="sxs-lookup"><span data-stu-id="39af4-113">This extension offers features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="39af4-114">Visual Studio Code biedt ook ondersteuning voor toegang tot broncodebeheer via de editor en extensies om met Azure-services te kunnen werken.</span><span class="sxs-lookup"><span data-stu-id="39af4-114">Visual Studio Code also offers support for in-editor access to source control, and extensions for working with Azure services.</span></span>

* [<span data-ttu-id="39af4-115">Visual Studio Code installeren</span><span class="sxs-lookup"><span data-stu-id="39af4-115">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="39af4-116">De Go-extensie voor Visual Studio Code downloaden</span><span class="sxs-lookup"><span data-stu-id="39af4-116">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="39af4-117">De Visual Studio Code Azure Tools-extensie ophalen</span><span class="sxs-lookup"><span data-stu-id="39af4-117">Get the Visual Studio Code Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a><span data-ttu-id="39af4-118">CI/CD met Azure DevOps Project</span><span class="sxs-lookup"><span data-stu-id="39af4-118">CI/CD with Azure DevOps Project</span></span>

<span data-ttu-id="39af4-119">Met Azure DevOpjs Project-pijplijnen stelt u een systeem voor continue integratie in voor uw Go-toepassingen.</span><span class="sxs-lookup"><span data-stu-id="39af4-119">Azure DevOps Project pipelines allow you to set up a continuous integration system for your Go applications.</span></span> <span data-ttu-id="39af4-120">Alles wat u nodig hebt is een git-opslagplaats, daarna kunt u meteen implementeren en testen in Azure.</span><span class="sxs-lookup"><span data-stu-id="39af4-120">All it takes is a git repo, and you can deploy and test directly on Azure.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="39af4-121">Leer hoe u een CI/CD-pijplijn maakt met Azure DevOps Project</span><span class="sxs-lookup"><span data-stu-id="39af4-121">Learn how to create a CI/CD pipeline with Azure DevOps Project</span></span>](/azure/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="39af4-122">Beheer van afhankelijkheden met dep</span><span class="sxs-lookup"><span data-stu-id="39af4-122">Dependency management with dep</span></span>

<span data-ttu-id="39af4-123">De Azure SDK voor Go gebruikt dep voor het beheer van afhankelijkheden.</span><span class="sxs-lookup"><span data-stu-id="39af4-123">The Azure SDK for Go uses dep for dependency management.</span></span> <span data-ttu-id="39af4-124">Met de dep-opdracht kunt u vereisten voor uw Go-toepassing opvragen en plaatsen, waarmee u versieconflicten vermijdt en ervoor zorgt dat uw project juist werkt.</span><span class="sxs-lookup"><span data-stu-id="39af4-124">The dep command allows you to pull and vendor requirements for your Go application, avoiding version conflicts and ensuring that your project works correctly.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="39af4-125">De afhankelijkheidsbeheerder dep downloaden</span><span class="sxs-lookup"><span data-stu-id="39af4-125">Get the dep dependency manager</span></span>](https://github.com/golang/dep)

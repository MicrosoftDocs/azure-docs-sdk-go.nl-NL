---
title: Hulpprogramma's voor Go-ontwikkelaars
description: Hulpprogramma's om met de Azure SDK voor Go en Azure-services te werken
keywords: azure, go, golang, azure, visual studio, visual studio code
author: sptramer
ms.author: sttramer
ms.date: 01/30/2018
ms.topic: article
ms.devlang: go
manager: routlaw
ms.openlocfilehash: 4753775e608b39c6da43d64fd08c1532e03d5810
ms.sourcegitcommit: aaa8c37880332625f858a38f5918e6cf581bf48d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2018
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="62e0d-104">Hulpprogramma's voor ontwikkelaars die gebruikmaken van de Azure SDK voor Go</span><span class="sxs-lookup"><span data-stu-id="62e0d-104">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="62e0d-105">Als u effectief wilt coderen met Go en u wilt dat Go perfect werkt met Azure-services, vindt u hieronder een aantal aanbevolen hulpprogramma's.</span><span class="sxs-lookup"><span data-stu-id="62e0d-105">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli-20"></a><span data-ttu-id="62e0d-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="62e0d-106">Azure CLI 2.0</span></span>

<span data-ttu-id="62e0d-107">Azure CLI 2.0 biedt een opdrachtregelinterface om Azure-resources te maken en te configureren in uw abonnementen.</span><span class="sxs-lookup"><span data-stu-id="62e0d-107">The Azure 2.0 CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="62e0d-108">De CLI kan u op weg helpen om snel algemene en gedeelde Azure-resources te maken, zodat u zich kunt richten op het complexere gebruik van de services.</span><span class="sxs-lookup"><span data-stu-id="62e0d-108">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="62e0d-109">De CLI heeft query- en filterfuncties, zodat u uitvoer rechtstreeks kunt gebruiken in uw favoriete opdrachtregelprogramma's.</span><span class="sxs-lookup"><span data-stu-id="62e0d-109">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="62e0d-110">De CLI is ter installatie beschikbaar op uw lokale systeem, als Docker-installatiekopie, of via [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="62e0d-110">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="62e0d-111">Azure CLI 2.0 installeren</span><span class="sxs-lookup"><span data-stu-id="62e0d-111">Install the Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="62e0d-112">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="62e0d-112">Visual Studio Code</span></span>

<span data-ttu-id="62e0d-113">Visual Studio Code is een lichtgewicht editor die via extensies uitgebreide ondersteuning biedt voor Go.</span><span class="sxs-lookup"><span data-stu-id="62e0d-113">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="62e0d-114">Deze extensies omvatten ondersteuning voor functies als automatisch aanvullen, `impl`-sjablonen, herstructurering en foutopsporing.</span><span class="sxs-lookup"><span data-stu-id="62e0d-114">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="62e0d-115">Visual Studio Code biedt ook veel extensies voor algemene hulpprogramma's voor ontwikkelaars, zoals broncodebeheer, en biedt zelfs extensies voor rechtstreekse communicatie met Azure-services.</span><span class="sxs-lookup"><span data-stu-id="62e0d-115">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="62e0d-116">Microsoft onderhoudt een officiële meta-extensie, inclusief deze Azure-extensies, waaronder een interactieve interface voor de Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="62e0d-116">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="62e0d-117">Visual Studio Code installeren</span><span class="sxs-lookup"><span data-stu-id="62e0d-117">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="62e0d-118">De Go-extensie voor Visual Studio Code downloaden</span><span class="sxs-lookup"><span data-stu-id="62e0d-118">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="62e0d-119">De extensie voor Azure Tools downloaden</span><span class="sxs-lookup"><span data-stu-id="62e0d-119">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="62e0d-120">Beheer van afhankelijkheden met dep</span><span class="sxs-lookup"><span data-stu-id="62e0d-120">Dependency management with dep</span></span>

<span data-ttu-id="62e0d-121">Met Go zijn er vele manieren om uw pakketafhankelijkheden te beheren en in de map Vendor te bewaren, aangezien er nog geen officiële oplossing is.</span><span class="sxs-lookup"><span data-stu-id="62e0d-121">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="62e0d-122">De aanbevolen manier voor dit beheer is met de afhankelijkheidsbeheerder `dep`.</span><span class="sxs-lookup"><span data-stu-id="62e0d-122">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="62e0d-123">De Azure SDK voor Go gebruikt dep om zichzelf in Vendor te bewaren en haalt gegarandeerd afhankelijkheden voor elk project op de juiste manier op met behulp van dep.</span><span class="sxs-lookup"><span data-stu-id="62e0d-123">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="62e0d-124">De afhankelijkheidsbeheerder dep downloaden</span><span class="sxs-lookup"><span data-stu-id="62e0d-124">Get the dep dependency manager</span></span>](https://github.com/tools/godep)

## <a name="telemetry-with-application-insights"></a><span data-ttu-id="62e0d-125">Telemetrie met Azure Insights</span><span class="sxs-lookup"><span data-stu-id="62e0d-125">Telemetry with Application Insights</span></span>

<span data-ttu-id="62e0d-126">[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) is een analyseproduct waarmee u eenvoudig telemetriegegevens van uw toepassingen kunt verzamelen en integreren met het Azure-ecosysteem, Visual Studio Team Services en GitHub.</span><span class="sxs-lookup"><span data-stu-id="62e0d-126">[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) is an analytics product that allows you to easily collect telemetry information from your applications and integrates with the Azure ecosystem, Visual Studio Team Services, and GitHub.</span></span> <span data-ttu-id="62e0d-127">Het kan in veel toepassingen worden gebruikt en Microsoft biedt een Go SDK om met Application Insights te werken.</span><span class="sxs-lookup"><span data-stu-id="62e0d-127">It can be used in many applications, and Microsoft offers a Go SDK for working with Application Insights.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="62e0d-128">De Go SDK voor Application Insights ophalen</span><span class="sxs-lookup"><span data-stu-id="62e0d-128">Get the Application Insights for Go SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-Go) 

---
title: Includedatei
description: Includedatei
services: functions
author: ggailey777
manager: jeconnoc
ms.service: multiple
ms.topic: include
ms.date: 06/21/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 0f86f2698a3a0c1e20272c335b63faf03b4b92d6
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460023"
---
<span data-ttu-id="d0c40-103">In diesem Tutorial stellen Sie eine einfache Webanwendung bereit, mit der eine HTML-basierte Benutzeroberfläche dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="d0c40-103">In this tutorial, you deploy a simple web application that presents an HTML-based user interface.</span></span> <span data-ttu-id="d0c40-104">Mit einem serverlosen Back-End kann die Anwendung Bilder hochladen und automatisch Bildtitel mit Beschreibungen abrufen.</span><span class="sxs-lookup"><span data-stu-id="d0c40-104">A serverless backend enables the application to upload images and automatically get captions describing them.</span></span>

![Ausführen der Web-App](media/functions-first-serverless-web-app/0-app-screenshot-finished.png)

<span data-ttu-id="d0c40-106">Im folgenden Diagramm sind die Azure-Dienste aufgeführt, die von der Anwendung verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="d0c40-106">The following diagram shows the Azure services used by the application:</span></span>

1. <span data-ttu-id="d0c40-107">Blob Storage stellt statischen Webinhalt (HTML, CSS, JS) bereit und speichert Bilder.</span><span class="sxs-lookup"><span data-stu-id="d0c40-107">Blob Storage serves static web content (HTML, CSS, JS) and stores images.</span></span>
2. <span data-ttu-id="d0c40-108">Azure Functions verwaltet das Hochladen von Bildern, die Größenänderung und die Speicherung von Metadaten.</span><span class="sxs-lookup"><span data-stu-id="d0c40-108">Azure Functions manages image uploads, resizing, and metadata storage.</span></span>
3. <span data-ttu-id="d0c40-109">In Cosmos DB werden Bildmetadaten gespeichert.</span><span class="sxs-lookup"><span data-stu-id="d0c40-109">Cosmos DB stores image metadata.</span></span>
4. <span data-ttu-id="d0c40-110">Logic Apps ruft Bildtitel über die Maschinelles Sehen-API ab.</span><span class="sxs-lookup"><span data-stu-id="d0c40-110">Logic Apps gets image captions from Computer Vision API.</span></span>
5. <span data-ttu-id="d0c40-111">Azure Active Directory wird zum Verwalten der Benutzerauthentifizierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="d0c40-111">Azure Active Directory manages user authentication.</span></span>

![Diagramm der Lösungsarchitektur](media/functions-first-serverless-web-app/0-architecture.jpg)

<span data-ttu-id="d0c40-113">In diesem Tutorial lernen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="d0c40-113">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="d0c40-114">Konfigurieren von Azure-Blobspeicher zum Hosten einer statischen Website und von hochgeladenen Bildern</span><span class="sxs-lookup"><span data-stu-id="d0c40-114">Configure Azure Blob storage to host a static website and uploaded images.</span></span>
> * <span data-ttu-id="d0c40-115">Hochladen von Bildern in Azure-Blobspeicher mit Azure Functions</span><span class="sxs-lookup"><span data-stu-id="d0c40-115">Upload images to Azure Blob storage using Azure Functions.</span></span>
> * <span data-ttu-id="d0c40-116">Ändern der Größe von Bildern mit Azure Functions</span><span class="sxs-lookup"><span data-stu-id="d0c40-116">Resize images using Azure Functions.</span></span>
> * <span data-ttu-id="d0c40-117">Speichern von Bildmetadaten in Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="d0c40-117">Store image metadata in Azure Cosmos DB.</span></span>
> * <span data-ttu-id="d0c40-118">Verwenden der Maschinelles Sehen-API von Cognitive Services zum automatischen Generieren von Bildtiteln</span><span class="sxs-lookup"><span data-stu-id="d0c40-118">Use Cognitive Services Vision API to auto-generate image captions.</span></span>
> * <span data-ttu-id="d0c40-119">Nutzen von Azure Active Directory zum Schützen der Web-App durch das Authentifizieren von Benutzern</span><span class="sxs-lookup"><span data-stu-id="d0c40-119">Use Azure Active Directory to secure the web app by authenticating users.</span></span>

---
title: Includedatei
description: Includedatei
services: functions
author: tdykstra
manager: jeconnoc
ms.service: multiple
ms.topic: include
ms.date: 06/21/2018
ms.author: tdykstra
ms.custom: include file
ms.openlocfilehash: d651fd3d03f2678d625e60f9ab1e9f59e623964f
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079462"
---
<span data-ttu-id="b349b-103">In diesem Tutorial stellen Sie eine einfache Webanwendung bereit, mit der eine HTML-basierte Benutzeroberfläche dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="b349b-103">In this tutorial, you deploy a simple web application that presents an HTML-based user interface.</span></span> <span data-ttu-id="b349b-104">Mit einem serverlosen Back-End kann die Anwendung Bilder hochladen und automatisch Bildtitel mit Beschreibungen abrufen.</span><span class="sxs-lookup"><span data-stu-id="b349b-104">A serverless backend enables the application to upload images and automatically get captions describing them.</span></span>

![Ausführen der Web-App](media/functions-first-serverless-web-app/0-app-screenshot-finished.png)

<span data-ttu-id="b349b-106">Im folgenden Diagramm sind die Azure-Dienste aufgeführt, die von der Anwendung verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="b349b-106">The following diagram shows the Azure services used by the application:</span></span>

1. <span data-ttu-id="b349b-107">Blob Storage stellt statischen Webinhalt (HTML, CSS, JS) bereit und speichert Bilder.</span><span class="sxs-lookup"><span data-stu-id="b349b-107">Blob Storage serves static web content (HTML, CSS, JS) and stores images.</span></span>
2. <span data-ttu-id="b349b-108">Azure Functions verwaltet das Hochladen von Bildern, die Größenänderung und die Speicherung von Metadaten.</span><span class="sxs-lookup"><span data-stu-id="b349b-108">Azure Functions manages image uploads, resizing, and metadata storage.</span></span>
3. <span data-ttu-id="b349b-109">In Cosmos DB werden Bildmetadaten gespeichert.</span><span class="sxs-lookup"><span data-stu-id="b349b-109">Cosmos DB stores image metadata.</span></span>
4. <span data-ttu-id="b349b-110">Logic Apps ruft Bildtitel über die Maschinelles Sehen-API ab.</span><span class="sxs-lookup"><span data-stu-id="b349b-110">Logic Apps gets image captions from Computer Vision API.</span></span>
5. <span data-ttu-id="b349b-111">Azure Active Directory wird zum Verwalten der Benutzerauthentifizierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="b349b-111">Azure Active Directory manages user authentication.</span></span>

![Diagramm der Lösungsarchitektur](media/functions-first-serverless-web-app/0-architecture.jpg)

<span data-ttu-id="b349b-113">In diesem Tutorial lernen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="b349b-113">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="b349b-114">Konfigurieren von Azure-Blobspeicher zum Hosten einer statischen Website und von hochgeladenen Bildern</span><span class="sxs-lookup"><span data-stu-id="b349b-114">Configure Azure Blob storage to host a static website and uploaded images.</span></span>
> * <span data-ttu-id="b349b-115">Hochladen von Bildern in Azure-Blobspeicher mit Azure Functions</span><span class="sxs-lookup"><span data-stu-id="b349b-115">Upload images to Azure Blob storage using Azure Functions.</span></span>
> * <span data-ttu-id="b349b-116">Ändern der Größe von Bildern mit Azure Functions</span><span class="sxs-lookup"><span data-stu-id="b349b-116">Resize images using Azure Functions.</span></span>
> * <span data-ttu-id="b349b-117">Speichern von Bildmetadaten in Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="b349b-117">Store image metadata in Azure Cosmos DB.</span></span>
> * <span data-ttu-id="b349b-118">Verwenden der Maschinelles Sehen-API von Cognitive Services zum automatischen Generieren von Bildtiteln</span><span class="sxs-lookup"><span data-stu-id="b349b-118">Use Cognitive Services Vision API to auto-generate image captions.</span></span>
> * <span data-ttu-id="b349b-119">Nutzen von Azure Active Directory zum Schützen der Web-App durch das Authentifizieren von Benutzern</span><span class="sxs-lookup"><span data-stu-id="b349b-119">Use Azure Active Directory to secure the web app by authenticating users.</span></span>
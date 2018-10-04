---
title: Includedatei
description: Includedatei
services: functions
author: ggailey777
manager: jeconnoc
ms.service: multiple
ms.topic: include
ms.date: 06/25/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: a66fcc3a406c79fcf9881ddaaaf8330f5b373043
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47459995"
---
<span data-ttu-id="1e834-103">Sie haben mithilfe von Azure-Diensten eine serverlose Anwendung mit vollem Funktionsumfang erstellt.</span><span class="sxs-lookup"><span data-stu-id="1e834-103">You've successfully created a full-featured, serverless application using Azure services.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="1e834-104">Bereinigen von Ressourcen</span><span class="sxs-lookup"><span data-stu-id="1e834-104">Clean up resources</span></span>

<span data-ttu-id="1e834-105">Wenn Sie die Arbeit mit dieser Anwendung beendet haben, können Sie den folgenden Befehl verwenden, um alle im Tutorial erstellten Ressourcen zu löschen:</span><span class="sxs-lookup"><span data-stu-id="1e834-105">When you are done working with this application, you can use the following command to delete all resources created during the tutorial:</span></span>

```azurecli
az group delete --name first-serverless-app
```

<span data-ttu-id="1e834-106">Geben Sie `y` ein, wenn Sie dazu aufgefordert werden.</span><span class="sxs-lookup"><span data-stu-id="1e834-106">Type `y` when prompted.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="1e834-107">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="1e834-107">Next steps</span></span>

<span data-ttu-id="1e834-108">In diesem Tutorial haben Sie Folgendes gelernt:</span><span class="sxs-lookup"><span data-stu-id="1e834-108">In this tutorial, you learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="1e834-109">Konfigurieren von Azure-Blobspeicher zum Hosten einer statischen Website und von hochgeladenen Bildern</span><span class="sxs-lookup"><span data-stu-id="1e834-109">Configure Azure Blob storage to host a static website and uploaded images.</span></span>
> * <span data-ttu-id="1e834-110">Hochladen von Bildern in Azure-Blobspeicher mit Azure Functions</span><span class="sxs-lookup"><span data-stu-id="1e834-110">Upload images to Azure Blob storage using Azure Functions.</span></span>
> * <span data-ttu-id="1e834-111">Ändern der Größe von Bildern mit Azure Functions</span><span class="sxs-lookup"><span data-stu-id="1e834-111">Resize images using Azure Functions.</span></span>
> * <span data-ttu-id="1e834-112">Speichern von Bildmetadaten in Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1e834-112">Store image metadata in Azure Cosmos DB.</span></span>
> * <span data-ttu-id="1e834-113">Verwenden der Maschinelles Sehen-API von Cognitive Services zum automatischen Generieren von Bildtiteln</span><span class="sxs-lookup"><span data-stu-id="1e834-113">Use Cognitive Services Vision API to auto-generate image captions.</span></span>
> * <span data-ttu-id="1e834-114">Nutzen von Azure Active Directory zum Schützen der Web-App durch das Authentifizieren von Benutzern</span><span class="sxs-lookup"><span data-stu-id="1e834-114">Use Azure Active Directory to secure the web app by authenticating users.</span></span>

<span data-ttu-id="1e834-115">Fahren Sie mit dem Logic Apps-Tutorial fort, um zu erfahren, wie Sie noch mehr Dienste mit Functions verbinden.</span><span class="sxs-lookup"><span data-stu-id="1e834-115">To learn how to connect even more services to Functions, continue to the Logic Apps tutorial.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="1e834-116">Erstellen einer Funktion, die in Azure Logic Apps integriert ist</span><span class="sxs-lookup"><span data-stu-id="1e834-116">Functions with Logic Apps</span></span>](https://docs.microsoft.com/azure/azure-functions/functions-twitter-email)

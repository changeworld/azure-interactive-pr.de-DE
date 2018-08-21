---
title: Includedatei
description: Includedatei
services: functions
author: tdykstra
manager: jeconnoc
ms.service: multiple
ms.topic: include
ms.date: 06/25/2018
ms.author: tdykstra
ms.custom: include file
ms.openlocfilehash: 1c4aaf7afd9fbc54dd34c2cca13a8b74e947c16a
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079475"
---
<span data-ttu-id="cdc3d-103">Sie haben mithilfe von Azure-Diensten eine serverlose Anwendung mit vollem Funktionsumfang erstellt.</span><span class="sxs-lookup"><span data-stu-id="cdc3d-103">You've successfully created a full-featured, serverless application using Azure services.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="cdc3d-104">Bereinigen von Ressourcen</span><span class="sxs-lookup"><span data-stu-id="cdc3d-104">Clean up resources</span></span>

<span data-ttu-id="cdc3d-105">Wenn Sie die Arbeit mit dieser Anwendung beendet haben, können Sie den folgenden Befehl verwenden, um alle im Tutorial erstellten Ressourcen zu löschen:</span><span class="sxs-lookup"><span data-stu-id="cdc3d-105">When you are done working with this application, you can use the following command to delete all resources created during the tutorial:</span></span>

```azurecli
az group delete --name first-serverless-app
```

<span data-ttu-id="cdc3d-106">Geben Sie `y` ein, wenn Sie dazu aufgefordert werden.</span><span class="sxs-lookup"><span data-stu-id="cdc3d-106">Type `y` when prompted.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="cdc3d-107">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="cdc3d-107">Next steps</span></span>

<span data-ttu-id="cdc3d-108">In diesem Tutorial haben Sie Folgendes gelernt:</span><span class="sxs-lookup"><span data-stu-id="cdc3d-108">In this tutorial, you learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="cdc3d-109">Konfigurieren von Azure-Blobspeicher zum Hosten einer statischen Website und von hochgeladenen Bildern</span><span class="sxs-lookup"><span data-stu-id="cdc3d-109">Configure Azure Blob storage to host a static website and uploaded images.</span></span>
> * <span data-ttu-id="cdc3d-110">Hochladen von Bildern in Azure-Blobspeicher mit Azure Functions</span><span class="sxs-lookup"><span data-stu-id="cdc3d-110">Upload images to Azure Blob storage using Azure Functions.</span></span>
> * <span data-ttu-id="cdc3d-111">Ändern der Größe von Bildern mit Azure Functions</span><span class="sxs-lookup"><span data-stu-id="cdc3d-111">Resize images using Azure Functions.</span></span>
> * <span data-ttu-id="cdc3d-112">Speichern von Bildmetadaten in Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="cdc3d-112">Store image metadata in Azure Cosmos DB.</span></span>
> * <span data-ttu-id="cdc3d-113">Verwenden der Maschinelles Sehen-API von Cognitive Services zum automatischen Generieren von Bildtiteln</span><span class="sxs-lookup"><span data-stu-id="cdc3d-113">Use Cognitive Services Vision API to auto-generate image captions.</span></span>
> * <span data-ttu-id="cdc3d-114">Nutzen von Azure Active Directory zum Schützen der Web-App durch das Authentifizieren von Benutzern</span><span class="sxs-lookup"><span data-stu-id="cdc3d-114">Use Azure Active Directory to secure the web app by authenticating users.</span></span>

<span data-ttu-id="cdc3d-115">Fahren Sie mit dem Logic Apps-Tutorial fort, um zu erfahren, wie Sie noch mehr Dienste mit Functions verbinden.</span><span class="sxs-lookup"><span data-stu-id="cdc3d-115">To learn how to connect even more services to Functions, continue to the Logic Apps tutorial.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="cdc3d-116">Erstellen einer Funktion, die in Azure Logic Apps integriert ist</span><span class="sxs-lookup"><span data-stu-id="cdc3d-116">Functions with Logic Apps</span></span>](https://docs.microsoft.com/azure/azure-functions/functions-twitter-email)
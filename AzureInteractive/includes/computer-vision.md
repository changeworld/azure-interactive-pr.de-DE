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
ms.openlocfilehash: f51b864cab14273c1e88dd85d22400e0e76ef770
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47459996"
---
<span data-ttu-id="f2cca-103">An diesem Punkt ist die Anwendung eine funktionierende Galerie, die Ihnen das Hochladen und Anzeigen von Bildern ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="f2cca-103">At this point, the application is a functional gallery that allows you to upload and view images.</span></span> <span data-ttu-id="f2cca-104">In diesem Modul wird beschrieben, wie Sie die Maschinelles Sehen-API von Microsoft Cognitive Services verwenden, um Bildtitel für hochgeladene Bilder zu generieren und die Bildtitel mit den Bildmetadaten in Cosmos DB zu speichern.</span><span class="sxs-lookup"><span data-stu-id="f2cca-104">In this module, you learn how to use the Computer Vision API from Microsoft Cognitive Services to generate captions for uploaded images and save the captions with the image metadata in Cosmos DB.</span></span>

## <a name="create-a-computer-vision-account"></a><span data-ttu-id="f2cca-105">Erstellen eines Kontos vom Typ „Maschinelles Sehen“</span><span class="sxs-lookup"><span data-stu-id="f2cca-105">Create a Computer Vision account</span></span>

<span data-ttu-id="f2cca-106">Microsoft Cognitive Services ist eine Sammlung von Diensten, mit denen Entwickler ihre Anwendungen um intelligente Funktionen und Features erweitern können.</span><span class="sxs-lookup"><span data-stu-id="f2cca-106">Microsoft Cognitive Services is a collection of services available to developers to make their applications more intelligent.</span></span> <span data-ttu-id="f2cca-107">Die Maschinelles Sehen-API ist ein serverloser Dienst, der Bilder mithilfe von erweiterten Algorithmen verarbeitet und Informationen zu jedem Bild zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="f2cca-107">The Computer Vision API is a serverless service that processes images using advanced algorithms and returns information about each image.</span></span> <span data-ttu-id="f2cca-108">Sie verfügt über einen kostenlosen Tarif, in dem bis zu 5.000 API-Aufrufe pro Monat möglich sind.</span><span class="sxs-lookup"><span data-stu-id="f2cca-108">It has a free tier that provides up to 5000 API calls per month.</span></span>

1. <span data-ttu-id="f2cca-109">Stellen Sie sicher, dass Sie noch an der Cloud Shell angemeldet sind.</span><span class="sxs-lookup"><span data-stu-id="f2cca-109">Ensure you are still signed into the Cloud Shell.</span></span> <span data-ttu-id="f2cca-110">Wählen Sie andernfalls die Option **Enter focus mode** (Fokusmodus aktivieren), um ein Cloud Shell-Fenster zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="f2cca-110">If not, select **Enter focus mode** to open a Cloud Shell window.</span></span> 

1. <span data-ttu-id="f2cca-111">Erstellen Sie in Ihrer Ressourcengruppe ein neues Cognitive Services-Konto vom Typ **ComputerVision** mit einem eindeutigen Namen.</span><span class="sxs-lookup"><span data-stu-id="f2cca-111">Create a new Cognitive Services account of kind **ComputerVision** with a unique name in your resource group.</span></span> <span data-ttu-id="f2cca-112">Verwenden Sie für den kostenlosen Tarif **F0** als SKU.</span><span class="sxs-lookup"><span data-stu-id="f2cca-112">For the free tier, use **F0** as the SKU.</span></span> <span data-ttu-id="f2cca-113">Falls Sie bereits über ein vorhandenes Konto vom Typ „Maschinelles Sehen“ verfügen, müssen Sie ggf. ein Standard-Konto (S1) erstellen. Hierfür können dann aber Kosten anfallen.</span><span class="sxs-lookup"><span data-stu-id="f2cca-113">If you already have an existing Computer Vision account, you may need to create a Standard account (S1); however, this may incur some costs.</span></span>

    ```azurecli
    az cognitiveservices account create -g first-serverless-app -n <computer vision account name> --kind ComputerVision --sku F0 -l westcentralus
    ```


## <a name="create-function-app-settings-for-computer-vision-url-and-key"></a><span data-ttu-id="f2cca-114">Erstellen von Funktions-App-Einstellungen für die Maschinelles Sehen-API und den Schlüssel</span><span class="sxs-lookup"><span data-stu-id="f2cca-114">Create Function App settings for Computer Vision URL and key</span></span>

<span data-ttu-id="f2cca-115">Zum Aufrufen der Maschinelles Sehen-API sind eine URL und ein Schlüssel erforderlich.</span><span class="sxs-lookup"><span data-stu-id="f2cca-115">To call the Computer Vision API, a URL and key are required.</span></span>

1. <span data-ttu-id="f2cca-116">Ermitteln Sie den Schlüssel und die URL für die Maschinelles Sehen-API, und speichern Sie diese Angaben in Bash-Variablen.</span><span class="sxs-lookup"><span data-stu-id="f2cca-116">Obtain the Computer Vision API key and URL and save them in bash variables.</span></span>

    ```azurecli
    export COMP_VISION_KEY=$(az cognitiveservices account keys list -g first-serverless-app -n <computer vision account name> --query key1 --output tsv)
    ```
    ```azurecli
    export COMP_VISION_URL=$(az cognitiveservices account show -g first-serverless-app -n <computer vision account name> --query endpoint --output tsv)
    ```

1. <span data-ttu-id="f2cca-117">Erstellen Sie in der Funktions-App App-Einstellungen mit dem Namen **COMP_VISION_KEY** bzw. **COMP_VISION_URL**.</span><span class="sxs-lookup"><span data-stu-id="f2cca-117">Create app settings named **COMP_VISION_KEY** and **COMP_VISION_URL**, respectively, in the function app.</span></span>

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings COMP_VISION_KEY=$COMP_VISION_KEY COMP_VISION_URL=$COMP_VISION_URL -o table
    ```


## <a name="call-computer-vision-api-from-resizeimage-function"></a><span data-ttu-id="f2cca-118">Aufrufen der Maschinelles Sehen-API über die ResizeImage-Funktion</span><span class="sxs-lookup"><span data-stu-id="f2cca-118">Call Computer Vision API from ResizeImage function</span></span>

<span data-ttu-id="f2cca-119">In den folgenden Schritten ändern Sie die **ResizeImage**-Funktion, um die Maschinelles Sehen-API aufzurufen und jedes hochgeladene Bild zu beschreiben und die Beschreibung in Cosmos DB zu speichern.</span><span class="sxs-lookup"><span data-stu-id="f2cca-119">In the following steps, you modify the **ResizeImage** function to call the Computer Vision API to describe each uploaded image and save the description in Cosmos DB.</span></span>

1. <span data-ttu-id="f2cca-120">Öffnen Sie Ihre Funktions-App im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="f2cca-120">Open your function app in the Azure Portal.</span></span>

1. <span data-ttu-id="f2cca-121">**C#**</span><span class="sxs-lookup"><span data-stu-id="f2cca-121">**C#**</span></span>

    1. <span data-ttu-id="f2cca-122">(C#) Verwenden Sie den linken Navigationsbereich, um nach der Funktion **ResizeImage** zu suchen und das zugehörige Codefenster zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="f2cca-122">(C#) Using the left navigation, locate the **ResizeImage** function and open its code window.</span></span>

    1. <span data-ttu-id="f2cca-123">(C#) Ersetzen Sie den Code durch den Inhalt von [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx).</span><span class="sxs-lookup"><span data-stu-id="f2cca-123">(C#) Replace the code with the contents of [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx).</span></span> <span data-ttu-id="f2cca-124">In diesem Code wird `HttpClient` verwendet, um die Maschinelles Sehen-API aufzurufen und das Ergebnis in Cosmos DB zu speichern.</span><span class="sxs-lookup"><span data-stu-id="f2cca-124">This code uses `HttpClient` to call Computer Vision API and save its result in Cosmos DB.</span></span>

1. <span data-ttu-id="f2cca-125">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="f2cca-125">**JavaScript**</span></span>

    1. <span data-ttu-id="f2cca-126">(JavaScript) Für diese Funktion wird das `axios`-Paket aus npm benötigt, um einen HTTP-Aufruf an die Maschinelles Sehen-API zu senden.</span><span class="sxs-lookup"><span data-stu-id="f2cca-126">(JavaScript) This function requires the `axios` package from npm to make an HTTP call to the Computer Vision API.</span></span> <span data-ttu-id="f2cca-127">Klicken Sie zum Installieren des npm-Pakets im linken Navigationsbereich auf den Namen der Funktions-App und dann auf **Plattformfeatures**.</span><span class="sxs-lookup"><span data-stu-id="f2cca-127">To install the npm package, click on the Function App's name on the left navigation and click **Platform features**.</span></span>

    1. <span data-ttu-id="f2cca-128">(JavaScript) Klicken Sie auf **Konsole**, um ein Konsolenfenster anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f2cca-128">(JavaScript) Click **Console** to reveal a console window.</span></span>

    1. <span data-ttu-id="f2cca-129">(JavaScript) Führen Sie den Befehl `npm install axios` in der Konsole aus.</span><span class="sxs-lookup"><span data-stu-id="f2cca-129">(JavaScript) Run the command `npm install axios` in the console.</span></span> <span data-ttu-id="f2cca-130">Es kann ein oder zwei Minuten dauern, bis der Vorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="f2cca-130">It may take a minute or two to complete the operation.</span></span>

    1. <span data-ttu-id="f2cca-131">(JavaScript) Klicken Sie im linken Navigationsbereich auf den Namen der Funktion (**ResizeImage**), um die Funktion anzuzeigen, und ersetzen Sie dann den gesamten Inhalt von **index.js** durch den Inhalt von [**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js).</span><span class="sxs-lookup"><span data-stu-id="f2cca-131">(JavaScript) Click on the function name (**ResizeImage**) in the left navigation to reveal the function, and then replace all of **index.js** with the contents of [**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js).</span></span>

1. <span data-ttu-id="f2cca-132">Klicken Sie unter dem Codefenster auf **Protokolle**, um den Protokollbereich zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="f2cca-132">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="f2cca-133">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="f2cca-133">Click **Save**.</span></span> <span data-ttu-id="f2cca-134">Überprüfen Sie den Protokollbereich, um sicherzustellen, dass die Funktion erfolgreich gespeichert wird und keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="f2cca-134">Check the logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="test-the-application"></a><span data-ttu-id="f2cca-135">Testen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="f2cca-135">Test the application</span></span>

1. <span data-ttu-id="f2cca-136">Öffnen Sie die Anwendung in einem Browser.</span><span class="sxs-lookup"><span data-stu-id="f2cca-136">Open the application in a browser.</span></span> <span data-ttu-id="f2cca-137">Wählen Sie eine Bilddatei aus, und laden Sie sie hoch.</span><span class="sxs-lookup"><span data-stu-id="f2cca-137">Select an image file, and upload it.</span></span>

1. <span data-ttu-id="f2cca-138">Nach einigen Sekunden sollte die Miniaturansicht des neuen Bilds auf der Seite angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="f2cca-138">After a few seconds, the thumbnail of the new image should appear on the page.</span></span> <span data-ttu-id="f2cca-139">Bewegen Sie den Mauszeiger auf das Bild, um die von „Maschinelles Sehen“ generierte Beschreibung anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f2cca-139">Hover over the image to see the description generated by Computer Vision.</span></span>


## <a name="summary"></a><span data-ttu-id="f2cca-140">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="f2cca-140">Summary</span></span>

<span data-ttu-id="f2cca-141">In diesem Modul haben Sie erfahren, wie Sie die automatische Generierung von Bildtiteln für jedes hochgeladene Bild verwenden können, indem Sie die Maschinelles Sehen-API von Microsoft Cognitive Services nutzen.</span><span class="sxs-lookup"><span data-stu-id="f2cca-141">In this module, you learned how to automatically generate captions for each uploaded image using Microsoft Cognitive Services Computer Vision API.</span></span> <span data-ttu-id="f2cca-142">Als Nächstes wird beschrieben, wie Sie der Anwendung die Authentifizierung hinzufügen, indem Sie die Azure App Service-Authentifizierung verwenden.</span><span class="sxs-lookup"><span data-stu-id="f2cca-142">Next, you learn how to add authentication to the application using Azure App Service Authentication.</span></span>

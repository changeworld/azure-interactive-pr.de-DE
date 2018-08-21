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
ms.openlocfilehash: 88b0ac838dfa8e097a30cc6cef591377e4a95ad8
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079355"
---
<span data-ttu-id="ff4f4-103">Im vorherigen Modul wurde beschrieben, wie mit einer serverlosen Funktion das sichere Hochladen von Bildern in Blobspeicher aus einer Webanwendung möglich ist.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-103">In the previous module, you saw how a serverless function can facilitate the secure uploading of images to Blob storage from a web application.</span></span> <span data-ttu-id="ff4f4-104">In diesem Modul erstellen Sie eine weitere serverlose Funktion, um hochgeladene Bilder zu ermitteln und daraus Miniaturansichten zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-104">In this module, you create another serverless function to watch for uploaded images and create thumbnails from them.</span></span>

## <a name="create-a-blob-storage-container-for-thumbnails"></a><span data-ttu-id="ff4f4-105">Erstellen eines Blobspeichercontainers für Miniaturansichten</span><span class="sxs-lookup"><span data-stu-id="ff4f4-105">Create a blob storage container for thumbnails</span></span>

<span data-ttu-id="ff4f4-106">Die Bilder mit vollständiger Größe werden in einem Container mit dem Namen **images** gespeichert.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-106">The full size images are stored in a container named **images**.</span></span> <span data-ttu-id="ff4f4-107">Sie benötigen einen weiteren Container, um die Miniaturansichten dieser Bilder zu speichern.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-107">You need another container to store thumbnails of those images.</span></span>

1. <span data-ttu-id="ff4f4-108">Stellen Sie sicher, dass Sie noch an der Cloud Shell (Bash) angemeldet sind.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-108">Ensure you are still signed in to the Cloud Shell (bash).</span></span>  <span data-ttu-id="ff4f4-109">Wählen Sie andernfalls die Option **Enter focus mode** (Fokusmodus aktivieren), um ein Cloud Shell-Fenster zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-109">If not, select **Enter focus mode** to open a Cloud Shell window.</span></span> 

1. <span data-ttu-id="ff4f4-110">Erstellen Sie unter Ihrem Speicherkonto einen neuen Container mit dem Namen **thumbnails**, der über öffentlichen Zugriff auf alle Blobs verfügt.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-110">Create a new container named **thumbnails** in your storage account with public access to all blobs.</span></span>

    ```azurecli
    az storage container create -n thumbnails --account-name <storage account name> --public-access blob
    ```


## <a name="create-a-blob-triggered-serverless-function"></a><span data-ttu-id="ff4f4-111">Erstellen einer per Blob ausgelösten serverlosen Funktion</span><span class="sxs-lookup"><span data-stu-id="ff4f4-111">Create a blob-triggered serverless function</span></span>

<span data-ttu-id="ff4f4-112">Ein Trigger definiert, wie eine Funktion aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-112">A trigger defines how a function is invoked.</span></span> <span data-ttu-id="ff4f4-113">Für die Funktion, die Sie als Nächstes erstellen, wird ein Blobtrigger verwendet: Die Funktion wird automatisch aufgerufen, wenn ein Blob (Bilddatei) in den Container **images** hochgeladen wird.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-113">The function you create next uses a blob trigger: the function is automatically invoked when a blob (image file) is uploaded to the **images** container.</span></span> <span data-ttu-id="ff4f4-114">Eine Funktion muss über einen Trigger verfügen.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-114">A function must have one trigger.</span></span> <span data-ttu-id="ff4f4-115">Trigger haben zugeordnete Daten, in üblicherweise mit der Nutzlast identisch sind, von der die Funktion ausgelöst wurde.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-115">Triggers have associated data, which is usually the payload that triggered the function.</span></span>

<span data-ttu-id="ff4f4-116">Mit Bindungen wird definiert, wie eine Funktion das Lesen oder Schreiben von Daten in Azure oder Drittanbieterdiensten durchführt.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-116">Bindings define how a function reads or writes data in Azure or third-party services.</span></span> <span data-ttu-id="ff4f4-117">Diese Funktion erstellt eine Miniaturansichtversion des Bilds, von dem sie ausgelöst wird, und speichert die Miniaturansicht im Container *thumbnails*.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-117">This function creates a thumbnail version of the image that triggers it and saves the thumbnail in a *thumbnails* container.</span></span>

1. <span data-ttu-id="ff4f4-118">Öffnen Sie Ihre Funktions-App im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-118">Open your function app in the Azure Portal.</span></span>

1. <span data-ttu-id="ff4f4-119">Bewegen Sie den Mauszeiger im linken Navigationsbereich des Fensters für die Funktions-App auf **Funktionen**, und klicken Sie auf **+**, um mit dem Erstellen einer neuen serverlosen Funktion zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-119">In the function app window's left hand navigation, hover over **Functions** and click **+** to start creating a new serverless function.</span></span> <span data-ttu-id="ff4f4-120">Klicken Sie bei der Anzeige einer Schnellstartseite auf **Benutzerdefinierte Funktion**, um eine Liste mit Funktionsvorlagen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-120">If a quickstart page appears, click **Custom function** to see a list of function templates.</span></span>

1. <span data-ttu-id="ff4f4-121">Suchen Sie nach der Vorlage **BlobTrigger**, und wählen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-121">Find the **BlobTrigger** template and select it.</span></span>

1. <span data-ttu-id="ff4f4-122">Verwenden Sie diese Werte, um eine Funktion zu erstellen, mit der beim Hochladen von Bildern Miniaturansichten erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-122">Use these values to create a function that creates thumbnails as images are uploaded.</span></span>

    | <span data-ttu-id="ff4f4-123">Einstellung</span><span class="sxs-lookup"><span data-stu-id="ff4f4-123">Setting</span></span>      |  <span data-ttu-id="ff4f4-124">Empfohlener Wert</span><span class="sxs-lookup"><span data-stu-id="ff4f4-124">Suggested value</span></span>   | <span data-ttu-id="ff4f4-125">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="ff4f4-125">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="ff4f4-126">**Sprache**</span><span class="sxs-lookup"><span data-stu-id="ff4f4-126">**Language**</span></span> | <span data-ttu-id="ff4f4-127">C# oder JavaScript</span><span class="sxs-lookup"><span data-stu-id="ff4f4-127">C# or JavaScript</span></span> | <span data-ttu-id="ff4f4-128">Wählen Sie Ihre bevorzugte Sprache aus.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-128">Choose your preferred language.</span></span> |
    | <span data-ttu-id="ff4f4-129">**Name Ihrer Funktion**</span><span class="sxs-lookup"><span data-stu-id="ff4f4-129">**Name your function**</span></span> | <span data-ttu-id="ff4f4-130">ResizeImage</span><span class="sxs-lookup"><span data-stu-id="ff4f4-130">ResizeImage</span></span> | <span data-ttu-id="ff4f4-131">Geben Sie diesen Namen genau wie hier angezeigt ein, damit die Anwendung die Funktion ermitteln kann.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-131">Type this name exactly as shown so the application can discover the function.</span></span> |
    | <span data-ttu-id="ff4f4-132">**Path**</span><span class="sxs-lookup"><span data-stu-id="ff4f4-132">**Path**</span></span> | <span data-ttu-id="ff4f4-133">images/{name}</span><span class="sxs-lookup"><span data-stu-id="ff4f4-133">images/{name}</span></span> | <span data-ttu-id="ff4f4-134">Führen Sie die Funktion aus, wenn im Container **images** eine Datei angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-134">Execute the function when a file appears in the **images** container.</span></span> |
    | <span data-ttu-id="ff4f4-135">**Speicherkontoinformationen**</span><span class="sxs-lookup"><span data-stu-id="ff4f4-135">**Storage account information**</span></span> | <span data-ttu-id="ff4f4-136">AZURE_STORAGE_CONNECTION_STRING</span><span class="sxs-lookup"><span data-stu-id="ff4f4-136">AZURE_STORAGE_CONNECTION_STRING</span></span> | <span data-ttu-id="ff4f4-137">Verwenden Sie die Umgebungsvariable, die zuvor mit der Verbindungszeichenfolge erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-137">Use the environment variable previously created with the connection string.</span></span> |

    ![Eingeben von Einstellungen für die neue Funktion](media/functions-first-serverless-web-app/3-new-function.png)

1. <span data-ttu-id="ff4f4-139">Klicken Sie auf **Erstellen**, um die Funktion zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-139">Click **Create** to create the function.</span></span>

1. <span data-ttu-id="ff4f4-140">Klicken Sie nach dem Erstellen der Funktion auf **Integrieren**, um die Trigger-, Eingabe- und Ausgabebindungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-140">When the function is created, click **Integrate** to view its trigger, input, and output bindings.</span></span>

1. <span data-ttu-id="ff4f4-141">Klicken Sie auf **Neue Ausgabe**, um eine neue Ausgabebindung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-141">Click **New output** to create a new output binding.</span></span>

    ![Auswählen von „Neue Ausgabe“ auf der Registerkarte „Integrieren“](media/functions-first-serverless-web-app/3-new-output.jpg)

1. <span data-ttu-id="ff4f4-143">Wählen Sie **Azure Blob Storage**, und klicken Sie auf **Auswählen**.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-143">Select **Azure Blob Storage** and click **Select**.</span></span> <span data-ttu-id="ff4f4-144">Unter Umständen müssen Sie nach unten scrollen, damit die Schaltfläche **Auswählen** angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-144">You may have to scroll down to see the **Select** button.</span></span>

    ![Auswählen von „Azure Blob Storage“](media/functions-first-serverless-web-app/3-storage-output.jpg)

1. <span data-ttu-id="ff4f4-146">Geben Sie die folgenden Werte ein.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-146">Enter the following values.</span></span>

    | <span data-ttu-id="ff4f4-147">Einstellung</span><span class="sxs-lookup"><span data-stu-id="ff4f4-147">Setting</span></span>      |  <span data-ttu-id="ff4f4-148">Empfohlener Wert</span><span class="sxs-lookup"><span data-stu-id="ff4f4-148">Suggested value</span></span>   | <span data-ttu-id="ff4f4-149">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="ff4f4-149">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="ff4f4-150">**Blobparametername**</span><span class="sxs-lookup"><span data-stu-id="ff4f4-150">**Blob parameter name**</span></span> | <span data-ttu-id="ff4f4-151">thumbnail</span><span class="sxs-lookup"><span data-stu-id="ff4f4-151">thumbnail</span></span> | <span data-ttu-id="ff4f4-152">Für die Funktion wird der Parameter mit diesem Namen verwendet, um die Miniaturansicht zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-152">The function uses the parameter with this name to write the thumbnail.</span></span> |
    | <span data-ttu-id="ff4f4-153">**Funktionsrückgabewert verwenden**</span><span class="sxs-lookup"><span data-stu-id="ff4f4-153">**Use function return value**</span></span> | <span data-ttu-id="ff4f4-154">Nein </span><span class="sxs-lookup"><span data-stu-id="ff4f4-154">No</span></span> |  |
    | <span data-ttu-id="ff4f4-155">**Path**</span><span class="sxs-lookup"><span data-stu-id="ff4f4-155">**Path**</span></span> | <span data-ttu-id="ff4f4-156">thumbnails/{name}</span><span class="sxs-lookup"><span data-stu-id="ff4f4-156">thumbnails/{name}</span></span> | <span data-ttu-id="ff4f4-157">Die Miniaturansichten werden in einen Container mit dem Namen **thumbnails** geschrieben.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-157">The thumbnails are written to a container named **thumbnails**.</span></span> |
    | <span data-ttu-id="ff4f4-158">**Speicherkontoverbindung**</span><span class="sxs-lookup"><span data-stu-id="ff4f4-158">**Storage account connection**</span></span> | <span data-ttu-id="ff4f4-159">AZURE_STORAGE_CONNECTION_STRING</span><span class="sxs-lookup"><span data-stu-id="ff4f4-159">AZURE_STORAGE_CONNECTION_STRING</span></span> | <span data-ttu-id="ff4f4-160">Verwenden Sie die Umgebungsvariable, die zuvor mit der Verbindungszeichenfolge erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-160">Use the environment variable previously created with the connection string.</span></span> |

    ![Eingeben der Einstellungen für die Blobausgabebindung](media/functions-first-serverless-web-app/3-blob-output.png)

1. <span data-ttu-id="ff4f4-162">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="ff4f4-162">**JavaScript**</span></span>

    1. <span data-ttu-id="ff4f4-163">(JavaScript) Klicken Sie oben rechts im Fenster auf **Erweiterter Editor**, um den JSON-Code für die Bindungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-163">(JavaScript) Click on **Advanced editor** in the top right corner of the window to reveal the JSON representing the bindings.</span></span>

    1. <span data-ttu-id="ff4f4-164">(JavaScript) Fügen Sie in der Bindung `blobTrigger` eine Eigenschaft mit dem Namen `dataType` und dem Wert `binary` hinzu.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-164">(JavaScript) In the `blobTrigger` binding, add a property named `dataType` with a value of `binary`.</span></span> <span data-ttu-id="ff4f4-165">Hiermit wird die Bindung so konfiguriert, dass der Blobinhalt in Form von binären Daten an die Funktion übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-165">This configures the binding to pass the blob contents to the function as binary data.</span></span>

    ```json
    {
      "name": "myBlob",
      "type": "blobTrigger",
      "direction": "in",
      "path": "images/{name}",
      "connection": "AZURE_STORAGE_CONNECTION_STRING",
      "dataType": "binary"
    }
    ```

1. <span data-ttu-id="ff4f4-166">Klicken Sie auf **Speichern**, um die neue Bindung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-166">Click **Save** to create the new binding.</span></span>

1. <span data-ttu-id="ff4f4-167">**C#**</span><span class="sxs-lookup"><span data-stu-id="ff4f4-167">**C#**</span></span>

    1. <span data-ttu-id="ff4f4-168">(C#) Wählen Sie im linken Navigationsbereich den Funktionsnamen **ResizeImage** aus, um den Quellcode der Funktion zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-168">(C#) Select the **ResizeImage** function name on the left navigation to open the function's source code.</span></span>

    1. <span data-ttu-id="ff4f4-169">(C#) Für die Funktion ist ein NuGet-Paket mit dem Namen **ImageResizer** erforderlich, um die Miniaturansichten zu generieren.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-169">(C#) The function requires a NuGet package called **ImageResizer** to generate the thumbnails.</span></span> <span data-ttu-id="ff4f4-170">NuGet-Pakete werden C#-Funktionen über die Datei **project.json** hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-170">NuGet packages are added to C# functions using a **project.json** file.</span></span> <span data-ttu-id="ff4f4-171">Klicken Sie zum Erstellen der Datei rechts auf **Dateien anzeigen**, um die Dateien anzuzeigen, aus denen die Funktion besteht.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-171">To create the file, click **View Files** on the right to reveal the files that make up the function.</span></span>
    
    1. <span data-ttu-id="ff4f4-172">(C#) Klicken Sie auf **Hinzufügen**, um eine neue Datei mit dem Namen **project.json** hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-172">(C#) Click **Add** to add a new file named **project.json**.</span></span>
    
    1. <span data-ttu-id="ff4f4-173">(C#) Kopieren Sie den Inhalt von [**/csharp/ResizeImage/project.json**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/project.json) in die neu erstellte Datei.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-173">(C#) Copy the contents of [**/csharp/ResizeImage/project.json**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/project.json) into the newly created file.</span></span> <span data-ttu-id="ff4f4-174">Speichern Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-174">Save the file.</span></span> <span data-ttu-id="ff4f4-175">Pakete werden automatisch wiederhergestellt, wenn die Datei aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-175">Packages are automatically restored when the file is updated.</span></span>
    
        ![Datei „project.json“ mit ImageResizer](media/functions-first-serverless-web-app/3-project-json.png)
    
    1. <span data-ttu-id="ff4f4-177">(C#) Wählen Sie unter **Dateien anzeigen** die Datei **run.csx** aus, und ersetzen Sie den Inhalt durch den Inhalt von [**/csharp/ResizeImage/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run.csx).</span><span class="sxs-lookup"><span data-stu-id="ff4f4-177">(C#) Select **run.csx** under **View Files** and replace its content with the content in [**/csharp/ResizeImage/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run.csx).</span></span>

1. <span data-ttu-id="ff4f4-178">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="ff4f4-178">**JavaScript**</span></span> 

    1. <span data-ttu-id="ff4f4-179">(JavaScript) Für diese Funktion ist das `jimp`-Paket von npm erforderlich, um die Größe des Fotos zu ändern.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-179">(JavaScript) This function requires the `jimp` package from npm to resize the photo.</span></span> <span data-ttu-id="ff4f4-180">Klicken Sie zum Installieren des npm-Pakets im linken Navigationsbereich auf den Namen der Funktions-App und dann auf **Plattformfeatures**.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-180">To install the npm package, click on the Function App's name on the left navigation and click **Platform features**.</span></span>

    1. <span data-ttu-id="ff4f4-181">(JavaScript) Klicken Sie auf **Konsole**, um ein Konsolenfenster anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-181">(JavaScript) Click **Console** to reveal a console window.</span></span>

    1. <span data-ttu-id="ff4f4-182">(JavaScript) Führen Sie den Befehl `npm install jimp` in der Konsole aus.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-182">(JavaScript) Run the command `npm install jimp` in the console.</span></span> <span data-ttu-id="ff4f4-183">Es kann ein oder zwei Minuten dauern, bis der Vorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-183">It may take a minute or two to complete the operation.</span></span>

    1. <span data-ttu-id="ff4f4-184">(JavaScript) Klicken Sie im linken Navigationsbereich auf den Namen der Funktion **ResizeImage**, um die Funktion anzuzeigen, und ersetzen Sie dann den gesamten Inhalt von **index.js** durch den Inhalt von [**/javascript/ResizeImage/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index.js).</span><span class="sxs-lookup"><span data-stu-id="ff4f4-184">(JavaScript) Click on the **ResizeImage** function name in the left navigation to reveal the function, replace all of **index.js** with the content of [**/javascript/ResizeImage/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index.js).</span></span>

1. <span data-ttu-id="ff4f4-185">Klicken Sie unter dem Codefenster auf **Protokolle**, um den Protokollbereich zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-185">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="ff4f4-186">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-186">Click **Save**.</span></span> <span data-ttu-id="ff4f4-187">Überprüfen Sie den Protokollbereich, um sicherzustellen, dass die Funktion erfolgreich gespeichert wird und keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-187">Check the logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="test-the-serverless-function"></a><span data-ttu-id="ff4f4-188">Testen der serverlosen Funktion</span><span class="sxs-lookup"><span data-stu-id="ff4f4-188">Test the serverless function</span></span>

1. <span data-ttu-id="ff4f4-189">Öffnen Sie die Anwendung in einem Browser.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-189">Open the application in a browser.</span></span> <span data-ttu-id="ff4f4-190">Wählen Sie eine Bilddatei aus, und laden Sie sie hoch.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-190">Select an image file and upload it.</span></span> <span data-ttu-id="ff4f4-191">Der Upload wird abgeschlossen. Da wir aber noch nicht die Möglichkeit zum Anzeigen von Bildern hinzugefügt haben, wird das hochgeladene Bild in der App nicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-191">The upload completes, but because we haven't added the ability to display images yet, the app doesn't show the uploaded photo.</span></span>

1. <span data-ttu-id="ff4f4-192">Vergewissern Sie sich in der Cloud Shell, dass das Bild in den Container **images** hochgeladen wurde.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-192">In the Cloud Shell, confirm the image was uploaded to the **images** container.</span></span>

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. <span data-ttu-id="ff4f4-193">Vergewissern Sie sich, dass die Miniaturansicht in einem Container mit dem Namen **thumbnails** erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-193">Confirm the thumbnail was created in a container named **thumbnails**.</span></span>

    ```azurecli
    az storage blob list --account-name <storage account name> -c thumbnails -o table
    ```

1. <span data-ttu-id="ff4f4-194">Rufen Sie die URL der Miniaturansicht ab.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-194">Obtain the thumbnail's URL.</span></span>

    ```azurecli
    az storage blob url --account-name <storage account name> -c thumbnails -n <filename> --output tsv
    ```

    <span data-ttu-id="ff4f4-195">Öffnen Sie die URL in einem Browser, und stellen Sie sicher, dass die Miniaturansicht richtig erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-195">Open the URL in a browser and confirm the thumbnail was properly created.</span></span>

1. <span data-ttu-id="ff4f4-196">Löschen Sie alle Dateien in den Containern **images** und **thumbnails**, bevor Sie mit dem nächsten Tutorial fortfahren.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-196">Before moving on to the next tutorial, delete all files in the **images** and **thumbnails** containers.</span></span>

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```
    ```azurecli
    az storage blob delete-batch -s thumbnails --account-name <storage account name>
    ```


## <a name="summary"></a><span data-ttu-id="ff4f4-197">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="ff4f4-197">Summary</span></span>

<span data-ttu-id="ff4f4-198">In diesem Modul haben Sie eine serverlose Funktion erstellt, um eine Miniaturansicht zu erstellen, wenn ein Bild in einen Blobspeichercontainer hochgeladen wird.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-198">In this module, you created a serverless function to create a thumbnail whenever an image is uploaded to a Blob storage container.</span></span> <span data-ttu-id="ff4f4-199">Als Nächstes erfahren Sie, wie Sie Azure Cosmos DB zum Speichern und Auflisten von Bildmetadaten verwenden.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-199">Next, you learn how to use Azure Cosmos DB to store and list image metadata.</span></span>

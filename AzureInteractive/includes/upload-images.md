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
ms.openlocfilehash: 56cfb4c2893977086309660f4f6941fd0d648913
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079474"
---
<span data-ttu-id="31148-103">Bei der von Ihnen erstellten Anwendung handelt es sich um eine Fotogalerie.</span><span class="sxs-lookup"><span data-stu-id="31148-103">The application that you're building is a photo gallery.</span></span> <span data-ttu-id="31148-104">Es wird clientseitiger JavaScript-Code verwendet, um APIs zum Hochladen und Anzeigen von Bildern aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="31148-104">It uses client-side JavaScript to call APIs to upload and display images.</span></span> <span data-ttu-id="31148-105">In diesem Modul erstellen Sie eine API, indem Sie eine serverlose Funktion verwenden, mit der eine URL mit zeitlicher Beschränkung zum Hochladen eines Bilds generiert wird.</span><span class="sxs-lookup"><span data-stu-id="31148-105">In this module, you create an API using a serverless function that generates a time-limited URL to upload an image.</span></span> <span data-ttu-id="31148-106">Die Webanwendung nutzt die generierte URL zum Hochladen eines Bilds in den Blobspeicher mit der [Blob-Dienst-REST-API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).</span><span class="sxs-lookup"><span data-stu-id="31148-106">The web application uses the generated URL to upload an image to Blob storage using the [Blob storage REST API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).</span></span>

## <a name="create-a-blob-storage-container-for-images"></a><span data-ttu-id="31148-107">Erstellen eines Blobspeichercontainers für Bilder</span><span class="sxs-lookup"><span data-stu-id="31148-107">Create a blob storage container for images</span></span>

<span data-ttu-id="31148-108">Für die Anwendung ist ein separater Speichercontainer erforderlich, um Bilder hochladen und hosten zu können.</span><span class="sxs-lookup"><span data-stu-id="31148-108">The application requires a separate storage container to upload and host images.</span></span>

1. <span data-ttu-id="31148-109">Stellen Sie sicher, dass Sie noch an der Cloud Shell (Bash) angemeldet sind.</span><span class="sxs-lookup"><span data-stu-id="31148-109">Ensure you are still signed in to the Cloud Shell (bash).</span></span> <span data-ttu-id="31148-110">Wählen Sie andernfalls die Option **Enter focus mode** (Fokusmodus aktivieren), um ein Cloud Shell-Fenster zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="31148-110">If not, select **Enter focus mode** to open a Cloud Shell window.</span></span>

1.  <span data-ttu-id="31148-111">Erstellen Sie unter Ihrem Speicherkonto einen neuen Container mit dem Namen **images**, der über öffentlichen Zugriff auf alle Blobs verfügt.</span><span class="sxs-lookup"><span data-stu-id="31148-111">Create a new container named **images** in your storage account with public access to all blobs.</span></span>

    ```azurecli
    az storage container create -n images --account-name <storage account name> --public-access blob
    ```

## <a name="create-an-azure-function-app"></a><span data-ttu-id="31148-112">Erstellen einer Azure-Funktions-App</span><span class="sxs-lookup"><span data-stu-id="31148-112">Create an Azure Function app</span></span>

<span data-ttu-id="31148-113">Azure Functions ist ein Dienst zum Ausführen von serverlosen Funktionen.</span><span class="sxs-lookup"><span data-stu-id="31148-113">Azure Functions is a service for running serverless functions.</span></span> <span data-ttu-id="31148-114">Eine serverlose Funktion kann von Ereignissen, z.B. einer HTTP-Anforderung, oder bei der Erstellung eines Blobs in einem Speichercontainer ausgelöst (aufgerufen) werden.</span><span class="sxs-lookup"><span data-stu-id="31148-114">A serverless function can be triggered (called) by events such as an HTTP request or when a blob is created in a storage container.</span></span>

<span data-ttu-id="31148-115">Eine Azure-Funktions-App ist ein Container für mindestens eine serverlose Funktion.</span><span class="sxs-lookup"><span data-stu-id="31148-115">An Azure Function app is a container for one or more serverless functions.</span></span>

1. <span data-ttu-id="31148-116">Erstellen Sie eine neue Azure-Funktions-App mit einem eindeutigen Namen in der Ressourcengruppe, die Sie zuvor erstellt haben. Verwenden Sie den Namen **first-serverless-app**.</span><span class="sxs-lookup"><span data-stu-id="31148-116">Create a new Azure Function app with a unique name in the resource group you created earlier named **first-serverless-app**.</span></span> <span data-ttu-id="31148-117">Für Funktions-Apps ist ein Storage-Konto erforderlich. In diesem Tutorial verwenden Sie das vorhandene Speicherkonto.</span><span class="sxs-lookup"><span data-stu-id="31148-117">Function apps require a Storage account; in this tutorial, you use the existing storage account.</span></span>

    ```azurecli
    az functionapp create -n <function app name> -g first-serverless-app -s <storage account name> -c westcentralus
    ```


## <a name="create-an-http-triggered-serverless-function"></a><span data-ttu-id="31148-118">Erstellen einer per HTTP ausgelösten serverlosen Funktion</span><span class="sxs-lookup"><span data-stu-id="31148-118">Create an HTTP-triggered serverless function</span></span>

<span data-ttu-id="31148-119">Die Fotogalerie-Webanwendung sendet eine HTTP-Anforderung an die serverlose Funktion, um eine URL mit zeitlicher Beschränkung zu generieren, damit ein Bild auf sichere Weise in den Blobspeicher hochgeladen werden kann.</span><span class="sxs-lookup"><span data-stu-id="31148-119">The photo gallery web application makes an HTTP request to serverless function to generate a time-limited URL to securely upload an image to Blob storage.</span></span> <span data-ttu-id="31148-120">Die Funktion wird per HTTP-Anforderung ausgelöst, und das Azure Storage SDK wird verwendet, um die sichere URL zu generieren und zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="31148-120">The function is triggered by an HTTP request and uses the Azure Storage SDK to generate and return the secure URL.</span></span>

1. <span data-ttu-id="31148-121">Nachdem die Funktions-App erstellt wurde, können Sie im Azure-Portal über das Feld „Suchen“ danach suchen und sie durch Daraufklicken öffnen.</span><span class="sxs-lookup"><span data-stu-id="31148-121">After the Function app is created, search for it in the Azure Portal using the Search box and click to open it.</span></span>

    ![Öffnen der Funktions-App](media/functions-first-serverless-web-app/2-search-function-app.png)

1. <span data-ttu-id="31148-123">Bewegen Sie den Mauszeiger im linken Navigationsbereich des Fensters für die Funktions-App auf **Funktionen**, und klicken Sie auf **+**, um mit dem Erstellen einer neuen serverlosen Funktion zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="31148-123">In the function app window's left hand navigation, hover over **Functions** and click **+** to start creating a new serverless function.</span></span>

    ![Erstellen einer neuen Funktion](media/functions-first-serverless-web-app/2-new-function.png)

1. <span data-ttu-id="31148-125">Klicken Sie auf **Benutzerdefinierte Funktion**, um eine Liste mit Funktionsvorlagen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="31148-125">Click **Custom function** to see a list of function templates.</span></span>

1. <span data-ttu-id="31148-126">Suchen Sie nach der Vorlage **HttpTrigger**, und klicken Sie auf die gewünschte Sprache (C# oder JavaScript).</span><span class="sxs-lookup"><span data-stu-id="31148-126">Find the **HttpTrigger** template and click the language to use (C# or JavaScript).</span></span>

1. <span data-ttu-id="31148-127">Verwenden Sie diese Werte, um eine Funktion zu erstellen, mit der eine URL für den Blob-Upload generiert wird.</span><span class="sxs-lookup"><span data-stu-id="31148-127">Use these values to create a function that generates a blob upload URL.</span></span>

    | <span data-ttu-id="31148-128">Einstellung</span><span class="sxs-lookup"><span data-stu-id="31148-128">Setting</span></span>      |  <span data-ttu-id="31148-129">Empfohlener Wert</span><span class="sxs-lookup"><span data-stu-id="31148-129">Suggested value</span></span>   | <span data-ttu-id="31148-130">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="31148-130">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="31148-131">**Sprache**</span><span class="sxs-lookup"><span data-stu-id="31148-131">**Language**</span></span> | <span data-ttu-id="31148-132">C# oder JavaScript</span><span class="sxs-lookup"><span data-stu-id="31148-132">C# or JavaScript</span></span> | <span data-ttu-id="31148-133">Wählen Sie die Sprache aus, die Sie verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="31148-133">Select the language you want to use.</span></span> |
    | <span data-ttu-id="31148-134">**Name Ihrer Funktion**</span><span class="sxs-lookup"><span data-stu-id="31148-134">**Name your function**</span></span> | <span data-ttu-id="31148-135">GetUploadUrl</span><span class="sxs-lookup"><span data-stu-id="31148-135">GetUploadUrl</span></span> | <span data-ttu-id="31148-136">Geben Sie diesen Namen genau wie hier angezeigt ein, damit die Anwendung die Funktion ermitteln kann.</span><span class="sxs-lookup"><span data-stu-id="31148-136">Type this name exactly as shown so the application can discover the function.</span></span> |
    | <span data-ttu-id="31148-137">**Autorisierungsstufe**</span><span class="sxs-lookup"><span data-stu-id="31148-137">**Authorization level**</span></span> | <span data-ttu-id="31148-138">Anonym</span><span class="sxs-lookup"><span data-stu-id="31148-138">Anonymous</span></span> | <span data-ttu-id="31148-139">Lässt den schnellen Zugriff auf die Funktion zu.</span><span class="sxs-lookup"><span data-stu-id="31148-139">Allow the function to be accessed publicly.</span></span> |

    ![Eingeben der Einstellungen für eine neue Funktion, die per HTTP ausgelöst wird](media/functions-first-serverless-web-app/2-new-function-httptrigger.png)

1. <span data-ttu-id="31148-141">Klicken Sie auf **Erstellen**, um die Funktion zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="31148-141">Click **Create** to create the function.</span></span>

1. <span data-ttu-id="31148-142">**C#**</span><span class="sxs-lookup"><span data-stu-id="31148-142">**C#**</span></span> 

    1. <span data-ttu-id="31148-143">Gehen Sie wie folgt vor, wenn der Quellcode der Funktion angezeigt wird: Ersetzen Sie den gesamten Inhalt von **run.csx** durch den Inhalt von [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx).</span><span class="sxs-lookup"><span data-stu-id="31148-143">When the function's source code appears, replace all of **run.csx** with the content of [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx).</span></span>

1. <span data-ttu-id="31148-144">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="31148-144">**JavaScript**</span></span> 

    1. <span data-ttu-id="31148-145">(JavaScript) Für diese Funktion ist das `azure-storage`-Paket aus npm erforderlich, um das SAS-Token (Shared Access Signature) zu generieren, das zum Erstellen der sicheren URL benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="31148-145">(JavaScript) This function requires the `azure-storage` package from npm to generate the shared access signature (SAS) token required to build the secure URL.</span></span> <span data-ttu-id="31148-146">Klicken Sie zum Installieren des npm-Pakets im linken Navigationsbereich auf den Namen der Funktions-App und dann auf **Plattformfeatures**.</span><span class="sxs-lookup"><span data-stu-id="31148-146">To install the npm package, click on the Function App's name on the left navigation and click **Platform features**.</span></span>

    1. <span data-ttu-id="31148-147">(JavaScript) Klicken Sie auf **Konsole**, um ein Konsolenfenster anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="31148-147">(JavaScript) Click **Console** to reveal a console window.</span></span>

        ![Öffnen eines Konsolenfensters](media/functions-first-serverless-web-app/2-open-console.jpg)

    1. <span data-ttu-id="31148-149">(JavaScript) Vergewissern Sie sich, dass **d:\home\site\wwwroot** das aktuelle Verzeichnis ist, indem Sie den Befehl `cd d:\home\site\wwwroot` ausführen.</span><span class="sxs-lookup"><span data-stu-id="31148-149">(JavaScript) Ensure the current directory is **d:\home\site\wwwroot** by running the command `cd d:\home\site\wwwroot`.</span></span>

    1. <span data-ttu-id="31148-150">(JavaScript) Führen Sie den Befehl `npm init -y` aus, um eine leere Datei mit dem Namen **package.json** zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="31148-150">(JavaScript) Run the command `npm init -y` to create an empty **package.json** file.</span></span>

    1. <span data-ttu-id="31148-151">(JavaScript) Führen Sie den Befehl `npm install --save azure-storage` in der Konsole aus, um das Paket zu installieren und in **package.json** zu speichern.</span><span class="sxs-lookup"><span data-stu-id="31148-151">(JavaScript) Run the command `npm install --save azure-storage` in the console to install the package and save it in **package.json**.</span></span> <span data-ttu-id="31148-152">Es kann ein oder zwei Minuten dauern, bis der Vorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="31148-152">It may take a minute or two to complete the operation.</span></span>

    1. <span data-ttu-id="31148-153">(JavaScript) Klicken Sie im linken Navigationsbereich auf den Namen der Funktion (**GetUploadUrl**), um die Funktion anzuzeigen. Ersetzen Sie den gesamten Inhalt von **index.js** durch den Inhalt von [**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js).</span><span class="sxs-lookup"><span data-stu-id="31148-153">(JavaScript) Click on the function name (**GetUploadUrl**) in the left navigation to reveal the function, replace all of **index.js** with the content of [**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js).</span></span>
    
        ![„index.js“ nach dem Update](media/functions-first-serverless-web-app/2-paste-js.jpg)

1. <span data-ttu-id="31148-155">Klicken Sie unter dem Codefenster auf **Protokolle**, um den Protokollbereich zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="31148-155">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="31148-156">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="31148-156">Click **Save**.</span></span> <span data-ttu-id="31148-157">Überprüfen Sie den Protokollbereich, um sicherzustellen, dass die Funktion erfolgreich kompiliert wird.</span><span class="sxs-lookup"><span data-stu-id="31148-157">Check the logs panel to ensure the function is successfully compiled.</span></span>

<span data-ttu-id="31148-158">Mit der Funktion wird eine so genannte SAS-URL (Shared Access Signature) generiert, die zum Hochladen einer Datei in Blobspeicher genutzt wird.</span><span class="sxs-lookup"><span data-stu-id="31148-158">The function generates what is called a shared access signature (SAS) URL that is used to upload a file to Blob storage.</span></span> <span data-ttu-id="31148-159">Die SAS-URL ist nur für kurze Zeit gültig, und es kann nur eine Datei hochgeladen werden.</span><span class="sxs-lookup"><span data-stu-id="31148-159">The SAS URL is valid for a short period of time and only allows a single file to be uploaded.</span></span> <span data-ttu-id="31148-160">Die Dokumentation zum Blobspeicher enthält weitere Informationen zur [Verwendung von Shared Access Signatures](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).</span><span class="sxs-lookup"><span data-stu-id="31148-160">Consult the Blob storage documentation for more information on [using shared access signatures](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).</span></span>


## <a name="add-an-environment-variable-for-the-storage-connection-string"></a><span data-ttu-id="31148-161">Hinzufügen einer Umgebungsvariablen für die Speicherverbindungszeichenfolge</span><span class="sxs-lookup"><span data-stu-id="31148-161">Add an environment variable for the storage connection string</span></span>

<span data-ttu-id="31148-162">Für die erstellte Funktion wird eine Verbindungszeichenfolge für das Speicherkonto benötigt, damit die SAS-URL generiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="31148-162">The function you just created requires a connection string for the Storage account so that it can generate the SAS URL.</span></span> <span data-ttu-id="31148-163">Anstatt die Verbindungszeichenfolge im Text der Funktion hartzucodieren, kann sie als Anwendungseinstellung gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="31148-163">Instead of hardcoding the connection string in the function's body, it can be stored as an application setting.</span></span> <span data-ttu-id="31148-164">Auf Anwendungseinstellungen kann von allen Funktionen der Funktions-App in Form von Umgebungsvariablen zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="31148-164">Application settings are accessible as environment variables by all functions in the Function app.</span></span>

1. <span data-ttu-id="31148-165">Fragen Sie in der Cloud Shell die Speicherkonto-Verbindungszeichenfolge ab, und speichern Sie sie in einer Bash-Variablen mit dem Namen **STORAGE_CONNECTION_STRING**.</span><span class="sxs-lookup"><span data-stu-id="31148-165">In the Cloud Shell, query the Storage account connection string and save it to a bash variable named **STORAGE_CONNECTION_STRING**.</span></span>

    ```azurecli
    export STORAGE_CONNECTION_STRING=$(az storage account show-connection-string -n <storage account name> -g first-serverless-app --query "connectionString" --output tsv)
    ```

    <span data-ttu-id="31148-166">Vergewissern Sie sich, dass das Festlegen der Variablen erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="31148-166">Confirm the variable is set successfully.</span></span>

    ```azurecli
    echo $STORAGE_CONNECTION_STRING
    ```

1. <span data-ttu-id="31148-167">Erstellen Sie eine neue Anwendungseinstellung mit dem Namen **AZURE_STORAGE_CONNECTION_STRING**, indem Sie den im vorherigen Schritt gespeicherten Wert verwenden.</span><span class="sxs-lookup"><span data-stu-id="31148-167">Create a new application setting named **AZURE_STORAGE_CONNECTION_STRING** using the value saved from the previous step.</span></span>

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings AZURE_STORAGE_CONNECTION_STRING=$STORAGE_CONNECTION_STRING -o table
    ```

    <span data-ttu-id="31148-168">Stellen Sie sicher, dass die Ausgabe des Befehls die neue Anwendungseinstellung mit dem richtigen Wert enthält.</span><span class="sxs-lookup"><span data-stu-id="31148-168">Confirm that the command's output contains the new application setting with the correct value.</span></span>


## <a name="test-the-serverless-function"></a><span data-ttu-id="31148-169">Testen der serverlosen Funktion</span><span class="sxs-lookup"><span data-stu-id="31148-169">Test the serverless function</span></span>

<span data-ttu-id="31148-170">Zusätzlich zum Erstellen und Bearbeiten von Funktionen enthält das Azure-Portal auch ein integriertes Tool zum Testen von Funktionen.</span><span class="sxs-lookup"><span data-stu-id="31148-170">In addition to creating and editing functions, the Azure portal also provides an built-in tool for testing functions.</span></span>

1. <span data-ttu-id="31148-171">Klicken Sie zum Testen der serverlosen HTTP-Funktion rechts vom Codefenster auf die Registerkarte **Test** (Testen), um den Testbereich zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="31148-171">To test the HTTP serverless function, click on the **Test** tab on the right of the code window to expand the test panel.</span></span>

1. <span data-ttu-id="31148-172">Ändern Sie die **HTTP-Methode** in **GET**.</span><span class="sxs-lookup"><span data-stu-id="31148-172">Change the **Http method** to **GET**.</span></span>

1. <span data-ttu-id="31148-173">Klicken Sie unter **Abfrage** auf *Parameter hinzufügen*\*, und fügen Sie den folgenden Parameter hinzu:</span><span class="sxs-lookup"><span data-stu-id="31148-173">Under **Query**, click *Add parameter*\* and add the following parameter:</span></span>

    | <span data-ttu-id="31148-174">name</span><span class="sxs-lookup"><span data-stu-id="31148-174">name</span></span>      |  <span data-ttu-id="31148-175">value</span><span class="sxs-lookup"><span data-stu-id="31148-175">value</span></span>   | 
    | --- | --- |
    | <span data-ttu-id="31148-176">filename</span><span class="sxs-lookup"><span data-stu-id="31148-176">filename</span></span> | <span data-ttu-id="31148-177">image1.jpg</span><span class="sxs-lookup"><span data-stu-id="31148-177">image1.jpg</span></span> |

1. <span data-ttu-id="31148-178">Klicken Sie im Testbereich auf **Ausführen**, um eine HTTP-Anforderung an die Funktion zu senden.</span><span class="sxs-lookup"><span data-stu-id="31148-178">Click **Run** in the test panel to send an HTTP request to the function.</span></span>

1. <span data-ttu-id="31148-179">Die Funktion gibt in der Ausgabe eine Upload-URL zurück.</span><span class="sxs-lookup"><span data-stu-id="31148-179">The function returns an upload URL in the output.</span></span> <span data-ttu-id="31148-180">Die Funktionsausführung wird im Protokollbereich angezeigt.</span><span class="sxs-lookup"><span data-stu-id="31148-180">The function execution appears in the logs panel.</span></span>

    ![Protokolle mit Informationen zur erfolgreichen Ausführung der Funktion](media/functions-first-serverless-web-app/2-test-function.png)


## <a name="configure-cors-in-the-function-app"></a><span data-ttu-id="31148-182">Konfigurieren von CORS in der Funktions-App</span><span class="sxs-lookup"><span data-stu-id="31148-182">Configure CORS in the function app</span></span>

<span data-ttu-id="31148-183">Da das Front-End der App im Blobspeicher gehostet wird, verfügt es über einen anderen Domänennamen als die Azure-Funktions-App.</span><span class="sxs-lookup"><span data-stu-id="31148-183">Because the app's frontend is hosted in Blob storage, it has a different domain name than the Azure Function app.</span></span> <span data-ttu-id="31148-184">Damit der clientseitige JavaScript-Code die eben erstellte Funktion erfolgreich aufrufen kann, muss die Funktions-App für Cross-Origin Resource Sharing (CORS) konfiguriert sein.</span><span class="sxs-lookup"><span data-stu-id="31148-184">For the client-side JavaScript to successfully call the function you just created, the function app needs to be configured for cross-origin resource sharing (CORS).</span></span>

1. <span data-ttu-id="31148-185">Klicken Sie im Fenster der Funktions-App in der linken Navigationsleiste auf den Namen Ihrer Funktions-App.</span><span class="sxs-lookup"><span data-stu-id="31148-185">In the left navigation bar of the function app window, click on the name of your function app.</span></span>

1. <span data-ttu-id="31148-186">Klicken Sie auf **Plattformfeatures**, um eine Liste mit erweiterten Features anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="31148-186">Click on **Platform features** to view a list of advanced features.</span></span>

1. <span data-ttu-id="31148-187">Klicken Sie unter **API** auf **CORS**.</span><span class="sxs-lookup"><span data-stu-id="31148-187">Under **API**, click **CORS**.</span></span>

    ![Auswählen von CORS](media/functions-first-serverless-web-app/2-open-cors.jpg)

1. <span data-ttu-id="31148-189">Fügen Sie aus dem vorherigen Modul einen zulässigen Ursprung für die Anwendungs-URL hinzu, und lassen Sie den nachgestellten Schrägstrich (**/**) weg (z.B. `https://firstserverlessweb.z4.web.core.windows.net`).</span><span class="sxs-lookup"><span data-stu-id="31148-189">Add an allow origin for the application URL from the previous module, omitting the trailing **/** (for example: `https://firstserverlessweb.z4.web.core.windows.net`).</span></span>

    ![CORS-Einstellungen mit hinzugefügter URL für serverlose Web-App](media/functions-first-serverless-web-app/2-add-cors.png)

1. <span data-ttu-id="31148-191">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="31148-191">Click **Save**.</span></span>

1. <span data-ttu-id="31148-192">C#</span><span class="sxs-lookup"><span data-stu-id="31148-192">C#</span></span>

   1. <span data-ttu-id="31148-193">(C#) Navigieren Sie zurück zur Funktion `GetUploadUrl`, und wählen Sie dann die Registerkarte **Integrieren**.</span><span class="sxs-lookup"><span data-stu-id="31148-193">(C#) Navigate back to the `GetUploadUrl` function, and then select the **Integrate** tab.</span></span>

   1. <span data-ttu-id="31148-194">(C#) Wählen Sie unter „Ausgewählte HTTP-Methoden“ die Option **OPTIONS**.</span><span class="sxs-lookup"><span data-stu-id="31148-194">(C#) Under Selected HTTP methods, select **OPTIONS**.</span></span>

      <span data-ttu-id="31148-195">**GET**, **POST** und **OPTIONS** sollten jeweils ausgewählt sein.</span><span class="sxs-lookup"><span data-stu-id="31148-195">**GET**, **POST**, and **OPTIONS** should all be selected.</span></span> <span data-ttu-id="31148-196">Für CORS wird die OPTIONS-Methode verwendet, die für C#-Funktionen nicht standardmäßig ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="31148-196">CORS uses the OPTIONS method, which is not selected by default for C# functions.</span></span>  

   1. <span data-ttu-id="31148-197">(C#) Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="31148-197">(C#) Click **Save**.</span></span>

1. <span data-ttu-id="31148-198">Navigieren Sie im Portal zur Funktions-App, wählen Sie die Registerkarte **Übersicht**, und klicken Sie dann auf **Neu starten**, um sicherzustellen, dass die Änderungen für CORS wirksam sind.</span><span class="sxs-lookup"><span data-stu-id="31148-198">Still in the portal, navigate to the function app, select the **Overview** tab, and then click **Restart** to make sure that the changes for CORS take effect.</span></span>

## <a name="configure-cors-in-the-storage-account"></a><span data-ttu-id="31148-199">Konfigurieren von CORS im Storage-Konto</span><span class="sxs-lookup"><span data-stu-id="31148-199">Configure CORS in the Storage account</span></span>

<span data-ttu-id="31148-200">Da die App auch clientseitige JavaScript-Aufrufe zum Hochladen von Dateien an Blob Storage sendet, müssen Sie außerdem das Storage-Konto für CORS konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="31148-200">Because the app also makes client-side JavaScript calls to Blob Storage to upload files, you also have to configure the Storage account for CORS.</span></span>

1. <span data-ttu-id="31148-201">Führen Sie den folgenden Befehl aus, um für alle Ursprungsorte das Hochladen in das Storage-Konto zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="31148-201">Run the following command to allow all origins to upload files to the Storage account.</span></span>

    ```azurecli
    az storage cors add --methods OPTIONS PUT --origins '*' --exposed-headers '*' --allowed-headers '*' --services b --account-name <storage account name>
    ```


## <a name="modify-the-web-app-to-upload-images"></a><span data-ttu-id="31148-202">Ändern der Web-App zum Hochladen von Bildern</span><span class="sxs-lookup"><span data-stu-id="31148-202">Modify the web app to upload images</span></span>

<span data-ttu-id="31148-203">Die Web-App ruft Einstellungen aus einer Datei mit dem Namen **settings.js** ab.</span><span class="sxs-lookup"><span data-stu-id="31148-203">The web app retrieves settings from a file named **settings.js**.</span></span> <span data-ttu-id="31148-204">In den folgenden Schritten erstellen Sie die Datei mit der Cloud Shell und legen dann `window.apiBaseUrl` auf die URL der Funktions-App und `window.blobBaseUrl` auf die URL des Azure Blob Storage-Endpunkts fest.</span><span class="sxs-lookup"><span data-stu-id="31148-204">In the following steps, you create the file using Cloud Shell, then set `window.apiBaseUrl` to the URL of the Function app and `window.blobBaseUrl` to the URL of the Azure Blob Storage endpoint.</span></span>

1. <span data-ttu-id="31148-205">Stellen Sie in der Cloud Shell sicher, dass das aktuelle Verzeichnis der Ordner **www/dist** ist.</span><span class="sxs-lookup"><span data-stu-id="31148-205">In the Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="31148-206">Fragen Sie die URL der Funktions-App ab, und speichern Sie sie in einer Bash-Variablen mit dem Namen **FUNCTION_APP_URL**.</span><span class="sxs-lookup"><span data-stu-id="31148-206">Query the function app's URL and store it in a bash variable named **FUNCTION_APP_URL**.</span></span>

    ```azurecli
    export FUNCTION_APP_URL="https://"$(az functionapp show -n <function app name> -g first-serverless-app --query "defaultHostName" --output tsv)
    ```

    <span data-ttu-id="31148-207">Vergewissern Sie sich, dass die Variable richtig festgelegt wurde.</span><span class="sxs-lookup"><span data-stu-id="31148-207">Confirm the variable is correctly set.</span></span>

    ```azurecli
    echo $FUNCTION_APP_URL
    ```

1. <span data-ttu-id="31148-208">Erstellen Sie die Datei **settings.js**, und fügen Sie die Funktions-App-URL beispielsweise wie unten angegeben hinzu, um den Basis-URI der API-Aufrufe für Ihre Funktions-App festzulegen.</span><span class="sxs-lookup"><span data-stu-id="31148-208">To set the base URI of API calls to your function app, create **settings.js** and add the function app URL like the following.</span></span>

    `window.apiBaseUrl = 'https://fnapp@lab.GlobalLabInstanceId.azurewebsites.net'`

    <span data-ttu-id="31148-209">Sie können die Änderung vornehmen, indem Sie den folgenden Befehl ausführen oder einen Befehlszeilen-Editor wie VIM verwenden.</span><span class="sxs-lookup"><span data-stu-id="31148-209">You can make the change by running the following command or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.apiBaseUrl = '$FUNCTION_APP_URL'" > settings.js
    ```

    <span data-ttu-id="31148-210">Vergewissern Sie sich, dass das Schreiben der Datei erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="31148-210">Confirm the file was successfully written.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="31148-211">Fragen Sie die Blob Storage-Basis-URL ab, und speichern Sie sie in einer Bash-Variablen mit dem Namen **BLOB_BASE_URL**.</span><span class="sxs-lookup"><span data-stu-id="31148-211">Query the Blob Storage base URL and store it in a bash variable named **BLOB_BASE_URL**.</span></span>

    ```azurecli
    export BLOB_BASE_URL=$(az storage account show -n <storage account name> -g first-serverless-app --query primaryEndpoints.blob -o tsv | sed 's/\/$//')
    ```

    <span data-ttu-id="31148-212">Vergewissern Sie sich, dass die Variable richtig festgelegt wurde.</span><span class="sxs-lookup"><span data-stu-id="31148-212">Confirm the variable is correctly set.</span></span>

    ```azurecli
    echo $BLOB_BASE_URL
    ```

1. <span data-ttu-id="31148-213">Fügen Sie die Speicher-URL an die Datei **settings.js** an (beispielsweise mit der folgenden Codezeile), um den Basis-URI der API-Aufrufe für Ihre Funktions-App festzulegen.</span><span class="sxs-lookup"><span data-stu-id="31148-213">To set the base URI of API calls to your function app, append the storage URL like the following line of code to **settings.js**.</span></span>

    `window.blobBaseUrl = 'https://mystorage.blob.core.windows.net'`

    <span data-ttu-id="31148-214">Sie können die Änderung vornehmen, indem Sie den folgenden Befehl ausführen oder einen Befehlszeilen-Editor wie VIM verwenden.</span><span class="sxs-lookup"><span data-stu-id="31148-214">You can make the change by running the following command or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.blobBaseUrl = '$BLOB_BASE_URL'" >> settings.js
    ```

    <span data-ttu-id="31148-215">Vergewissern Sie sich, dass das Schreiben der Datei erfolgreich war und dass sie jetzt zwei Zeilen enthält.</span><span class="sxs-lookup"><span data-stu-id="31148-215">Confirm the file was successfully written and it now contains 2 lines.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="31148-216">Laden Sie die Datei in Blobspeicher hoch.</span><span class="sxs-lookup"><span data-stu-id="31148-216">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-web-application"></a><span data-ttu-id="31148-217">Testen der Webanwendung</span><span class="sxs-lookup"><span data-stu-id="31148-217">Test the web application</span></span>

<span data-ttu-id="31148-218">An diesem Punkt kann die Galerieanwendung ein Bild in den Blobspeicher hochladen, aber es können noch keine Bilder angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="31148-218">At this point, the gallery application is able to upload an image to Blob storage, but it can't display images yet.</span></span> <span data-ttu-id="31148-219">Die Anwendung versucht, die Funktion `GetImages` aufzurufen. Sie ist aber noch nicht vorhanden, da Sie sie erst in einem späteren Modul erstellen.</span><span class="sxs-lookup"><span data-stu-id="31148-219">It will try to call a `GetImages` function that doesn't exist yet because you create it in a later module.</span></span> <span data-ttu-id="31148-220">Der Aufruf ist nicht erfolgreich, und scheinbar hängt die Webseite mit einem Hinweis der Art „Wird analysiert...“, aber der Upload des gewählten Bilds ist trotzdem erfolgreich.</span><span class="sxs-lookup"><span data-stu-id="31148-220">That call will fail, and the web page will appear to be stuck on "Analyzing...", but the image you select will be successfully uploaded.</span></span>

<span data-ttu-id="31148-221">Sie können sich vergewissern, ob ein Bild erfolgreich hochgeladen wurde, indem Sie im Azure-Portal den Inhalt des Containers **images** überprüfen.</span><span class="sxs-lookup"><span data-stu-id="31148-221">You can verify an image is successfully uploaded by checking the contents of the **images** container in the Azure portal.</span></span>

1. <span data-ttu-id="31148-222">Navigieren Sie in einem Browserfenster zur Anwendung.</span><span class="sxs-lookup"><span data-stu-id="31148-222">In a browser window, browse to the application.</span></span> <span data-ttu-id="31148-223">Wählen Sie eine Bilddatei aus, und laden Sie sie hoch.</span><span class="sxs-lookup"><span data-stu-id="31148-223">Select an image file and upload it.</span></span> <span data-ttu-id="31148-224">Der Upload wird abgeschlossen. Da wir aber noch nicht die Möglichkeit zum Anzeigen von Bildern hinzugefügt haben, wird das hochgeladene Bild in der App nicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="31148-224">The upload completes, but because we haven't added the ability to display images yet, the app doesn't show the uploaded photo.</span></span> <span data-ttu-id="31148-225">(Die Webseite scheint beim Vorgang „Bild wird analysiert...“ zu hängen. Dies beheben Sie später.)</span><span class="sxs-lookup"><span data-stu-id="31148-225">(The web page appears to be stuck on "Analyzing image..."; you'll fix that later.)</span></span>

1. <span data-ttu-id="31148-226">Vergewissern Sie sich in der Cloud Shell, dass das Bild in den Container **images** hochgeladen wurde.</span><span class="sxs-lookup"><span data-stu-id="31148-226">In the Cloud Shell, confirm the image was uploaded to the **images** container.</span></span>

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. <span data-ttu-id="31148-227">Löschen Sie alle Dateien im Container **images**, bevor Sie mit dem nächsten Tutorial fortfahren.</span><span class="sxs-lookup"><span data-stu-id="31148-227">Before moving on to the next tutorial, delete all files in the **images** container.</span></span>

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```


## <a name="summary"></a><span data-ttu-id="31148-228">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="31148-228">Summary</span></span>

<span data-ttu-id="31148-229">In diesem Modul haben Sie eine Funktions-App erstellt und erfahren, wie Sie eine serverlose Funktion verwenden, um für eine Webanwendung das Hochladen von Bildern in Blobspeicher zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="31148-229">In this module, you created an Azure Function app and learned how to use a serverless function to allow a web application to upload images to Blob storage.</span></span> <span data-ttu-id="31148-230">Als Nächstes wird beschrieben, wie Sie Miniaturansichten für die hochgeladenen Bilder erstellen, indem Sie eine per Blob ausgelöste serverlose Funktion verwenden.</span><span class="sxs-lookup"><span data-stu-id="31148-230">Next, you learn how to create thumbnails for the uploaded images using a Blob triggered serverless function.</span></span>

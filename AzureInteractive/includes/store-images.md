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
ms.openlocfilehash: 2202cdebe77668972372983a0e802d00edabf6dd
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079463"
---
<span data-ttu-id="f6bdf-103">Azure Cosmos DB ist der serverlose, global verteilte Microsoft-Datenbankdienst mit mehreren Modellen.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-103">Azure Cosmos DB is Microsoft's serverless, globally distributed, multi-model database.</span></span> <span data-ttu-id="f6bdf-104">In diesem Modul erfahren Sie, wie Sie Azure Functions zum Speichern und Abrufen von Bildmetadaten als JSON-Dokumente in Cosmos DB verwenden.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-104">In this module, you learn how to use Azure Functions to store and retrieve image metadata as JSON documents in Cosmos DB.</span></span>

## <a name="create-a-cosmos-db-account-database-and-collection"></a><span data-ttu-id="f6bdf-105">Erstellen eines Kontos, einer Datenbank und einer Sammlung für Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="f6bdf-105">Create a Cosmos DB account, database, and collection</span></span>

<span data-ttu-id="f6bdf-106">Ein Cosmos DB-Konto ist eine Azure-Ressource, die Cosmos DB-Datenbanken enthält.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-106">A Cosmos DB account is an Azure resource that contains Cosmos DB databases.</span></span>

1. <span data-ttu-id="f6bdf-107">Stellen Sie sicher, dass Sie noch an der Cloud Shell angemeldet sind.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-107">Ensure you are still signed into the Cloud Shell.</span></span>  <span data-ttu-id="f6bdf-108">Wählen Sie andernfalls die Option **Enter focus mode** (Fokusmodus aktivieren), um ein Cloud Shell-Fenster zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-108">If not, select **Enter focus mode** to open a Cloud Shell window.</span></span> 

1. <span data-ttu-id="f6bdf-109">Erstellen Sie ein Cosmos DB-Konto mit einem eindeutigen Namen in derselben Ressourcengruppe, in der Sie auch die anderen Ressourcen dieses Tutorials erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-109">Create a Cosmos DB account with a unique name in the same resource group as the other resources in this tutorial.</span></span>

    ```azurecli
    az cosmosdb create -g first-serverless-app -n <cosmos db account name>
    ```

1. <span data-ttu-id="f6bdf-110">Erstellen Sie nach der Erstellung des Cosmos DB-Kontos im Konto eine neue Datenbank mit dem Namen **imagesdb**.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-110">After the Cosmos DB account is created, create a new database named **imagesdb** in the account.</span></span>

    ```azurecli
    az cosmosdb database create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb
    ```

1. <span data-ttu-id="f6bdf-111">Erstellen Sie nach der Erstellung der Datenbank darin eine neue Sammlung mit dem Namen **images**, die über einen Durchsatz von 400 Anforderungseinheiten (Request Units, RUs) verfügt.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-111">After the database is created, create a new collection named **images** in the database with a throughput of 400 request units (RUs).</span></span>

    ```azurecli
    az cosmosdb collection create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb --collection-name images --throughput 400
    ```


## <a name="save-a-document-to-cosmos-db-when-a-thumbnail-is-created"></a><span data-ttu-id="f6bdf-112">Speichern eines Dokuments in Cosmos DB bei der Erstellung einer Miniaturansicht</span><span class="sxs-lookup"><span data-stu-id="f6bdf-112">Save a document to Cosmos DB when a thumbnail is created</span></span>

<span data-ttu-id="f6bdf-113">Mit der Cosmos DB-Ausgabebindung können Sie Dokumente in einer Cosmos DB-Sammlung über Azure Functions erstellen.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-113">The Cosmos DB output binding lets you create documents in a Cosmos DB collection from Azure Functions.</span></span> <span data-ttu-id="f6bdf-114">In den folgenden Schritten konfigurieren Sie eine Cosmos DB-Ausgabebindung in der Funktion **ResizeImage** und ändern die Funktion, um ein zu speicherndes Dokument (Objekt) zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-114">In the following steps, you configure a Cosmos DB output binding in the **ResizeImage** function and modify the function to return a document (object) to be saved.</span></span>

1. <span data-ttu-id="f6bdf-115">Öffnen Sie die Funktions-App im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-115">Open the function app in the Azure Portal.</span></span>

1. <span data-ttu-id="f6bdf-116">Erweitern Sie im linken Navigationsbereich die Funktion **ResizeImage**, und wählen Sie dann **Integrieren**.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-116">In the left hand navigation, expand the **ResizeImage** function, and then select **Integrate**.</span></span>

1. <span data-ttu-id="f6bdf-117">Klicken Sie unter **Ausgaben** auf **Neue Ausgabe**.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-117">Under **Outputs**, click **New Output**.</span></span>

1. <span data-ttu-id="f6bdf-118">Suchen Sie nach dem Element **Azure Cosmos DB**, und wählen Sie es aus.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-118">Find the **Azure Cosmos DB** item and select it.</span></span> <span data-ttu-id="f6bdf-119">Klicken Sie dann auf **Auswählen**.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-119">Then click **Select**.</span></span>

    ![Auswählen von „Neue Ausgabe“](media/functions-first-serverless-web-app/4-new-output.jpg)

1. <span data-ttu-id="f6bdf-121">Füllen Sie die Felder der **Azure Cosmos DB-Ausgabe** mit den unten angegebenen Werten.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-121">Fill out the fields under **Azure Cosmos DB output** with the following values.</span></span>

    | <span data-ttu-id="f6bdf-122">Einstellung</span><span class="sxs-lookup"><span data-stu-id="f6bdf-122">Setting</span></span>      |  <span data-ttu-id="f6bdf-123">Empfohlener Wert</span><span class="sxs-lookup"><span data-stu-id="f6bdf-123">Suggested value</span></span>   | <span data-ttu-id="f6bdf-124">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="f6bdf-124">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="f6bdf-125">**Dokumentparametername**</span><span class="sxs-lookup"><span data-stu-id="f6bdf-125">**Document parameter name**</span></span> | <span data-ttu-id="f6bdf-126">Wählen Sie **Funktionsrückgabewert verwenden**.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-126">Select **Use function return value**</span></span> | <span data-ttu-id="f6bdf-127">Der Wert des Textfelds wird automatisch auf **$return** festgelegt.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-127">The value of the textbox is automatically set to **$return**.</span></span> |
    | <span data-ttu-id="f6bdf-128">**Datenbankname**</span><span class="sxs-lookup"><span data-stu-id="f6bdf-128">**Database name**</span></span> | <span data-ttu-id="f6bdf-129">imagesdb</span><span class="sxs-lookup"><span data-stu-id="f6bdf-129">imagesdb</span></span> | <span data-ttu-id="f6bdf-130">Verwenden Sie den Namen der Datenbank, die Sie erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-130">Use the name of the database that you created.</span></span> |
    | <span data-ttu-id="f6bdf-131">**Sammlungsname**</span><span class="sxs-lookup"><span data-stu-id="f6bdf-131">**Collection name**</span></span> | <span data-ttu-id="f6bdf-132">images</span><span class="sxs-lookup"><span data-stu-id="f6bdf-132">images</span></span> | <span data-ttu-id="f6bdf-133">Verwenden Sie den Namen der Sammlung, die Sie erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-133">Use the name of the collection that you created.</span></span> |

1. <span data-ttu-id="f6bdf-134">Klicken Sie neben **Azure Cosmos DB-Kontoverbindung** auf **Neu**.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-134">Next to **Azure Cosmos DB account connection**, click **new**.</span></span> <span data-ttu-id="f6bdf-135">Wählen Sie das Cosmos DB-Konto aus, das Sie zuvor erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-135">Select the Cosmos DB account you previously created.</span></span>

    ![Eingeben der Einstellungen für Azure Cosmos DB-Ausgabebindung](media/functions-first-serverless-web-app/4-cosmos-db-output.png)

1. <span data-ttu-id="f6bdf-137">Klicken Sie auf **Speichern**, um die Cosmos DB-Ausgabebindung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-137">Click **Save** to create the Cosmos DB output binding.</span></span>

1. <span data-ttu-id="f6bdf-138">Klicken Sie links auf den Funktionsnamen **ResizeImage**, um die Funktion zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-138">Click on the **ResizeImage** function name on the left to open the function.</span></span>

1. <span data-ttu-id="f6bdf-139">**C#**</span><span class="sxs-lookup"><span data-stu-id="f6bdf-139">**C#**</span></span>

    1. <span data-ttu-id="f6bdf-140">(C#) Ändern Sie den Rückgabetyp der Funktion von **void** in **object**.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-140">(C#) Change the return type of the function from **void** to **object**.</span></span>

    1. <span data-ttu-id="f6bdf-141">(C#) Fügen Sie am Ende der Funktion den folgenden Codeblock hinzu, um das zu speichernde Dokument zurückzugeben:</span><span class="sxs-lookup"><span data-stu-id="f6bdf-141">(C#) At the end of the function, add the following code block to return the document to be saved:</span></span>
    
        ```csharp
        return new {
            id = name,
            imgPath = "/images/" + name,
            thumbnailPath = "/thumbnails/" + name
        };
        ```
    
        ![„run.csx“ für Funktion „ResizeImages“ nach den Änderungen](media/functions-first-serverless-web-app/4-update-function.png)

1. <span data-ttu-id="f6bdf-143">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="f6bdf-143">**JavaScript**</span></span>

    1. <span data-ttu-id="f6bdf-144">(JavaScript) Ändern Sie die `context.done()`-Anweisung in der `else`-Klausel, um das in Cosmos DB zu speichernde Dokument zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-144">(JavaScript) Change the `context.done()` statement in the `else` clause to return the document to be saved to Cosmos DB.</span></span>

    ```javascript
    if (error) {
        context.done(error);
    } else {
        context.bindings.thumbnail = stream;
        context.done(null, {
            id: context.bindingData.name,
            imgPath: "/images/" + context.bindingData.name,
            thumbnailPath: "/thumbnails/" + context.bindingData.name
        });
    }
    ```

1. <span data-ttu-id="f6bdf-145">Klicken Sie unter dem Codefenster auf **Protokolle**, um den Protokollbereich zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-145">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="f6bdf-146">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-146">Click **Save**.</span></span> <span data-ttu-id="f6bdf-147">Überprüfen Sie den Protokollbereich, um sicherzustellen, dass die Funktion erfolgreich gespeichert wird und keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-147">Check the logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="create-a-function-to-list-images-from-cosmos-db"></a><span data-ttu-id="f6bdf-148">Erstellen einer Funktion zum Auflisten von Bildern aus Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="f6bdf-148">Create a function to list images from Cosmos DB</span></span>

<span data-ttu-id="f6bdf-149">Für die Webanwendung ist eine API zum Abrufen der Bildmetadaten aus Cosmos DB erforderlich.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-149">The web application requires an API to retrieve image metadata from Cosmos DB.</span></span> <span data-ttu-id="f6bdf-150">Mit den folgenden Schritten</span><span class="sxs-lookup"><span data-stu-id="f6bdf-150">In the following steps.</span></span> <span data-ttu-id="f6bdf-151">erstellen Sie eine per HTTP ausgelöste Funktion, bei der eine Cosmos DB-Eingabebindung erstellt wird, um die Datenbanksammlung abzufragen.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-151">uou create an HTTP triggered function that uses a Cosmos DB input binding to query the database collection.</span></span>

1. <span data-ttu-id="f6bdf-152">Bewegen Sie den Mauszeiger in Ihrer Funktions-App auf der linken Seite auf **Funktionen**, und klicken Sie auf **+**, um eine neue Funktion zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-152">In your function app, hover over **Functions** on the left and click **+** to create a new function.</span></span>

1. <span data-ttu-id="f6bdf-153">Suchen Sie nach der Vorlage **HttpTrigger**, und wählen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-153">Find the **HttpTrigger** template and select it.</span></span>

1. <span data-ttu-id="f6bdf-154">Verwenden Sie diese Werte, um eine Funktion zu erstellen, mit der eine URL zum Abrufen von Bildern generiert wird.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-154">Use these values to create a function that generates a get images URL.</span></span>

    | <span data-ttu-id="f6bdf-155">Einstellung</span><span class="sxs-lookup"><span data-stu-id="f6bdf-155">Setting</span></span>      |  <span data-ttu-id="f6bdf-156">Empfohlener Wert</span><span class="sxs-lookup"><span data-stu-id="f6bdf-156">Suggested value</span></span>   | <span data-ttu-id="f6bdf-157">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="f6bdf-157">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="f6bdf-158">**Name Ihrer Funktion**</span><span class="sxs-lookup"><span data-stu-id="f6bdf-158">**Name your function**</span></span> | <span data-ttu-id="f6bdf-159">GetImages</span><span class="sxs-lookup"><span data-stu-id="f6bdf-159">GetImages</span></span> | <span data-ttu-id="f6bdf-160">Geben Sie diesen Namen genau wie hier angezeigt ein, damit die Anwendung die Funktion ermitteln kann.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-160">Type this name exactly as shown so the application can discover the function.</span></span> |
    | <span data-ttu-id="f6bdf-161">**Autorisierungsstufe**</span><span class="sxs-lookup"><span data-stu-id="f6bdf-161">**Authorization level**</span></span> | <span data-ttu-id="f6bdf-162">Anonym</span><span class="sxs-lookup"><span data-stu-id="f6bdf-162">Anonymous</span></span> | <span data-ttu-id="f6bdf-163">Lässt den schnellen Zugriff auf die Funktion zu.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-163">Allow the function to be accessed publicly.</span></span> |

1. <span data-ttu-id="f6bdf-164">Klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-164">Click **Create**.</span></span>

1. <span data-ttu-id="f6bdf-165">Klicken Sie nach der Erstellung der neuen Funktion im linken Navigationsbereich unter dem Namen der Funktion auf **Integrieren**.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-165">When the new function is created, click **Integrate** under the function's name on the left navigation.</span></span>

1. <span data-ttu-id="f6bdf-166">Klicken Sie auf **Neue Eingabe**, und wählen Sie **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-166">Click **New Input** and select **Azure Cosmos DB**.</span></span> 

    ![Auswählen von „Neue Eingabe“](media/functions-first-serverless-web-app/4-new-input.jpg)

1. <span data-ttu-id="f6bdf-168">Klicken Sie auf **Auswählen**.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-168">Click **Select**.</span></span>

1. <span data-ttu-id="f6bdf-169">Geben Sie die folgenden Werte an:</span><span class="sxs-lookup"><span data-stu-id="f6bdf-169">Fill out the following values:</span></span>

    | <span data-ttu-id="f6bdf-170">Einstellung</span><span class="sxs-lookup"><span data-stu-id="f6bdf-170">Setting</span></span>      |  <span data-ttu-id="f6bdf-171">Empfohlener Wert</span><span class="sxs-lookup"><span data-stu-id="f6bdf-171">Suggested value</span></span>   | <span data-ttu-id="f6bdf-172">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="f6bdf-172">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="f6bdf-173">**Dokumentparametername**</span><span class="sxs-lookup"><span data-stu-id="f6bdf-173">**Document parameter name**</span></span> | <span data-ttu-id="f6bdf-174">documents</span><span class="sxs-lookup"><span data-stu-id="f6bdf-174">documents</span></span> | <span data-ttu-id="f6bdf-175">Entspricht dem Parameternamen in der Funktion.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-175">Matches parameter name in the function.</span></span> |
    | <span data-ttu-id="f6bdf-176">**Datenbankname**</span><span class="sxs-lookup"><span data-stu-id="f6bdf-176">**Database name**</span></span> | <span data-ttu-id="f6bdf-177">imagesdb</span><span class="sxs-lookup"><span data-stu-id="f6bdf-177">imagesdb</span></span> |  |
    | <span data-ttu-id="f6bdf-178">**Sammlungsname**</span><span class="sxs-lookup"><span data-stu-id="f6bdf-178">**Collection name**</span></span> | <span data-ttu-id="f6bdf-179">images</span><span class="sxs-lookup"><span data-stu-id="f6bdf-179">images</span></span> |  |
    | <span data-ttu-id="f6bdf-180">**SQL query**</span><span class="sxs-lookup"><span data-stu-id="f6bdf-180">**SQL query**</span></span> | <span data-ttu-id="f6bdf-181">select \* from c order by c._ts desc</span><span class="sxs-lookup"><span data-stu-id="f6bdf-181">select \* from c order by c._ts desc</span></span> | <span data-ttu-id="f6bdf-182">Dient zum Abrufen von Dokumenten (letzte Dokumente zuerst).</span><span class="sxs-lookup"><span data-stu-id="f6bdf-182">Get documents, latest documents first.</span></span> |
    | <span data-ttu-id="f6bdf-183">**Azure Cosmos DB-Kontoverbindung**</span><span class="sxs-lookup"><span data-stu-id="f6bdf-183">**Azure Cosmos DB account connection**</span></span> | <span data-ttu-id="f6bdf-184">Auswählen der vorhandenen Verbindungszeichenfolge</span><span class="sxs-lookup"><span data-stu-id="f6bdf-184">Select existing connection string</span></span> |  |

1. <span data-ttu-id="f6bdf-185">Klicken Sie auf **Speichern**, um die Eingabebindung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-185">Click **Save** to create the input binding.</span></span>

1. <span data-ttu-id="f6bdf-186">**C#**</span><span class="sxs-lookup"><span data-stu-id="f6bdf-186">**C#**</span></span>

    1. <span data-ttu-id="f6bdf-187">Klicken Sie auf den Namen der Funktion, um das Codefenster zu öffnen, und ersetzen Sie dann den gesamten Inhalt von **run.csx** durch den Inhalt von [**/csharp/GetImages/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetImages/run.csx).</span><span class="sxs-lookup"><span data-stu-id="f6bdf-187">Click the function's name to open the code window, and then replace all of **run.csx** with the content in [**/csharp/GetImages/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetImages/run.csx).</span></span>

1. <span data-ttu-id="f6bdf-188">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="f6bdf-188">**JavaScript**</span></span>

    1. <span data-ttu-id="f6bdf-189">Klicken Sie auf den Namen der Funktion, um das Codefenster zu öffnen, und ersetzen Sie dann den gesamten Inhalt von **index.js** durch den Inhalt von [**/javascript/GetImages/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetImages/index.js).</span><span class="sxs-lookup"><span data-stu-id="f6bdf-189">Click the function's name to open the code window, and then replace all of **index.js** with the content in [**/javascript/GetImages/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetImages/index.js).</span></span>

1. <span data-ttu-id="f6bdf-190">Klicken Sie unter dem Codefenster auf **Protokolle**, um den Protokollbereich zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-190">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="f6bdf-191">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-191">Click **Save**.</span></span> <span data-ttu-id="f6bdf-192">Überprüfen Sie den Protokollbereich, um sicherzustellen, dass die Funktion erfolgreich gespeichert wird und keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-192">Check the logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="test-the-application"></a><span data-ttu-id="f6bdf-193">Testen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="f6bdf-193">Test the application</span></span>

1. <span data-ttu-id="f6bdf-194">Öffnen Sie die Anwendung in einem Browser.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-194">Open the application in a browser.</span></span> <span data-ttu-id="f6bdf-195">Wählen Sie eine Bilddatei aus, und laden Sie sie hoch.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-195">Select an image file and upload it.</span></span>

1. <span data-ttu-id="f6bdf-196">Nach einigen Sekunden wird die Miniaturansicht des neuen Bilds auf der Seite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-196">After a few seconds, the thumbnail of the new image appears on the page.</span></span>

1. <span data-ttu-id="f6bdf-197">Verwenden Sie im Azure-Portal das Suchfeld, um anhand des Namens nach Ihrem Cosmos DB-Konto zu suchen.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-197">In the Azure portal, use the Search box to search for your Cosmos DB account by name.</span></span> <span data-ttu-id="f6bdf-198">Klicken Sie darauf, um es zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-198">Click it to open it.</span></span>

1. <span data-ttu-id="f6bdf-199">Klicken Sie links auf **Daten-Explorer**, um Sammlungen und Dokumente zu durchsuchen.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-199">Click **Data Explorer** on the left to browse collections and documents.</span></span>

1. <span data-ttu-id="f6bdf-200">Wählen Sie unter der Datenbank **imagesdb** die Sammlung **images** aus.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-200">Under the **imagesdb** database, select the **images** collection.</span></span>

1. <span data-ttu-id="f6bdf-201">Vergewissern Sie sich, dass ein Dokument für das hochgeladene Bild erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-201">Confirm that a document was created for the uploaded image.</span></span>

    ![Anzeige eines Dokuments für ein hochgeladenes Bild im Daten-Explorer](media/functions-first-serverless-web-app/4-data-explorer.png)



## <a name="summary"></a><span data-ttu-id="f6bdf-203">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="f6bdf-203">Summary</span></span>

<span data-ttu-id="f6bdf-204">In diesem Modul haben Sie erfahren, wie Sie ein Konto, eine Datenbank und eine Sammlung für Cosmos DB erstellen.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-204">In this module, you learned how to create a Cosmos DB account, database, and collection.</span></span> <span data-ttu-id="f6bdf-205">Außerdem wurde beschrieben, wie Sie die Cosmos DB-Bindungen zum Speichern und Abrufen von Bildmetadaten in der Cosmos DB-Sammlung verwenden.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-205">You also learned how to use the Cosmos DB bindings to save and retrieve image metadata in the Cosmos DB collection.</span></span> <span data-ttu-id="f6bdf-206">Als Nächstes wird beschrieben, wie Sie für jedes hochgeladene Bild mit Microsoft Cognitive Services automatisch einen Bildtitel generieren können.</span><span class="sxs-lookup"><span data-stu-id="f6bdf-206">Next, you learn how to automatically generate a caption for each uploaded image using Microsoft Cognitive Services.</span></span>
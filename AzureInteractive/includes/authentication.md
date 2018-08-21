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
ms.openlocfilehash: d1f9a07ce3d3b096b498e48b5c4f68c3454b2b37
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079487"
---
<span data-ttu-id="d9b4d-103">Die Azure App Service-Authentifizierung ermöglicht in einer Azure-Funktions-App die Unterstützung der Authentifizierung ohne weiteren Einrichtungsaufwand.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-103">Azure App Service Authentication enables turn-key authentication support in an Azure Function app.</span></span> <span data-ttu-id="d9b4d-104">Sie kann nahtlos in Facebook, Twitter, ein Microsoft-Konto, Google und Azure Active Directory integriert werden.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-104">It integrates seamlessly with Facebook, Twitter, Microsoft Account, Google, and Azure Active Directory.</span></span> <span data-ttu-id="d9b4d-105">Sie fügen die App Service-Authentifizierung hinzu, um die Back-End-APIs für Ihre Web-App zu schützen.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-105">You'll add App Service Authentication to protect the backend APIs of your web app.</span></span>

## <a name="enable-app-service-authentication"></a><span data-ttu-id="d9b4d-106">Aktivieren der App Service-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="d9b4d-106">Enable App Service Authentication</span></span>

1. <span data-ttu-id="d9b4d-107">Öffnen Sie die Funktions-App im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-107">Open the function app in the Azure Portal.</span></span>

1. <span data-ttu-id="d9b4d-108">Wählen Sie unter **Plattformfeatures** die Option **Authentifizierung/Autorisierung**.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-108">Under **Platform features**, select **Authentication/Authorization**.</span></span>

    ![Auswählen von „Authentifizierung/Autorisierung“](media/functions-first-serverless-web-app/6-authorization.jpg)

1. <span data-ttu-id="d9b4d-110">Wählen Sie folgende Werte aus:</span><span class="sxs-lookup"><span data-stu-id="d9b4d-110">Select the following values:</span></span>
    
    | <span data-ttu-id="d9b4d-111">Einstellung</span><span class="sxs-lookup"><span data-stu-id="d9b4d-111">Setting</span></span>      |  <span data-ttu-id="d9b4d-112">Empfohlener Wert</span><span class="sxs-lookup"><span data-stu-id="d9b4d-112">Suggested value</span></span>   | <span data-ttu-id="d9b4d-113">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="d9b4d-113">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="d9b4d-114">**App Service-Authentifizierung**</span><span class="sxs-lookup"><span data-stu-id="d9b4d-114">**App Service Authentication**</span></span> | <span data-ttu-id="d9b4d-115">Ein</span><span class="sxs-lookup"><span data-stu-id="d9b4d-115">On</span></span> | <span data-ttu-id="d9b4d-116">Aktiviert die Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-116">Enable authentication.</span></span> |
    | <span data-ttu-id="d9b4d-117">**Aktion, wenn die Anforderung nicht authentifiziert ist**</span><span class="sxs-lookup"><span data-stu-id="d9b4d-117">**Action when request is not authenticated**</span></span> | <span data-ttu-id="d9b4d-118">Anmelden mit Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d9b4d-118">Log in with Azure Active Directory</span></span> | <span data-ttu-id="d9b4d-119">Wählen Sie eine konfigurierte Authentifizierungsmethode aus (siehe unten).</span><span class="sxs-lookup"><span data-stu-id="d9b4d-119">Select a configured authentication method (below).</span></span> |
    | <span data-ttu-id="d9b4d-120">**Authentifizierungsanbieter**</span><span class="sxs-lookup"><span data-stu-id="d9b4d-120">**Authentication Providers**</span></span> | <span data-ttu-id="d9b4d-121">Siehe unten</span><span class="sxs-lookup"><span data-stu-id="d9b4d-121">See below</span></span> | <span data-ttu-id="d9b4d-122">Siehe unten</span><span class="sxs-lookup"><span data-stu-id="d9b4d-122">See below</span></span> |
    | <span data-ttu-id="d9b4d-123">**Tokenspeicher**</span><span class="sxs-lookup"><span data-stu-id="d9b4d-123">**Token store**</span></span> | <span data-ttu-id="d9b4d-124">Ein</span><span class="sxs-lookup"><span data-stu-id="d9b4d-124">On</span></span> | <span data-ttu-id="d9b4d-125">Ermöglicht App Service das Speichern und Verwalten von Token.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-125">Allow App Service to store and manage tokens.</span></span> |
    | <span data-ttu-id="d9b4d-126">**Zulässige externe Umleitungs-URLs**</span><span class="sxs-lookup"><span data-stu-id="d9b4d-126">**Allowed external redirect URLs**</span></span> | <span data-ttu-id="d9b4d-127">Die URL Ihrer Anwendung, z.B.: https://firstserverlessweb.z4.web.core.windows.net/</span><span class="sxs-lookup"><span data-stu-id="d9b4d-127">The URL of your application, for example: https://firstserverlessweb.z4.web.core.windows.net/</span></span> | <span data-ttu-id="d9b4d-128">URL(s), an die App Service umleiten darf, nachdem ein Benutzer authentifiziert wurde.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-128">URL(s) that App Service is allowed to redirect to after a user is authenticated.</span></span> |

1. <span data-ttu-id="d9b4d-129">Wählen Sie **Azure Active Directory**, um die **Azure Active Directory-Einstellungen** anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-129">Select **Azure Active Directory** to reveal **Azure Active Directory Settings**.</span></span>

    1. <span data-ttu-id="d9b4d-130">Wählen Sie **Express** als **Verwaltungsmodus** aus, und fügen Sie die unten angegebenen Informationen ein.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-130">Select **Express** as the **Management Mode** and fill in the following information.</span></span>
    
        | <span data-ttu-id="d9b4d-131">Einstellung</span><span class="sxs-lookup"><span data-stu-id="d9b4d-131">Setting</span></span>      |  <span data-ttu-id="d9b4d-132">Empfohlener Wert</span><span class="sxs-lookup"><span data-stu-id="d9b4d-132">Suggested value</span></span>   | <span data-ttu-id="d9b4d-133">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="d9b4d-133">Description</span></span>                                        |
        | --- | --- | ---|
        | <span data-ttu-id="d9b4d-134">**Verwaltungsmodus**</span><span class="sxs-lookup"><span data-stu-id="d9b4d-134">**Management mode**</span></span> | <span data-ttu-id="d9b4d-135">Express: Erstellen einer neuen AD-App</span><span class="sxs-lookup"><span data-stu-id="d9b4d-135">Express, Create new AD app</span></span> | <span data-ttu-id="d9b4d-136">Es werden automatisch ein Dienstprinzipal und die Azure Active Directory-Authentifizierung eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-136">Automatically set up a service principal and Azure Active Directory authentication.</span></span> |
        | <span data-ttu-id="d9b4d-137">**Erstellen einer App**</span><span class="sxs-lookup"><span data-stu-id="d9b4d-137">**Create app**</span></span> | <span data-ttu-id="d9b4d-138">my-serverless-webapp</span><span class="sxs-lookup"><span data-stu-id="d9b4d-138">my-serverless-webapp</span></span> | <span data-ttu-id="d9b4d-139">Geben Sie einen eindeutigen Anwendungsnamen ein.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-139">Enter a unique application name.</span></span> |
    
    1. <span data-ttu-id="d9b4d-140">Klicken Sie auf **OK**, um die Azure Active Directory-Einstellungen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-140">Click **OK** to save the Azure Active Directory settings.</span></span>

    ![„Authentifizierung/Autorisierung“ und „Azure Active Directory-Einstellungen“](media/functions-first-serverless-web-app/6-create-aad.png)

1. <span data-ttu-id="d9b4d-142">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-142">Click **Save**.</span></span>


## <a name="modify-the-web-app-to-enable-authentication"></a><span data-ttu-id="d9b4d-143">Ändern der Web-App zum Aktivieren der Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="d9b4d-143">Modify the web app to enable authentication</span></span>

1. <span data-ttu-id="d9b4d-144">Stellen Sie in der Cloud Shell sicher, dass das aktuelle Verzeichnis der Ordner **www/dist** ist.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-144">In the Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="d9b4d-145">Fügen Sie die folgende Codezeile an **settings.js** an, um die Authentifizierung in Ihrer Funktions-App zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-145">To enable authentication in your function app, append the following line of code to **settings.js**.</span></span>

    `window.authEnabled = true`

    <span data-ttu-id="d9b4d-146">Nehmen Sie die Änderung vor, und zeigen Sie das Ergebnis an, indem Sie die folgenden Befehle oder einen Befehlszeilen-Editor wie VIM verwenden.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-146">Make the change and view the result by using the following commands or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.authEnabled = true" >> settings.js
    ```

    <span data-ttu-id="d9b4d-147">Vergewissern Sie sich, dass die Änderung an der Datei vorgenommen wurde.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-147">Confirm the change was made to the file.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="d9b4d-148">Laden Sie die Datei in Blobspeicher hoch.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-148">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-application"></a><span data-ttu-id="d9b4d-149">Testen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="d9b4d-149">Test the application</span></span>

1. <span data-ttu-id="d9b4d-150">Öffnen Sie die Anwendung in einem Browser.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-150">Open the application in a browser.</span></span> <span data-ttu-id="d9b4d-151">Klicken Sie auf **Anmelden**, und melden Sie sich an.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-151">Click **Log in** and log in.</span></span>

1. <span data-ttu-id="d9b4d-152">Wählen Sie eine Bilddatei aus, und laden Sie sie hoch.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-152">Select an image file and upload it.</span></span>

    ![Anmeldeseite](media/functions-first-serverless-web-app/6-aad-auth.png)
    

## <a name="summary"></a><span data-ttu-id="d9b4d-154">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="d9b4d-154">Summary</span></span>

<span data-ttu-id="d9b4d-155">In diesem Modul wurde beschrieben, wie Sie der Anwendung die Authentifizierung hinzufügen, indem Sie die Azure App Service-Authentifizierung verwenden.</span><span class="sxs-lookup"><span data-stu-id="d9b4d-155">In this module, you learned how to add authentication to the applcation using Azure App Service Authentication.</span></span>

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
ms.openlocfilehash: 426a7287458a48d1bda220ad1a5f067be2ce77d6
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460040"
---
<span data-ttu-id="f3c9b-103">Die Azure App Service-Authentifizierung ermöglicht in einer Azure-Funktions-App die Unterstützung der Authentifizierung ohne weiteren Einrichtungsaufwand.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-103">Azure App Service Authentication enables turn-key authentication support in an Azure Function app.</span></span> <span data-ttu-id="f3c9b-104">Sie kann nahtlos in Facebook, Twitter, ein Microsoft-Konto, Google und Azure Active Directory integriert werden.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-104">It integrates seamlessly with Facebook, Twitter, Microsoft Account, Google, and Azure Active Directory.</span></span> <span data-ttu-id="f3c9b-105">Sie fügen die App Service-Authentifizierung hinzu, um die Back-End-APIs für Ihre Web-App zu schützen.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-105">You'll add App Service Authentication to protect the backend APIs of your web app.</span></span>

## <a name="enable-app-service-authentication"></a><span data-ttu-id="f3c9b-106">Aktivieren der App Service-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="f3c9b-106">Enable App Service Authentication</span></span>

1. <span data-ttu-id="f3c9b-107">Öffnen Sie die Funktions-App im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-107">Open the function app in the Azure Portal.</span></span>

1. <span data-ttu-id="f3c9b-108">Wählen Sie unter **Plattformfeatures** die Option **Authentifizierung/Autorisierung**.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-108">Under **Platform features**, select **Authentication/Authorization**.</span></span>

    ![Auswählen von „Authentifizierung/Autorisierung“](media/functions-first-serverless-web-app/6-authorization.jpg)

1. <span data-ttu-id="f3c9b-110">Wählen Sie folgende Werte aus:</span><span class="sxs-lookup"><span data-stu-id="f3c9b-110">Select the following values:</span></span>
    
    | <span data-ttu-id="f3c9b-111">Einstellung</span><span class="sxs-lookup"><span data-stu-id="f3c9b-111">Setting</span></span>      |  <span data-ttu-id="f3c9b-112">Empfohlener Wert</span><span class="sxs-lookup"><span data-stu-id="f3c9b-112">Suggested value</span></span>   | <span data-ttu-id="f3c9b-113">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="f3c9b-113">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="f3c9b-114">**App Service-Authentifizierung**</span><span class="sxs-lookup"><span data-stu-id="f3c9b-114">**App Service Authentication**</span></span> | <span data-ttu-id="f3c9b-115">Andererseits</span><span class="sxs-lookup"><span data-stu-id="f3c9b-115">On</span></span> | <span data-ttu-id="f3c9b-116">Aktiviert die Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-116">Enable authentication.</span></span> |
    | <span data-ttu-id="f3c9b-117">**Aktion, wenn die Anforderung nicht authentifiziert ist**</span><span class="sxs-lookup"><span data-stu-id="f3c9b-117">**Action when request is not authenticated**</span></span> | <span data-ttu-id="f3c9b-118">Anmelden mit Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f3c9b-118">Log in with Azure Active Directory</span></span> | <span data-ttu-id="f3c9b-119">Wählen Sie eine konfigurierte Authentifizierungsmethode aus (siehe unten).</span><span class="sxs-lookup"><span data-stu-id="f3c9b-119">Select a configured authentication method (below).</span></span> |
    | <span data-ttu-id="f3c9b-120">**Authentifizierungsanbieter**</span><span class="sxs-lookup"><span data-stu-id="f3c9b-120">**Authentication Providers**</span></span> | <span data-ttu-id="f3c9b-121">Siehe unten</span><span class="sxs-lookup"><span data-stu-id="f3c9b-121">See below</span></span> | <span data-ttu-id="f3c9b-122">Siehe unten</span><span class="sxs-lookup"><span data-stu-id="f3c9b-122">See below</span></span> |
    | <span data-ttu-id="f3c9b-123">**Tokenspeicher**</span><span class="sxs-lookup"><span data-stu-id="f3c9b-123">**Token store**</span></span> | <span data-ttu-id="f3c9b-124">Andererseits</span><span class="sxs-lookup"><span data-stu-id="f3c9b-124">On</span></span> | <span data-ttu-id="f3c9b-125">Ermöglicht App Service das Speichern und Verwalten von Token.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-125">Allow App Service to store and manage tokens.</span></span> |
    | <span data-ttu-id="f3c9b-126">**Zulässige externe Umleitungs-URLs**</span><span class="sxs-lookup"><span data-stu-id="f3c9b-126">**Allowed external redirect URLs**</span></span> | <span data-ttu-id="f3c9b-127">Die URL Ihrer Anwendung, z.B.: https://firstserverlessweb.z4.web.core.windows.net/</span><span class="sxs-lookup"><span data-stu-id="f3c9b-127">The URL of your application, for example: https://firstserverlessweb.z4.web.core.windows.net/</span></span> | <span data-ttu-id="f3c9b-128">URL(s), an die App Service umleiten darf, nachdem ein Benutzer authentifiziert wurde.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-128">URL(s) that App Service is allowed to redirect to after a user is authenticated.</span></span> |

1. <span data-ttu-id="f3c9b-129">Wählen Sie **Azure Active Directory**, um die **Azure Active Directory-Einstellungen** anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-129">Select **Azure Active Directory** to reveal **Azure Active Directory Settings**.</span></span>

    1. <span data-ttu-id="f3c9b-130">Wählen Sie **Express** als **Verwaltungsmodus** aus, und fügen Sie die unten angegebenen Informationen ein.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-130">Select **Express** as the **Management Mode** and fill in the following information.</span></span>
    
        | <span data-ttu-id="f3c9b-131">Einstellung</span><span class="sxs-lookup"><span data-stu-id="f3c9b-131">Setting</span></span>      |  <span data-ttu-id="f3c9b-132">Empfohlener Wert</span><span class="sxs-lookup"><span data-stu-id="f3c9b-132">Suggested value</span></span>   | <span data-ttu-id="f3c9b-133">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="f3c9b-133">Description</span></span>                                        |
        | --- | --- | ---|
        | <span data-ttu-id="f3c9b-134">**Verwaltungsmodus**</span><span class="sxs-lookup"><span data-stu-id="f3c9b-134">**Management mode**</span></span> | <span data-ttu-id="f3c9b-135">Express: Erstellen einer neuen AD-App</span><span class="sxs-lookup"><span data-stu-id="f3c9b-135">Express, Create new AD app</span></span> | <span data-ttu-id="f3c9b-136">Es werden automatisch ein Dienstprinzipal und die Azure Active Directory-Authentifizierung eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-136">Automatically set up a service principal and Azure Active Directory authentication.</span></span> |
        | <span data-ttu-id="f3c9b-137">**Erstellen einer App**</span><span class="sxs-lookup"><span data-stu-id="f3c9b-137">**Create app**</span></span> | <span data-ttu-id="f3c9b-138">my-serverless-webapp</span><span class="sxs-lookup"><span data-stu-id="f3c9b-138">my-serverless-webapp</span></span> | <span data-ttu-id="f3c9b-139">Geben Sie einen eindeutigen Anwendungsnamen ein.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-139">Enter a unique application name.</span></span> |
    
    1. <span data-ttu-id="f3c9b-140">Klicken Sie auf **OK**, um die Azure Active Directory-Einstellungen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-140">Click **OK** to save the Azure Active Directory settings.</span></span>

    ![„Authentifizierung/Autorisierung“ und „Azure Active Directory-Einstellungen“](media/functions-first-serverless-web-app/6-create-aad.png)

1. <span data-ttu-id="f3c9b-142">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-142">Click **Save**.</span></span>


## <a name="modify-the-web-app-to-enable-authentication"></a><span data-ttu-id="f3c9b-143">Ändern der Web-App zum Aktivieren der Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="f3c9b-143">Modify the web app to enable authentication</span></span>

1. <span data-ttu-id="f3c9b-144">Stellen Sie in der Cloud Shell sicher, dass das aktuelle Verzeichnis der Ordner **www/dist** ist.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-144">In the Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="f3c9b-145">Fügen Sie die folgende Codezeile an **settings.js** an, um die Authentifizierung in Ihrer Funktions-App zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-145">To enable authentication in your function app, append the following line of code to **settings.js**.</span></span>

    `window.authEnabled = true`

    <span data-ttu-id="f3c9b-146">Nehmen Sie die Änderung vor, und zeigen Sie das Ergebnis an, indem Sie die folgenden Befehle oder einen Befehlszeilen-Editor wie VIM verwenden.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-146">Make the change and view the result by using the following commands or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.authEnabled = true" >> settings.js
    ```

    <span data-ttu-id="f3c9b-147">Vergewissern Sie sich, dass die Änderung an der Datei vorgenommen wurde.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-147">Confirm the change was made to the file.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="f3c9b-148">Laden Sie die Datei in Blobspeicher hoch.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-148">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-application"></a><span data-ttu-id="f3c9b-149">Testen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="f3c9b-149">Test the application</span></span>

1. <span data-ttu-id="f3c9b-150">Öffnen Sie die Anwendung in einem Browser.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-150">Open the application in a browser.</span></span> <span data-ttu-id="f3c9b-151">Klicken Sie auf **Anmelden**, und melden Sie sich an.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-151">Click **Log in** and log in.</span></span>

1. <span data-ttu-id="f3c9b-152">Wählen Sie eine Bilddatei aus, und laden Sie sie hoch.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-152">Select an image file and upload it.</span></span>

    ![Anmeldeseite](media/functions-first-serverless-web-app/6-aad-auth.png)
    

## <a name="summary"></a><span data-ttu-id="f3c9b-154">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="f3c9b-154">Summary</span></span>

<span data-ttu-id="f3c9b-155">In diesem Modul wurde beschrieben, wie Sie der Anwendung die Authentifizierung hinzufügen, indem Sie die Azure App Service-Authentifizierung verwenden.</span><span class="sxs-lookup"><span data-stu-id="f3c9b-155">In this module, you learned how to add authentication to the applcation using Azure App Service Authentication.</span></span>

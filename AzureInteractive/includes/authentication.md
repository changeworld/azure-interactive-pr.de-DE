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
Die Azure App Service-Authentifizierung ermöglicht in einer Azure-Funktions-App die Unterstützung der Authentifizierung ohne weiteren Einrichtungsaufwand. Sie kann nahtlos in Facebook, Twitter, ein Microsoft-Konto, Google und Azure Active Directory integriert werden. Sie fügen die App Service-Authentifizierung hinzu, um die Back-End-APIs für Ihre Web-App zu schützen.

## <a name="enable-app-service-authentication"></a>Aktivieren der App Service-Authentifizierung

1. Öffnen Sie die Funktions-App im Azure-Portal.

1. Wählen Sie unter **Plattformfeatures** die Option **Authentifizierung/Autorisierung**.

    ![Auswählen von „Authentifizierung/Autorisierung“](media/functions-first-serverless-web-app/6-authorization.jpg)

1. Wählen Sie folgende Werte aus:
    
    | Einstellung      |  Empfohlener Wert   | BESCHREIBUNG                                        |
    | --- | --- | ---|
    | **App Service-Authentifizierung** | Ein | Aktiviert die Authentifizierung. |
    | **Aktion, wenn die Anforderung nicht authentifiziert ist** | Anmelden mit Azure Active Directory | Wählen Sie eine konfigurierte Authentifizierungsmethode aus (siehe unten). |
    | **Authentifizierungsanbieter** | Siehe unten | Siehe unten |
    | **Tokenspeicher** | Ein | Ermöglicht App Service das Speichern und Verwalten von Token. |
    | **Zulässige externe Umleitungs-URLs** | Die URL Ihrer Anwendung, z.B.: https://firstserverlessweb.z4.web.core.windows.net/ | URL(s), an die App Service umleiten darf, nachdem ein Benutzer authentifiziert wurde. |

1. Wählen Sie **Azure Active Directory**, um die **Azure Active Directory-Einstellungen** anzuzeigen.

    1. Wählen Sie **Express** als **Verwaltungsmodus** aus, und fügen Sie die unten angegebenen Informationen ein.
    
        | Einstellung      |  Empfohlener Wert   | BESCHREIBUNG                                        |
        | --- | --- | ---|
        | **Verwaltungsmodus** | Express: Erstellen einer neuen AD-App | Es werden automatisch ein Dienstprinzipal und die Azure Active Directory-Authentifizierung eingerichtet. |
        | **Erstellen einer App** | my-serverless-webapp | Geben Sie einen eindeutigen Anwendungsnamen ein. |
    
    1. Klicken Sie auf **OK**, um die Azure Active Directory-Einstellungen zu speichern.

    ![„Authentifizierung/Autorisierung“ und „Azure Active Directory-Einstellungen“](media/functions-first-serverless-web-app/6-create-aad.png)

1. Klicken Sie auf **Speichern**.


## <a name="modify-the-web-app-to-enable-authentication"></a>Ändern der Web-App zum Aktivieren der Authentifizierung

1. Stellen Sie in der Cloud Shell sicher, dass das aktuelle Verzeichnis der Ordner **www/dist** ist.

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. Fügen Sie die folgende Codezeile an **settings.js** an, um die Authentifizierung in Ihrer Funktions-App zu aktivieren.

    `window.authEnabled = true`

    Nehmen Sie die Änderung vor, und zeigen Sie das Ergebnis an, indem Sie die folgenden Befehle oder einen Befehlszeilen-Editor wie VIM verwenden.

    ```azurecli
    echo "window.authEnabled = true" >> settings.js
    ```

    Vergewissern Sie sich, dass die Änderung an der Datei vorgenommen wurde.

    ```azurecli
    cat settings.js
    ```

1. Laden Sie die Datei in Blobspeicher hoch.

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-application"></a>Testen der Anwendung

1. Öffnen Sie die Anwendung in einem Browser. Klicken Sie auf **Anmelden**, und melden Sie sich an.

1. Wählen Sie eine Bilddatei aus, und laden Sie sie hoch.

    ![Anmeldeseite](media/functions-first-serverless-web-app/6-aad-auth.png)
    

## <a name="summary"></a>Zusammenfassung

In diesem Modul wurde beschrieben, wie Sie der Anwendung die Authentifizierung hinzufügen, indem Sie die Azure App Service-Authentifizierung verwenden.

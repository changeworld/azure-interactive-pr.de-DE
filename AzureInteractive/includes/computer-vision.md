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
ms.openlocfilehash: 7e51d3cd0533b4fb64d7dfa783af55266d536f54
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079497"
---
An diesem Punkt ist die Anwendung eine funktionierende Galerie, die Ihnen das Hochladen und Anzeigen von Bildern ermöglicht. In diesem Modul wird beschrieben, wie Sie die Maschinelles Sehen-API von Microsoft Cognitive Services verwenden, um Bildtitel für hochgeladene Bilder zu generieren und die Bildtitel mit den Bildmetadaten in Cosmos DB zu speichern.

## <a name="create-a-computer-vision-account"></a>Erstellen eines Kontos vom Typ „Maschinelles Sehen“

Microsoft Cognitive Services ist eine Sammlung von Diensten, mit denen Entwickler ihre Anwendungen um intelligente Funktionen und Features erweitern können. Die Maschinelles Sehen-API ist ein serverloser Dienst, der Bilder mithilfe von erweiterten Algorithmen verarbeitet und Informationen zu jedem Bild zurückgibt. Sie verfügt über einen kostenlosen Tarif, in dem bis zu 5.000 API-Aufrufe pro Monat möglich sind.

1. Stellen Sie sicher, dass Sie noch an der Cloud Shell angemeldet sind. Wählen Sie andernfalls die Option **Enter focus mode** (Fokusmodus aktivieren), um ein Cloud Shell-Fenster zu öffnen. 

1. Erstellen Sie in Ihrer Ressourcengruppe ein neues Cognitive Services-Konto vom Typ **ComputerVision** mit einem eindeutigen Namen. Verwenden Sie für den kostenlosen Tarif **F0** als SKU. Falls Sie bereits über ein vorhandenes Konto vom Typ „Maschinelles Sehen“ verfügen, müssen Sie ggf. ein Standard-Konto (S1) erstellen. Hierfür können dann aber Kosten anfallen.

    ```azurecli
    az cognitiveservices account create -g first-serverless-app -n <computer vision account name> --kind ComputerVision --sku F0 -l westcentralus
    ```


## <a name="create-function-app-settings-for-computer-vision-url-and-key"></a>Erstellen von Funktions-App-Einstellungen für die Maschinelles Sehen-API und den Schlüssel

Zum Aufrufen der Maschinelles Sehen-API sind eine URL und ein Schlüssel erforderlich.

1. Ermitteln Sie den Schlüssel und die URL für die Maschinelles Sehen-API, und speichern Sie diese Angaben in Bash-Variablen.

    ```azurecli
    export COMP_VISION_KEY=$(az cognitiveservices account keys list -g first-serverless-app -n <computer vision account name> --query key1 --output tsv)
    ```
    ```azurecli
    export COMP_VISION_URL=$(az cognitiveservices account show -g first-serverless-app -n <computer vision account name> --query endpoint --output tsv)
    ```

1. Erstellen Sie in der Funktions-App App-Einstellungen mit dem Namen **COMP_VISION_KEY** bzw. **COMP_VISION_URL**.

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings COMP_VISION_KEY=$COMP_VISION_KEY COMP_VISION_URL=$COMP_VISION_URL -o table
    ```


## <a name="call-computer-vision-api-from-resizeimage-function"></a>Aufrufen der Maschinelles Sehen-API über die ResizeImage-Funktion

In den folgenden Schritten ändern Sie die **ResizeImage**-Funktion, um die Maschinelles Sehen-API aufzurufen und jedes hochgeladene Bild zu beschreiben und die Beschreibung in Cosmos DB zu speichern.

1. Öffnen Sie Ihre Funktions-App im Azure-Portal.

1. **C#**

    1. (C#) Verwenden Sie den linken Navigationsbereich, um nach der Funktion **ResizeImage** zu suchen und das zugehörige Codefenster zu öffnen.

    1. (C#) Ersetzen Sie den Code durch den Inhalt von [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx). In diesem Code wird `HttpClient` verwendet, um die Maschinelles Sehen-API aufzurufen und das Ergebnis in Cosmos DB zu speichern.

1. **JavaScript**

    1. (JavaScript) Für diese Funktion wird das `axios`-Paket aus npm benötigt, um einen HTTP-Aufruf an die Maschinelles Sehen-API zu senden. Klicken Sie zum Installieren des npm-Pakets im linken Navigationsbereich auf den Namen der Funktions-App und dann auf **Plattformfeatures**.

    1. (JavaScript) Klicken Sie auf **Konsole**, um ein Konsolenfenster anzuzeigen.

    1. (JavaScript) Führen Sie den Befehl `npm install axios` in der Konsole aus. Es kann ein oder zwei Minuten dauern, bis der Vorgang abgeschlossen ist.

    1. (JavaScript) Klicken Sie im linken Navigationsbereich auf den Namen der Funktion (**ResizeImage**), um die Funktion anzuzeigen, und ersetzen Sie dann den gesamten Inhalt von **index.js** durch den Inhalt von [**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js).

1. Klicken Sie unter dem Codefenster auf **Protokolle**, um den Protokollbereich zu erweitern.

1. Klicken Sie auf **Speichern**. Überprüfen Sie den Protokollbereich, um sicherzustellen, dass die Funktion erfolgreich gespeichert wird und keine Fehler vorliegen.


## <a name="test-the-application"></a>Testen der Anwendung

1. Öffnen Sie die Anwendung in einem Browser. Wählen Sie eine Bilddatei aus, und laden Sie sie hoch.

1. Nach einigen Sekunden sollte die Miniaturansicht des neuen Bilds auf der Seite angezeigt werden. Bewegen Sie den Mauszeiger auf das Bild, um die von „Maschinelles Sehen“ generierte Beschreibung anzuzeigen.


## <a name="summary"></a>Zusammenfassung

In diesem Modul haben Sie erfahren, wie Sie die automatische Generierung von Bildtiteln für jedes hochgeladene Bild verwenden können, indem Sie die Maschinelles Sehen-API von Microsoft Cognitive Services nutzen. Als Nächstes wird beschrieben, wie Sie der Anwendung die Authentifizierung hinzufügen, indem Sie die Azure App Service-Authentifizierung verwenden.
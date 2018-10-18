---
title: Includedatei
description: Includedatei
services: functions
author: ggailey777
manager: jeconnoc
ms.service: multiple
ms.topic: include
ms.date: 10/12/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 3779c2e130afa7ee8d5879f30a924e258b7a41e9
ms.sourcegitcommit: fdb43556b8dcf67cb39c18e532b5fab7ac53eaee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49315975"
---
Bei der von Ihnen erstellten Anwendung handelt es sich um eine Fotogalerie. Es wird clientseitiger JavaScript-Code verwendet, um APIs zum Hochladen und Anzeigen von Bildern aufzurufen. In diesem Modul erstellen Sie eine API, indem Sie eine serverlose Funktion verwenden, mit der eine URL mit zeitlicher Beschränkung zum Hochladen eines Bilds generiert wird. Die Webanwendung nutzt die generierte URL zum Hochladen eines Bilds in den Blobspeicher mit der [Blob-Dienst-REST-API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).

## <a name="create-a-blob-storage-container-for-images"></a>Erstellen eines Blobspeichercontainers für Bilder

Für die Anwendung ist ein separater Speichercontainer erforderlich, um Bilder hochladen und hosten zu können.

1. Stellen Sie sicher, dass Sie noch an der Cloud Shell (Bash) angemeldet sind. Wählen Sie andernfalls die Option **Enter focus mode** (Fokusmodus aktivieren), um ein Cloud Shell-Fenster zu öffnen.

1.  Erstellen Sie unter Ihrem Speicherkonto einen neuen Container mit dem Namen **images**, der über öffentlichen Zugriff auf alle Blobs verfügt.

    ```azurecli
    az storage container create -n images --account-name <storage account name> --public-access blob
    ```

## <a name="create-an-azure-function-app"></a>Erstellen einer Azure Function-App

Azure Functions ist ein Dienst zum Ausführen von serverlosen Funktionen. Eine serverlose Funktion kann von Ereignissen, z.B. einer HTTP-Anforderung, oder bei der Erstellung eines Blobs in einem Speichercontainer ausgelöst (aufgerufen) werden.

Eine Azure-Funktions-App ist ein Container für mindestens eine serverlose Funktion.

Erstellen Sie eine neue Azure-Funktions-App mit einem eindeutigen Namen in der Ressourcengruppe, die Sie zuvor erstellt haben. Verwenden Sie den Namen **first-serverless-app**. Für Funktions-Apps ist ein Storage-Konto erforderlich. In diesem Tutorial verwenden Sie das vorhandene Speicherkonto.

```azurecli
az functionapp create -n <function app name> -g first-serverless-app -s <storage account name> -c westcentralus
```

## <a name="configure-the-function-app"></a>Konfigurieren der Funktions-App

Für die Funktions-App in diesem Tutorial ist Version 1.x der Functions-Laufzeit erforderlich. Durch das Festlegen der Anwendungseinstellung `FUNCTIONS_EXTENSION_VERSION` auf `~1` wird für die Funktions-App die aktuelle 1.x-Version festgelegt. Legen Sie Anwendungseinstellungen mit dem Befehl [az functionapp config appsettings set](https://docs.microsoft.com/cli/azure/functionapp/config/appsettings#set) fest.

Im folgenden Azure CLI-Befehl steht „<app_name>“ für den Namen Ihrer Funktions-App.

```azurecli
az functionapp config appsettings set --name <function app name> --g first-serverless-app --settings FUNCTIONS_EXTENSION_VERSION=~1
```

## <a name="create-an-http-triggered-serverless-function"></a>Erstellen einer per HTTP ausgelösten serverlosen Funktion

Die Fotogalerie-Webanwendung sendet eine HTTP-Anforderung an die serverlose Funktion, um eine URL mit zeitlicher Beschränkung zu generieren, damit ein Bild auf sichere Weise in den Blobspeicher hochgeladen werden kann. Die Funktion wird per HTTP-Anforderung ausgelöst, und das Azure Storage SDK wird verwendet, um die sichere URL zu generieren und zurückzugeben.

1. Nachdem die Funktions-App erstellt wurde, können Sie im Azure-Portal über das Feld „Suchen“ danach suchen und sie durch Daraufklicken öffnen.

    ![Öffnen der Funktions-App](media/functions-first-serverless-web-app/2-search-function-app.png)

1. Bewegen Sie den Mauszeiger im linken Navigationsbereich des Fensters für die Funktions-App auf **Funktionen**, und klicken Sie auf **+**, um mit dem Erstellen einer neuen serverlosen Funktion zu beginnen.

    ![Erstellen einer neuen Funktion](media/functions-first-serverless-web-app/2-new-function.png)

1. Klicken Sie auf **Benutzerdefinierte Funktion**, um eine Liste mit Funktionsvorlagen anzuzeigen.

1. Suchen Sie nach der Vorlage **HttpTrigger**, und klicken Sie auf die gewünschte Sprache (C# oder JavaScript).

1. Verwenden Sie diese Werte, um eine Funktion zu erstellen, mit der eine URL für den Blob-Upload generiert wird.

    | Einstellung      |  Empfohlener Wert   | BESCHREIBUNG                                        |
    | --- | --- | ---|
    | **Sprache** | C# oder JavaScript | Wählen Sie die Sprache aus, die Sie verwenden möchten. |
    | **Name Ihrer Funktion** | GetUploadUrl | Geben Sie diesen Namen genau wie hier angezeigt ein, damit die Anwendung die Funktion ermitteln kann. |
    | **Autorisierungsstufe** | Anonym | Lässt den schnellen Zugriff auf die Funktion zu. |

    ![Eingeben der Einstellungen für eine neue Funktion, die per HTTP ausgelöst wird](media/functions-first-serverless-web-app/2-new-function-httptrigger.png)

1. Klicken Sie auf **Erstellen**, um die Funktion zu erstellen.

1. **C#** 

    1. Gehen Sie wie folgt vor, wenn der Quellcode der Funktion angezeigt wird: Ersetzen Sie den gesamten Inhalt von **run.csx** durch den Inhalt von [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx).

1. **JavaScript** 

    1. (JavaScript) Für diese Funktion ist das `azure-storage`-Paket aus npm erforderlich, um das SAS-Token (Shared Access Signature) zu generieren, das zum Erstellen der sicheren URL benötigt wird. Klicken Sie zum Installieren des npm-Pakets im linken Navigationsbereich auf den Namen der Funktions-App und dann auf **Plattformfeatures**.

    1. (JavaScript) Klicken Sie auf **Konsole**, um ein Konsolenfenster anzuzeigen.

        ![Öffnen eines Konsolenfensters](media/functions-first-serverless-web-app/2-open-console.jpg)

    1. (JavaScript) Vergewissern Sie sich, dass **d:\home\site\wwwroot** das aktuelle Verzeichnis ist, indem Sie den Befehl `cd d:\home\site\wwwroot` ausführen.

    1. (JavaScript) Führen Sie den Befehl `npm init -y` aus, um eine leere Datei mit dem Namen **package.json** zu erstellen.

    1. (JavaScript) Führen Sie den Befehl `npm install --save azure-storage` in der Konsole aus, um das Paket zu installieren und in **package.json** zu speichern. Es kann ein oder zwei Minuten dauern, bis der Vorgang abgeschlossen ist.

    1. (JavaScript) Klicken Sie im linken Navigationsbereich auf den Namen der Funktion (**GetUploadUrl**), um die Funktion anzuzeigen. Ersetzen Sie den gesamten Inhalt von **index.js** durch den Inhalt von [**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js).
    
        ![„index.js“ nach dem Update](media/functions-first-serverless-web-app/2-paste-js.jpg)

1. Klicken Sie unter dem Codefenster auf **Protokolle**, um den Protokollbereich zu erweitern.

1. Klicken Sie auf **Speichern**. Überprüfen Sie den Protokollbereich, um sicherzustellen, dass die Funktion erfolgreich kompiliert wird.

Mit der Funktion wird eine so genannte SAS-URL (Shared Access Signature) generiert, die zum Hochladen einer Datei in Blobspeicher genutzt wird. Die SAS-URL ist nur für kurze Zeit gültig, und es kann nur eine Datei hochgeladen werden. Die Dokumentation zum Blobspeicher enthält weitere Informationen zur [Verwendung von Shared Access Signatures](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).


## <a name="add-an-environment-variable-for-the-storage-connection-string"></a>Hinzufügen einer Umgebungsvariablen für die Speicherverbindungszeichenfolge

Für die erstellte Funktion wird eine Verbindungszeichenfolge für das Speicherkonto benötigt, damit die SAS-URL generiert werden kann. Anstatt die Verbindungszeichenfolge im Text der Funktion hartzucodieren, kann sie als Anwendungseinstellung gespeichert werden. Auf Anwendungseinstellungen kann von allen Funktionen der Funktions-App in Form von Umgebungsvariablen zugegriffen werden.

1. Fragen Sie in der Cloud Shell die Speicherkonto-Verbindungszeichenfolge ab, und speichern Sie sie in einer Bash-Variablen mit dem Namen **STORAGE_CONNECTION_STRING**.

    ```azurecli
    export STORAGE_CONNECTION_STRING=$(az storage account show-connection-string -n <storage account name> -g first-serverless-app --query "connectionString" --output tsv)
    ```

    Vergewissern Sie sich, dass das Festlegen der Variablen erfolgreich war.

    ```azurecli
    echo $STORAGE_CONNECTION_STRING
    ```

1. Erstellen Sie eine neue Anwendungseinstellung mit dem Namen **AZURE_STORAGE_CONNECTION_STRING**, indem Sie den im vorherigen Schritt gespeicherten Wert verwenden.

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings AZURE_STORAGE_CONNECTION_STRING=$STORAGE_CONNECTION_STRING -o table
    ```

    Stellen Sie sicher, dass die Ausgabe des Befehls die neue Anwendungseinstellung mit dem richtigen Wert enthält.


## <a name="test-the-serverless-function"></a>Testen der serverlosen Funktion

Zusätzlich zum Erstellen und Bearbeiten von Funktionen enthält das Azure-Portal auch ein integriertes Tool zum Testen von Funktionen.

1. Klicken Sie zum Testen der serverlosen HTTP-Funktion rechts vom Codefenster auf die Registerkarte **Test** (Testen), um den Testbereich zu erweitern.

1. Ändern Sie die **HTTP-Methode** in **GET**.

1. Klicken Sie unter **Abfrage** auf *Parameter hinzufügen*\*, und fügen Sie den folgenden Parameter hinzu:

    | name      |  value   | 
    | --- | --- |
    | filename | image1.jpg |

1. Klicken Sie im Testbereich auf **Ausführen**, um eine HTTP-Anforderung an die Funktion zu senden.

1. Die Funktion gibt in der Ausgabe eine Upload-URL zurück. Die Funktionsausführung wird im Protokollbereich angezeigt.

    ![Protokolle mit Informationen zur erfolgreichen Ausführung der Funktion](media/functions-first-serverless-web-app/2-test-function.png)


## <a name="configure-cors-in-the-function-app"></a>Konfigurieren von CORS in der Funktions-App

Da das Front-End der App im Blobspeicher gehostet wird, verfügt es über einen anderen Domänennamen als die Azure-Funktions-App. Damit der clientseitige JavaScript-Code die eben erstellte Funktion erfolgreich aufrufen kann, muss die Funktions-App für Cross-Origin Resource Sharing (CORS) konfiguriert sein.

1. Klicken Sie im Fenster der Funktions-App in der linken Navigationsleiste auf den Namen Ihrer Funktions-App.

1. Klicken Sie auf **Plattformfeatures**, um eine Liste mit erweiterten Features anzuzeigen.

1. Klicken Sie unter **API** auf **CORS**.

    ![Auswählen von CORS](media/functions-first-serverless-web-app/2-open-cors.jpg)

1. Fügen Sie aus dem vorherigen Modul einen zulässigen Ursprung für die Anwendungs-URL hinzu, und lassen Sie den nachgestellten Schrägstrich (**/**) weg (z.B. `https://firstserverlessweb.z4.web.core.windows.net`).

    ![CORS-Einstellungen mit hinzugefügter URL für serverlose Web-App](media/functions-first-serverless-web-app/2-add-cors.png)

1. Klicken Sie auf **Speichern**.

1. C#

   1. (C#) Navigieren Sie zurück zur Funktion `GetUploadUrl`, und wählen Sie dann die Registerkarte **Integrieren**.

   1. (C#) Wählen Sie unter „Ausgewählte HTTP-Methoden“ die Option **OPTIONS**.

      **GET**, **POST** und **OPTIONS** sollten jeweils ausgewählt sein. Für CORS wird die OPTIONS-Methode verwendet, die für C#-Funktionen nicht standardmäßig ausgewählt ist.  

   1. (C#) Klicken Sie auf **Speichern**.

1. Navigieren Sie im Portal zur Funktions-App, wählen Sie die Registerkarte **Übersicht**, und klicken Sie dann auf **Neu starten**, um sicherzustellen, dass die Änderungen für CORS wirksam sind.

## <a name="configure-cors-in-the-storage-account"></a>Konfigurieren von CORS im Storage-Konto

Da die App auch clientseitige JavaScript-Aufrufe zum Hochladen von Dateien an Blob Storage sendet, müssen Sie außerdem das Storage-Konto für CORS konfigurieren.

1. Führen Sie den folgenden Befehl aus, um für alle Ursprungsorte das Hochladen in das Storage-Konto zuzulassen.

    ```azurecli
    az storage cors add --methods OPTIONS PUT --origins '*' --exposed-headers '*' --allowed-headers '*' --services b --account-name <storage account name>
    ```


## <a name="modify-the-web-app-to-upload-images"></a>Ändern der Web-App zum Hochladen von Bildern

Die Web-App ruft Einstellungen aus einer Datei mit dem Namen **settings.js** ab. In den folgenden Schritten erstellen Sie die Datei mit der Cloud Shell und legen dann `window.apiBaseUrl` auf die URL der Funktions-App und `window.blobBaseUrl` auf die URL des Azure Blob Storage-Endpunkts fest.

1. Stellen Sie in der Cloud Shell sicher, dass das aktuelle Verzeichnis der Ordner **www/dist** ist.

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. Fragen Sie die URL der Funktions-App ab, und speichern Sie sie in einer Bash-Variablen mit dem Namen **FUNCTION_APP_URL**.

    ```azurecli
    export FUNCTION_APP_URL="https://"$(az functionapp show -n <function app name> -g first-serverless-app --query "defaultHostName" --output tsv)
    ```

    Vergewissern Sie sich, dass die Variable richtig festgelegt wurde.

    ```azurecli
    echo $FUNCTION_APP_URL
    ```

1. Erstellen Sie die Datei **settings.js**, und fügen Sie die Funktions-App-URL beispielsweise wie unten angegeben hinzu, um den Basis-URI der API-Aufrufe für Ihre Funktions-App festzulegen.

    `window.apiBaseUrl = 'https://fnapp@lab.GlobalLabInstanceId.azurewebsites.net'`

    Sie können die Änderung vornehmen, indem Sie den folgenden Befehl ausführen oder einen Befehlszeilen-Editor wie VIM verwenden.

    ```azurecli
    echo "window.apiBaseUrl = '$FUNCTION_APP_URL'" > settings.js
    ```

    Vergewissern Sie sich, dass das Schreiben der Datei erfolgreich war.

    ```azurecli
    cat settings.js
    ```

1. Fragen Sie die Blob Storage-Basis-URL ab, und speichern Sie sie in einer Bash-Variablen mit dem Namen **BLOB_BASE_URL**.

    ```azurecli
    export BLOB_BASE_URL=$(az storage account show -n <storage account name> -g first-serverless-app --query primaryEndpoints.blob -o tsv | sed 's/\/$//')
    ```

    Vergewissern Sie sich, dass die Variable richtig festgelegt wurde.

    ```azurecli
    echo $BLOB_BASE_URL
    ```

1. Fügen Sie die Speicher-URL an die Datei **settings.js** an (beispielsweise mit der folgenden Codezeile), um den Basis-URI der API-Aufrufe für Ihre Funktions-App festzulegen.

    `window.blobBaseUrl = 'https://mystorage.blob.core.windows.net'`

    Sie können die Änderung vornehmen, indem Sie den folgenden Befehl ausführen oder einen Befehlszeilen-Editor wie VIM verwenden.

    ```azurecli
    echo "window.blobBaseUrl = '$BLOB_BASE_URL'" >> settings.js
    ```

    Vergewissern Sie sich, dass das Schreiben der Datei erfolgreich war und dass sie jetzt zwei Zeilen enthält.

    ```azurecli
    cat settings.js
    ```

1. Laden Sie die Datei in Blobspeicher hoch.

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-web-application"></a>Testen der Webanwendung

An diesem Punkt kann die Galerieanwendung ein Bild in den Blobspeicher hochladen, aber es können noch keine Bilder angezeigt werden. Die Anwendung versucht, die Funktion `GetImages` aufzurufen. Sie ist aber noch nicht vorhanden, da Sie sie erst in einem späteren Modul erstellen. Der Aufruf ist nicht erfolgreich, und scheinbar hängt die Webseite mit einem Hinweis der Art „Wird analysiert...“, aber der Upload des gewählten Bilds ist trotzdem erfolgreich.

Sie können sich vergewissern, ob ein Bild erfolgreich hochgeladen wurde, indem Sie im Azure-Portal den Inhalt des Containers **images** überprüfen.

1. Navigieren Sie in einem Browserfenster zur Anwendung. Wählen Sie eine Bilddatei aus, und laden Sie sie hoch. Der Upload wird abgeschlossen. Da wir aber noch nicht die Möglichkeit zum Anzeigen von Bildern hinzugefügt haben, wird das hochgeladene Bild in der App nicht angezeigt. (Die Webseite scheint beim Vorgang „Bild wird analysiert...“ zu hängen. Dies beheben Sie später.)

1. Vergewissern Sie sich in der Cloud Shell, dass das Bild in den Container **images** hochgeladen wurde.

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. Löschen Sie alle Dateien im Container **images**, bevor Sie mit dem nächsten Tutorial fortfahren.

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```


## <a name="summary"></a>Zusammenfassung

In diesem Modul haben Sie eine Funktions-App erstellt und erfahren, wie Sie eine serverlose Funktion verwenden, um für eine Webanwendung das Hochladen von Bildern in Blobspeicher zuzulassen. Als Nächstes wird beschrieben, wie Sie Miniaturansichten für die hochgeladenen Bilder erstellen, indem Sie eine per Blob ausgelöste serverlose Funktion verwenden.

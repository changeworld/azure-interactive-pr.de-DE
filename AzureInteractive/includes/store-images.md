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
ms.openlocfilehash: 194a25dbf9abda80379aa5aab408ac4ffe9ab7f5
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460057"
---
Azure Cosmos DB ist der serverlose, global verteilte Microsoft-Datenbankdienst mit mehreren Modellen. In diesem Modul erfahren Sie, wie Sie Azure Functions zum Speichern und Abrufen von Bildmetadaten als JSON-Dokumente in Cosmos DB verwenden.

## <a name="create-a-cosmos-db-account-database-and-collection"></a>Erstellen eines Kontos, einer Datenbank und einer Sammlung für Cosmos DB

Ein Cosmos DB-Konto ist eine Azure-Ressource, die Cosmos DB-Datenbanken enthält.

1. Stellen Sie sicher, dass Sie noch an der Cloud Shell angemeldet sind.  Wählen Sie andernfalls die Option **Enter focus mode** (Fokusmodus aktivieren), um ein Cloud Shell-Fenster zu öffnen. 

1. Erstellen Sie ein Cosmos DB-Konto mit einem eindeutigen Namen in derselben Ressourcengruppe, in der Sie auch die anderen Ressourcen dieses Tutorials erstellt haben.

    ```azurecli
    az cosmosdb create -g first-serverless-app -n <cosmos db account name>
    ```

1. Erstellen Sie nach der Erstellung des Cosmos DB-Kontos im Konto eine neue Datenbank mit dem Namen **imagesdb**.

    ```azurecli
    az cosmosdb database create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb
    ```

1. Erstellen Sie nach der Erstellung der Datenbank darin eine neue Sammlung mit dem Namen **images**, die über einen Durchsatz von 400 Anforderungseinheiten (Request Units, RUs) verfügt.

    ```azurecli
    az cosmosdb collection create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb --collection-name images --throughput 400
    ```


## <a name="save-a-document-to-cosmos-db-when-a-thumbnail-is-created"></a>Speichern eines Dokuments in Cosmos DB bei der Erstellung einer Miniaturansicht

Mit der Cosmos DB-Ausgabebindung können Sie Dokumente in einer Cosmos DB-Sammlung über Azure Functions erstellen. In den folgenden Schritten konfigurieren Sie eine Cosmos DB-Ausgabebindung in der Funktion **ResizeImage** und ändern die Funktion, um ein zu speicherndes Dokument (Objekt) zurückzugeben.

1. Öffnen Sie die Funktions-App im Azure-Portal.

1. Erweitern Sie im linken Navigationsbereich die Funktion **ResizeImage**, und wählen Sie dann **Integrieren**.

1. Klicken Sie unter **Ausgaben** auf **Neue Ausgabe**.

1. Suchen Sie nach dem Element **Azure Cosmos DB**, und wählen Sie es aus. Klicken Sie dann auf **Auswählen**.

    ![Auswählen von „Neue Ausgabe“](media/functions-first-serverless-web-app/4-new-output.jpg)

1. Füllen Sie die Felder der **Azure Cosmos DB-Ausgabe** mit den unten angegebenen Werten.

    | Einstellung      |  Empfohlener Wert   | BESCHREIBUNG                                        |
    | --- | --- | ---|
    | **Dokumentparametername** | Wählen Sie **Funktionsrückgabewert verwenden**. | Der Wert des Textfelds wird automatisch auf **$return** festgelegt. |
    | **Datenbankname** | imagesdb | Verwenden Sie den Namen der Datenbank, die Sie erstellt haben. |
    | **Sammlungsname** | images | Verwenden Sie den Namen der Sammlung, die Sie erstellt haben. |

1. Klicken Sie neben **Azure Cosmos DB-Kontoverbindung** auf **Neu**. Wählen Sie das Cosmos DB-Konto aus, das Sie zuvor erstellt haben.

    ![Eingeben der Einstellungen für Azure Cosmos DB-Ausgabebindung](media/functions-first-serverless-web-app/4-cosmos-db-output.png)

1. Klicken Sie auf **Speichern**, um die Cosmos DB-Ausgabebindung zu erstellen.

1. Klicken Sie links auf den Funktionsnamen **ResizeImage**, um die Funktion zu öffnen.

1. **C#**

    1. (C#) Ändern Sie den Rückgabetyp der Funktion von **void** in **object**.

    1. (C#) Fügen Sie am Ende der Funktion den folgenden Codeblock hinzu, um das zu speichernde Dokument zurückzugeben:
    
        ```csharp
        return new {
            id = name,
            imgPath = "/images/" + name,
            thumbnailPath = "/thumbnails/" + name
        };
        ```
    
        ![„run.csx“ für Funktion „ResizeImages“ nach den Änderungen](media/functions-first-serverless-web-app/4-update-function.png)

1. **JavaScript**

    1. (JavaScript) Ändern Sie die `context.done()`-Anweisung in der `else`-Klausel, um das in Cosmos DB zu speichernde Dokument zurückzugeben.

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

1. Klicken Sie unter dem Codefenster auf **Protokolle**, um den Protokollbereich zu erweitern.

1. Klicken Sie auf **Speichern**. Überprüfen Sie den Protokollbereich, um sicherzustellen, dass die Funktion erfolgreich gespeichert wird und keine Fehler vorliegen.


## <a name="create-a-function-to-list-images-from-cosmos-db"></a>Erstellen einer Funktion zum Auflisten von Bildern aus Cosmos DB

Für die Webanwendung ist eine API zum Abrufen der Bildmetadaten aus Cosmos DB erforderlich. Mit den folgenden Schritten erstellen Sie eine per HTTP ausgelöste Funktion, bei der eine Cosmos DB-Eingabebindung erstellt wird, um die Datenbanksammlung abzufragen.

1. Bewegen Sie den Mauszeiger in Ihrer Funktions-App auf der linken Seite auf **Funktionen**, und klicken Sie auf **+**, um eine neue Funktion zu erstellen.

1. Suchen Sie nach der Vorlage **HttpTrigger**, und wählen Sie sie aus.

1. Verwenden Sie diese Werte, um eine Funktion zu erstellen, mit der eine URL zum Abrufen von Bildern generiert wird.

    | Einstellung      |  Empfohlener Wert   | BESCHREIBUNG                                        |
    | --- | --- | ---|
    | **Name Ihrer Funktion** | GetImages | Geben Sie diesen Namen genau wie hier angezeigt ein, damit die Anwendung die Funktion ermitteln kann. |
    | **Autorisierungsstufe** | Anonym | Lässt den schnellen Zugriff auf die Funktion zu. |

1. Klicken Sie auf **Create**.

1. Klicken Sie nach der Erstellung der neuen Funktion im linken Navigationsbereich unter dem Namen der Funktion auf **Integrieren**.

1. Klicken Sie auf **Neue Eingabe**, und wählen Sie **Azure Cosmos DB**. 

    ![Auswählen von „Neue Eingabe“](media/functions-first-serverless-web-app/4-new-input.jpg)

1. Klicken Sie auf **Auswählen**.

1. Geben Sie die folgenden Werte an:

    | Einstellung      |  Empfohlener Wert   | BESCHREIBUNG                                        |
    | --- | --- | ---|
    | **Dokumentparametername** | documents | Entspricht dem Parameternamen in der Funktion. |
    | **Datenbankname** | imagesdb |  |
    | **Sammlungsname** | images |  |
    | **SQL query** | select * from c order by c._ts desc | Dient zum Abrufen von Dokumenten (letzte Dokumente zuerst). |
    | **Azure Cosmos DB-Kontoverbindung** | Auswählen der vorhandenen Verbindungszeichenfolge |  |

1. Klicken Sie auf **Speichern**, um die Eingabebindung zu erstellen.

1. **C#**

    1. Klicken Sie auf den Namen der Funktion, um das Codefenster zu öffnen, und ersetzen Sie dann den gesamten Inhalt von **run.csx** durch den Inhalt von [**/csharp/GetImages/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetImages/run.csx).

1. **JavaScript**

    1. Klicken Sie auf den Namen der Funktion, um das Codefenster zu öffnen, und ersetzen Sie dann den gesamten Inhalt von **index.js** durch den Inhalt von [**/javascript/GetImages/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetImages/index.js).

1. Klicken Sie unter dem Codefenster auf **Protokolle**, um den Protokollbereich zu erweitern.

1. Klicken Sie auf **Speichern**. Überprüfen Sie den Protokollbereich, um sicherzustellen, dass die Funktion erfolgreich gespeichert wird und keine Fehler vorliegen.


## <a name="test-the-application"></a>Testen der Anwendung

1. Öffnen Sie die Anwendung in einem Browser. Wählen Sie eine Bilddatei aus, und laden Sie sie hoch.

1. Nach einigen Sekunden wird die Miniaturansicht des neuen Bilds auf der Seite angezeigt.

1. Verwenden Sie im Azure-Portal das Suchfeld, um anhand des Namens nach Ihrem Cosmos DB-Konto zu suchen. Klicken Sie darauf, um es zu öffnen.

1. Klicken Sie links auf **Daten-Explorer**, um Sammlungen und Dokumente zu durchsuchen.

1. Wählen Sie unter der Datenbank **imagesdb** die Sammlung **images** aus.

1. Vergewissern Sie sich, dass ein Dokument für das hochgeladene Bild erstellt wurde.

    ![Anzeige eines Dokuments für ein hochgeladenes Bild im Daten-Explorer](media/functions-first-serverless-web-app/4-data-explorer.png)



## <a name="summary"></a>Zusammenfassung

In diesem Modul haben Sie erfahren, wie Sie ein Konto, eine Datenbank und eine Sammlung für Cosmos DB erstellen. Außerdem wurde beschrieben, wie Sie die Cosmos DB-Bindungen zum Speichern und Abrufen von Bildmetadaten in der Cosmos DB-Sammlung verwenden. Als Nächstes wird beschrieben, wie Sie für jedes hochgeladene Bild mit Microsoft Cognitive Services automatisch einen Bildtitel generieren können.

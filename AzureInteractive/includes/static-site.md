Azure Blob Storage ist ein kostengünstiger und extrem skalierbarer Dienst, der zum Hosten von statischen Dateien verwendet werden kann. Für dieses Tutorial verwenden Sie den Dienst zum Bereitstellen von statischem Inhalt (z.B. HTML, JavaScript, CSS) für die von Ihnen erstellte Web-App.

## <a name="create-a-storage-account"></a>Erstellen eines Speicherkontos

Ein Storage-Konto ist eine Azure-Ressource, mit der Sie Tabellen, Warteschlangen, Dateien, Blobs (Objekte) und VM-Datenträger speichern können.

1. Melden Sie sich an der Cloud Shell (Bash) an, indem Sie die Schaltfläche **Enter focus mode** (Fokusmodus aktivieren) verwenden. Diese Schaltfläche befindet sich oben rechts oder unten auf der Seite. Dies hängt davon ab, wie breit Ihr Browserfenster dargestellt wird. Im Fokusmodus wird ein Cloud Shell-Fenster im Browserfenster auf der rechten Seite angedockt, damit Sie im Tutorial angezeigte Befehle leicht ausführen können.

1. In Azure ist eine Ressourcengruppe ein Container, der verwandte Azure-Ressourcen enthält, um die Verwaltung zu vereinfachen. Erstellen Sie eine neue Ressourcengruppe mit dem Namen **first-serverless-app**.

    ```azurecli
    az group create -n first-serverless-app -l westcentralus
    ```

1. Der statische Inhalt (HTML-, CSS- und JavaScript-Dateien) für dieses Tutorial wird in Blob Storage gehostet. Für Blob Storage ist ein Storage-Konto erforderlich. Erstellen Sie in der Ressourcengruppe ein Storage-Konto (Allgemein V2). Ersetzen Sie `<storage account name>` durch einen eindeutigen Namen.

    ```azurecli
    az storage account create -n <storage account name> -g first-serverless-app --kind StorageV2 -l westcentralus --https-only true --sku Standard_LRS
    ```

1. Suchen Sie über die Suchleiste oben im [Azure-Portal](https://portal.azure.com) nach dem Speicherkonto, das Sie gerade erstellt haben, und öffnen Sie es.

1. Wählen Sie im linken Navigationsbereich die Option **Statische Website (Vorschau)**, um einen Container für das Hosten von statischen Websites zu konfigurieren.
    - Wählen Sie **Aktiviert**, um die statische Website zu aktivieren.
    - Geben Sie *index.html* als Name des Indexdokuments ein. Das Feld enthält bereits den Text *index.html* in grauer Schrift, aber dies ist nur ein Beispieltext. Sie müssen den Wert trotzdem in das Feld eingeben.
    - Klicken Sie auf **Speichern**.
    
    ![Eingeben von Einstellungen für „Statische Website“](media/functions-first-serverless-web-app/1-storage-static-website.png)

1. Speichern Sie den **primären Endpunkt** an einem Ort, an dem Sie ihn beim Durcharbeiten des Tutorials bequem kopieren können. Dies ist die URL Ihrer Webanwendung.

## <a name="upload-the-web-application"></a>Hochladen der Webanwendung

1. Die Quelldateien für die Anwendung, die Sie in diesem Tutorial erstellen, befinden sich in einem [GitHub-Repository](https://github.com/Azure-Samples/functions-first-serverless-web-application). Stellen Sie sicher, dass Sie sich in der Cloud Shell in Ihrem Basisverzeichnis befinden, und klonen Sie dieses Repository.

    ```azurecli
    cd ~
    git clone https://github.com/Azure-Samples/functions-first-serverless-web-application
    ```

    Das Repository wird in `/home/<username>/functions-first-serverless-web-application` geklont.

1. Die clientseitige Webanwendung befindet sich im Ordner **www** und wird mit dem JavaScript-Framework „Vue.js“ erstellt. Wechseln Sie in den Ordner, und führen Sie npm-Befehle aus, um die Abhängigkeiten der Anwendung zu installieren und die Anwendung zu erstellen. Es kann mehrere Minuten dauern, bis der letzte dieser Befehle abgeschlossen ist.

    ```azurecli
    cd ~/functions-first-serverless-web-application/www
    npm install
    npm run generate
    ```

    Die Anwendung wird im Ordner **dist** generiert.

1. Machen Sie **dist** zum aktuellen Verzeichnis, und laden Sie die Anwendung in den Blobcontainer **$web** hoch.

    ```azurecli
    cd dist
    az storage blob upload-batch -s . -d \$web --account-name <storage account name>
    ```

1. Öffnen Sie die URL des primären Endpunkts für statische Storage-Websites in einem Webbrowser, um die Anwendung anzuzeigen.

    ![Startseite der ersten serverlosen Web-App](media/functions-first-serverless-web-app/1-app-screenshot-new.png)


## <a name="summary"></a>Zusammenfassung

In diesem Modul haben Sie eine Ressourcengruppe mit dem Namen **first-serverless-app** erstellt, die ein Storage-Konto enthält. In einem Blobcontainer mit dem Namen **$web** im Storage-Konto wird der statische Inhalt für Ihre Webanwendung gespeichert und der Inhalt öffentlich verfügbar gemacht. Als Nächstes erfahren Sie, wie Sie eine serverlose Funktion zum Hochladen von Bildern in Blobspeicher aus dieser Anwendung verwenden.

<span data-ttu-id="20dc1-101">Azure Blob Storage ist ein kostengünstiger und extrem skalierbarer Dienst, der zum Hosten von statischen Dateien verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="20dc1-101">Azure Blob storage is a low-cost and massively scalable service that can be used to host static files.</span></span> <span data-ttu-id="20dc1-102">Für dieses Tutorial verwenden Sie den Dienst zum Bereitstellen von statischem Inhalt (z.B. HTML, JavaScript, CSS) für die von Ihnen erstellte Web-App.</span><span class="sxs-lookup"><span data-stu-id="20dc1-102">For this tutorial, you use it to serve static content (for example, HTML, JavaScript, CSS) for the web app that you build.</span></span>

## <a name="create-a-storage-account"></a><span data-ttu-id="20dc1-103">Erstellen eines Speicherkontos</span><span class="sxs-lookup"><span data-stu-id="20dc1-103">Create a Storage account</span></span>

<span data-ttu-id="20dc1-104">Ein Storage-Konto ist eine Azure-Ressource, mit der Sie Tabellen, Warteschlangen, Dateien, Blobs (Objekte) und VM-Datenträger speichern können.</span><span class="sxs-lookup"><span data-stu-id="20dc1-104">A Storage account is an Azure resource that allows you to store tables, queues, files, blobs (objects), and virtual machine disks.</span></span>

1. <span data-ttu-id="20dc1-105">Melden Sie sich an der Cloud Shell (Bash) an, indem Sie die Schaltfläche **Enter focus mode** (Fokusmodus aktivieren) verwenden.</span><span class="sxs-lookup"><span data-stu-id="20dc1-105">Log in to the Cloud Shell (Bash), by selecting the **Enter focus mode** button.</span></span> <span data-ttu-id="20dc1-106">Diese Schaltfläche befindet sich oben rechts oder unten auf der Seite. Dies hängt davon ab, wie breit Ihr Browserfenster dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="20dc1-106">This button is at the top right or the bottom of the page, depending on how wide your browser window is.</span></span> <span data-ttu-id="20dc1-107">Im Fokusmodus wird ein Cloud Shell-Fenster im Browserfenster auf der rechten Seite angedockt, damit Sie im Tutorial angezeigte Befehle leicht ausführen können.</span><span class="sxs-lookup"><span data-stu-id="20dc1-107">Focus mode docks a Cloud Shell window on the right side of your browser window, so you can easily execute commands that are shown in the tutorial.</span></span>

1. <span data-ttu-id="20dc1-108">In Azure ist eine Ressourcengruppe ein Container, der verwandte Azure-Ressourcen enthält, um die Verwaltung zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="20dc1-108">In Azure, a Resource Group is a container that holds related Azure resources for ease of management.</span></span> <span data-ttu-id="20dc1-109">Erstellen Sie eine neue Ressourcengruppe mit dem Namen **first-serverless-app**.</span><span class="sxs-lookup"><span data-stu-id="20dc1-109">Create a new resource group named **first-serverless-app**.</span></span>

    ```azurecli
    az group create -n first-serverless-app -l westcentralus
    ```

1. <span data-ttu-id="20dc1-110">Der statische Inhalt (HTML-, CSS- und JavaScript-Dateien) für dieses Tutorial wird in Blob Storage gehostet.</span><span class="sxs-lookup"><span data-stu-id="20dc1-110">The static content (HTML, CSS, and JavaScript files) for this tutorial is hosted in Blob Storage.</span></span> <span data-ttu-id="20dc1-111">Für Blob Storage ist ein Storage-Konto erforderlich.</span><span class="sxs-lookup"><span data-stu-id="20dc1-111">Blob Storage requires a Storage account.</span></span> <span data-ttu-id="20dc1-112">Erstellen Sie in der Ressourcengruppe ein Storage-Konto (Allgemein V2).</span><span class="sxs-lookup"><span data-stu-id="20dc1-112">Create a Storage account (general purpose V2) in the resource group.</span></span> <span data-ttu-id="20dc1-113">Ersetzen Sie `<storage account name>` durch einen eindeutigen Namen.</span><span class="sxs-lookup"><span data-stu-id="20dc1-113">Replace `<storage account name>` with a unique name.</span></span>

    ```azurecli
    az storage account create -n <storage account name> -g first-serverless-app --kind StorageV2 -l westcentralus --https-only true --sku Standard_LRS
    ```

1. <span data-ttu-id="20dc1-114">Suchen Sie über die Suchleiste oben im [Azure-Portal](https://portal.azure.com) nach dem Speicherkonto, das Sie gerade erstellt haben, und öffnen Sie es.</span><span class="sxs-lookup"><span data-stu-id="20dc1-114">Using the Search bar at the top of the [Azure portal](https://portal.azure.com), find the storage account you just created and open it.</span></span>

1. <span data-ttu-id="20dc1-115">Wählen Sie im linken Navigationsbereich die Option **Statische Website (Vorschau)**, um einen Container für das Hosten von statischen Websites zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="20dc1-115">On the left navigation, select **Static website (preview)** to configure a container for static website hosting.</span></span>
    - <span data-ttu-id="20dc1-116">Wählen Sie **Aktiviert**, um die statische Website zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="20dc1-116">Select **Enabled** to enable static website.</span></span>
    - <span data-ttu-id="20dc1-117">Geben Sie *index.html* als Name des Indexdokuments ein.</span><span class="sxs-lookup"><span data-stu-id="20dc1-117">Enter *index.html* as the index document name.</span></span> <span data-ttu-id="20dc1-118">Das Feld enthält bereits den Text *index.html* in grauer Schrift, aber dies ist nur ein Beispieltext. Sie müssen den Wert trotzdem in das Feld eingeben.</span><span class="sxs-lookup"><span data-stu-id="20dc1-118">The field already has *index.html* in a gray font but this is only example text; you still have to enter that value in the field.</span></span>
    - <span data-ttu-id="20dc1-119">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="20dc1-119">Click **Save**.</span></span>
    
    ![Eingeben von Einstellungen für „Statische Website“](media/functions-first-serverless-web-app/1-storage-static-website.png)

1. <span data-ttu-id="20dc1-121">Speichern Sie den **primären Endpunkt** an einem Ort, an dem Sie ihn beim Durcharbeiten des Tutorials bequem kopieren können.</span><span class="sxs-lookup"><span data-stu-id="20dc1-121">Save the **Primary Endpoint** somewhere you can conveniently copy it from while working through the tutorial.</span></span> <span data-ttu-id="20dc1-122">Dies ist die URL Ihrer Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="20dc1-122">This is the URL of your web application.</span></span>

## <a name="upload-the-web-application"></a><span data-ttu-id="20dc1-123">Hochladen der Webanwendung</span><span class="sxs-lookup"><span data-stu-id="20dc1-123">Upload the web application</span></span>

1. <span data-ttu-id="20dc1-124">Die Quelldateien für die Anwendung, die Sie in diesem Tutorial erstellen, befinden sich in einem [GitHub-Repository](https://github.com/Azure-Samples/functions-first-serverless-web-application).</span><span class="sxs-lookup"><span data-stu-id="20dc1-124">The source files for the application that you build in this tutorial are located in a [GitHub repository](https://github.com/Azure-Samples/functions-first-serverless-web-application).</span></span> <span data-ttu-id="20dc1-125">Stellen Sie sicher, dass Sie sich in der Cloud Shell in Ihrem Basisverzeichnis befinden, und klonen Sie dieses Repository.</span><span class="sxs-lookup"><span data-stu-id="20dc1-125">Make sure that you are in your home directory in Cloud Shell and clone this repository.</span></span>

    ```azurecli
    cd ~
    git clone https://github.com/Azure-Samples/functions-first-serverless-web-application
    ```

    <span data-ttu-id="20dc1-126">Das Repository wird in `/home/<username>/functions-first-serverless-web-application` geklont.</span><span class="sxs-lookup"><span data-stu-id="20dc1-126">The repository is cloned to `/home/<username>/functions-first-serverless-web-application`.</span></span>

1. <span data-ttu-id="20dc1-127">Die clientseitige Webanwendung befindet sich im Ordner **www** und wird mit dem JavaScript-Framework „Vue.js“ erstellt.</span><span class="sxs-lookup"><span data-stu-id="20dc1-127">The client-side web application is located in the **www** folder and is built using the Vue.js JavaScript framework.</span></span> <span data-ttu-id="20dc1-128">Wechseln Sie in den Ordner, und führen Sie npm-Befehle aus, um die Abhängigkeiten der Anwendung zu installieren und die Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="20dc1-128">Change into the folder and run npm commands to install the application's dependencies and build the application.</span></span> <span data-ttu-id="20dc1-129">Es kann mehrere Minuten dauern, bis der letzte dieser Befehle abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="20dc1-129">The last of these commands might take several minutes to complete.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www
    npm install
    npm run generate
    ```

    <span data-ttu-id="20dc1-130">Die Anwendung wird im Ordner **dist** generiert.</span><span class="sxs-lookup"><span data-stu-id="20dc1-130">The application is generated in the **dist** folder.</span></span>

1. <span data-ttu-id="20dc1-131">Machen Sie **dist** zum aktuellen Verzeichnis, und laden Sie die Anwendung in den Blobcontainer **$web** hoch.</span><span class="sxs-lookup"><span data-stu-id="20dc1-131">Change the current directory to **dist** and upload the application to the **$web** Blob container.</span></span>

    ```azurecli
    cd dist
    az storage blob upload-batch -s . -d \$web --account-name <storage account name>
    ```

1. <span data-ttu-id="20dc1-132">Öffnen Sie die URL des primären Endpunkts für statische Storage-Websites in einem Webbrowser, um die Anwendung anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="20dc1-132">Open the Storage static websites primary endpoint URL in a web browser to view the application.</span></span>

    ![Startseite der ersten serverlosen Web-App](media/functions-first-serverless-web-app/1-app-screenshot-new.png)


## <a name="summary"></a><span data-ttu-id="20dc1-134">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="20dc1-134">Summary</span></span>

<span data-ttu-id="20dc1-135">In diesem Modul haben Sie eine Ressourcengruppe mit dem Namen **first-serverless-app** erstellt, die ein Storage-Konto enthält.</span><span class="sxs-lookup"><span data-stu-id="20dc1-135">In this module, you created a resource group named **first-serverless-app** containing a Storage account.</span></span> <span data-ttu-id="20dc1-136">In einem Blobcontainer mit dem Namen **$web** im Storage-Konto wird der statische Inhalt für Ihre Webanwendung gespeichert und der Inhalt öffentlich verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="20dc1-136">A blob container named **$web** in the Storage account stores the static content for your web application and makes the content available publicly.</span></span> <span data-ttu-id="20dc1-137">Als Nächstes erfahren Sie, wie Sie eine serverlose Funktion zum Hochladen von Bildern in Blobspeicher aus dieser Anwendung verwenden.</span><span class="sxs-lookup"><span data-stu-id="20dc1-137">Next, you learn how to use a serverless function to upload images to Blob storage from this web application.</span></span>

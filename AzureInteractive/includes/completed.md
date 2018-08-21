---
title: Includedatei
description: Includedatei
services: functions
author: tdykstra
manager: jeconnoc
ms.service: multiple
ms.topic: include
ms.date: 06/25/2018
ms.author: tdykstra
ms.custom: include file
ms.openlocfilehash: 1c4aaf7afd9fbc54dd34c2cca13a8b74e947c16a
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079475"
---
Sie haben mithilfe von Azure-Diensten eine serverlose Anwendung mit vollem Funktionsumfang erstellt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Arbeit mit dieser Anwendung beendet haben, können Sie den folgenden Befehl verwenden, um alle im Tutorial erstellten Ressourcen zu löschen:

```azurecli
az group delete --name first-serverless-app
```

Geben Sie `y` ein, wenn Sie dazu aufgefordert werden.  

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:
> [!div class="checklist"]
> * Konfigurieren von Azure-Blobspeicher zum Hosten einer statischen Website und von hochgeladenen Bildern
> * Hochladen von Bildern in Azure-Blobspeicher mit Azure Functions
> * Ändern der Größe von Bildern mit Azure Functions
> * Speichern von Bildmetadaten in Azure Cosmos DB
> * Verwenden der Maschinelles Sehen-API von Cognitive Services zum automatischen Generieren von Bildtiteln
> * Nutzen von Azure Active Directory zum Schützen der Web-App durch das Authentifizieren von Benutzern

Fahren Sie mit dem Logic Apps-Tutorial fort, um zu erfahren, wie Sie noch mehr Dienste mit Functions verbinden. 

> [!div class="nextstepaction"]
> [Erstellen einer Funktion, die in Azure Logic Apps integriert ist](https://docs.microsoft.com/azure/azure-functions/functions-twitter-email)
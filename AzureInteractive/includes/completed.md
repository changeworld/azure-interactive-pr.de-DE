---
title: Includedatei
description: Includedatei
services: functions
author: ggailey777
manager: jeconnoc
ms.service: multiple
ms.topic: include
ms.date: 06/25/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: a66fcc3a406c79fcf9881ddaaaf8330f5b373043
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47459995"
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

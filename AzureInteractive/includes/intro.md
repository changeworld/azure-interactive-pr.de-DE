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
ms.openlocfilehash: 0f86f2698a3a0c1e20272c335b63faf03b4b92d6
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460023"
---
In diesem Tutorial stellen Sie eine einfache Webanwendung bereit, mit der eine HTML-basierte Benutzeroberfläche dargestellt wird. Mit einem serverlosen Back-End kann die Anwendung Bilder hochladen und automatisch Bildtitel mit Beschreibungen abrufen.

![Ausführen der Web-App](media/functions-first-serverless-web-app/0-app-screenshot-finished.png)

Im folgenden Diagramm sind die Azure-Dienste aufgeführt, die von der Anwendung verwendet werden:

1. Blob Storage stellt statischen Webinhalt (HTML, CSS, JS) bereit und speichert Bilder.
2. Azure Functions verwaltet das Hochladen von Bildern, die Größenänderung und die Speicherung von Metadaten.
3. In Cosmos DB werden Bildmetadaten gespeichert.
4. Logic Apps ruft Bildtitel über die Maschinelles Sehen-API ab.
5. Azure Active Directory wird zum Verwalten der Benutzerauthentifizierung verwendet.

![Diagramm der Lösungsarchitektur](media/functions-first-serverless-web-app/0-architecture.jpg)

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Konfigurieren von Azure-Blobspeicher zum Hosten einer statischen Website und von hochgeladenen Bildern
> * Hochladen von Bildern in Azure-Blobspeicher mit Azure Functions
> * Ändern der Größe von Bildern mit Azure Functions
> * Speichern von Bildmetadaten in Azure Cosmos DB
> * Verwenden der Maschinelles Sehen-API von Cognitive Services zum automatischen Generieren von Bildtiteln
> * Nutzen von Azure Active Directory zum Schützen der Web-App durch das Authentifizieren von Benutzern

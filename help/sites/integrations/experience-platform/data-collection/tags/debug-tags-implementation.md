---
title: Debuggen einer Tags-Implementierung
description: Eine Einführung in einige gängige Tools und Techniken zum Debuggen einer Tag-Implementierung. Erfahren Sie, wie Sie mit der Entwicklerkonsole des Browsers und der Experience Platform Debugger-Erweiterung wichtige Aspekte einer Tags-Implementierung identifizieren und beheben können.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 6047
thumbnail: 38567.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---

# Debuggen einer Tags-Implementierung {#debug-tags-implementation}

Eine Einführung in gängige Tools und Techniken, die zum Debugging einer Tag-Implementierung verwendet werden. Erfahren Sie, wie Sie mit der Entwicklerkonsole des Browsers und der Experience Platform Debugger-Erweiterung wichtige Aspekte einer Tags-Implementierung identifizieren und beheben können.

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## Clientseitige Debugging über ein Satelliten-Objekt

Das clientseitige Debugging ist hilfreich, um das Laden der Tag-Eigenschaft-Regel oder die Ausführungsreihenfolge zu überprüfen. Wenn eine Tag-Eigenschaft zur Website hinzugefügt wird, wird die `_satellite` Im Browser ist ein JavaScript-Objekt vorhanden, um das clientseitige Ereignis- und Daten-Tracking zu erleichtern.

Um das clientseitige Debugging zu aktivieren, rufen Sie die `setDebug(true)` -Methode `_satellite` -Objekt.

1. Öffnen Sie die Browser-Konsole und führen Sie den folgenden Befehl aus.

   ```javascript
       _satellite.setDebug(true);
   ```

1. Laden Sie die AEM Site-Seite neu und überprüfen Sie, ob das Konsolenprotokoll angezeigt wird. _Regel ausgelöst_ wie unten beschrieben.

   ![Tag-Eigenschaft auf Autoren- und Veröffentlichungsseiten](assets/satellite-object-debugging.png)

## Debugging über Adobe Experience Platform Debugger

Adobe bietet Adobe Experience Platform Debugger [Chrome-Erweiterung](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) und [Firefox-Add-on](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) , um die Integration zu debuggen, zu verstehen und Einblicke in sie zu erhalten.

1. Öffnen Sie die Adobe Experience Platform Debugger-Erweiterung und öffnen Sie die Site-Seite auf der Veröffentlichungsinstanz.

1. Im **Adobe Experience Platform Debugger > Zusammenfassung > Adobe Experience Platform-Tags** überprüfen Sie die Details Ihrer Tag-Eigenschaft wie Name, Version, Build-Datum, Umgebung und Erweiterungen.

   ![Adobe Experience Platform Debugger- und Tag-Eigenschaftendetails](assets/tag-property-details.png)

## Zusätzliche Ressourcen {#additional-resources}

+ [Einführung in den Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

+ [Satellitenobjektreferenz](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html)

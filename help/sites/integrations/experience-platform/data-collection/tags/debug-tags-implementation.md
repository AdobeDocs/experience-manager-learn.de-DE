---
title: Debugging einer Tag-Implementierung
description: Eine Einführung in einige gängige Tools und Techniken zum Debugging einer Tag-Implementierung. Erfahren Sie, wie Sie mit der Developer Console des Browsers und der Experience Platform Debugger-Erweiterung wichtige Aspekte einer Tags-Implementierung identifizieren und beheben können.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-6047
thumbnail: 38567.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 100%

---

# Debugging einer Tag-Implementierung {#debug-tags-implementation}

Eine Einführung in gängige Tools und Techniken, die zum Debugging einer Tag-Implementierung verwendet werden. Erfahren Sie, wie Sie mit der Developer Console des Browsers und der Experience Platform Debugger-Erweiterung wichtige Aspekte einer Tags-Implementierung identifizieren und beheben können.

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## Client-seitiges Debugging über Satelliten-Objekt

Das Client-seitige Debugging ist hilfreich, um das Laden der Tag-Eigenschaft-Regel oder die Ausführungsreihenfolge zu überprüfen. Wenn eine Tag-Eigenschaft zur Website hinzugefügt wird, ist das `_satellite`-JavaScript-Objekt im Browser vorhanden, um das Client-seitige Ereignis- und Daten-Tracking zu erleichtern.

Um das Client-seitige Debugging zu aktivieren, rufen Sie die `setDebug(true)`-Methode für das `_satellite`-Objekt auf.

1. Öffnen Sie die Browser-Konsole und führen Sie den folgenden Befehl aus.

   ```javascript
       _satellite.setDebug(true);
   ```

1. Laden Sie die AEM-Seite neu und überprüfen Sie, ob das Konsolenprotokoll die Meldung _Regel ausgelöst_ wie unten beschrieben anzeigt.

   ![Tag-Eigenschaft auf Autoren- und Veröffentlichungsseiten](assets/satellite-object-debugging.png)

## Debugging über Adobe Experience Platform Debugger

Adobe bietet Adobe Experience Platform Debugger als [Chrome-Erweiterung](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) und [Firefox-Add-on](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/), um die Integration zu debuggen, zu verstehen und Einblicke in sie zu erhalten.

1. Öffnen Sie die Adobe Experience Platform Debugger-Erweiterung und öffnen Sie die Site-Seite auf der Veröffentlichungsinstanz.

1. Im **Adobe Experience Platform Debugger > Zusammenfassung > Adobe Experience Platform-Tags** überprüfen Sie die Details Ihrer Tag-Eigenschaft wie Name, Version, Build-Datum, Umgebung und Erweiterungen.

   ![Adobe Experience Platform Debugger- und Tag-Eigenschaftsdetails](assets/tag-property-details.png)

## Zusätzliche Ressourcen {#additional-resources}

+ [Einführung in Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=de)

+ [Satellitenobjektreferenz](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html?lang=de)

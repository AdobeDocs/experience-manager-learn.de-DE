---
title: Entwickeln von Ressourcenstatus in AEM Sites
description: 'Adobe Experience Managers Resource Status-APIs ist ein Plug-in-fähiges Framework für die Anzeige von Statusnachrichten in AEM verschiedenen Editor-Web-Benutzeroberflächen. '
topics: development
audience: developer
doc-type: tutorial
activity: develop
version: 6.4, 6.5
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 3%

---


# Entwickeln von Ressourcenstatus {#developing-resource-statuses-in-aem-sites}

Adobe Experience Managers Resource Status-APIs ist ein Plug-in-fähiges Framework für die Anzeige von Statusnachrichten in AEM verschiedenen Editor-Web-Benutzeroberflächen.

## Übersicht {#overview}

Das Framework &quot;Resource Status for Editors&quot;bietet serverseitige und Client-seitige APIs für die standardmäßige und einheitliche Anzeige und Interaktion mit den Editorstatus.

Die Statusleisten des Editors sind nativ in den Editoren Seite, Experience Fragment und Vorlage von AEM verfügbar.

Anwendungsbeispiele für benutzerdefinierte Ressourcenstatusanbieter sind:

* Benachrichtigung für Autoren, wenn eine Seite innerhalb von 2 Stunden nach geplanter Aktivierung liegt
* Benachrichtigung der Autoren, dass eine Seite innerhalb der letzten 15 Minuten aktiviert wurde
* Indem Sie Autoren darüber informieren, dass eine Seite in den letzten 5 Minuten bearbeitet wurde und von wem

![Übersicht über den Ressourcenstatus AEM Editors](assets/sample-editor-resource-status-screenshot.png)

## Framework für den Ressourcenstatus {#resource-status-provider-framework}

Bei der Entwicklung benutzerdefinierter Ressourcenstatus umfasst die Entwicklungsarbeit Folgendes:

1. Die ResourceStatusProvider-Implementierung, die für die Bestimmung des Status benötigt wird, und die grundlegenden Informationen zum Status: Titel, Nachricht, Priorität, Variante, Symbol und verfügbare Aktionen.
2. Optional kann GraniteUI-JavaScript verwendet werden, das die Funktionalität aller verfügbaren Aktionen implementiert.

   ![Architektur des Ressourcenstatus](assets/sample-editor-resource-status-application-architecture.png)

3. Die im Seiten-, Experience Fragment- und Vorlagen-Editor bereitgestellte Statusressource erhält über die Ressourcen einen Typ[!DNL statusType]&quot;.

   * Seiten-Editor: `editor`
   * Experience Fragment-Editor: `editor`
   * Vorlagen-Editor: `template-editor`

4. Die Statusressource `statusType` wird mit registriert abgeglichen `CompositeStatusType` OSGi-Konfiguration `name` -Eigenschaft.

   Bei allen Treffern wird die `CompositeStatusType's` werden erfasst und zur Erfassung der `ResourceStatusProvider` Implementierungen mit diesem Typ, via `ResourceStatusProvider.getType()`.

5. Die `ResourceStatusProvider` wird übergeben, `resource` im Editor und bestimmt, ob die `resource` hat den Status angezeigt werden. Wenn der Status erforderlich ist, ist diese Implementierung für das Erstellen von 0 oder vielen `ResourceStatuses` zurück, wobei jede einen anzuzeigenden Status darstellt.

   In der Regel wird ein `ResourceStatusProvider` 0 oder 1 zurückgibt `ResourceStatus` per `resource`.

6. ResourceStatus ist eine Benutzeroberfläche, die vom Kunden oder der `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` kann verwendet werden, um einen Status zu erstellen. Ein Status besteht aus:

   * Titel
   * Nachricht
   * Symbol
   * Variante
   * Priorität
   * Aktionen
   * Daten

7. Optional, wenn `Actions` für die `ResourceStatus` -Objekt, das clientlibs unterstützt, erforderlich sind, um die Funktionalität an die Aktionslinks in der Statusleiste zu binden.

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. Alle unterstützenden JavaScript- oder CSS-Elemente zur Unterstützung der Aktionen müssen durch die jeweiligen Client-Bibliotheken der einzelnen Editoren bereitgestellt werden, um sicherzustellen, dass der Frontend-Code im Editor verfügbar ist.

   * Kategorie des Seiteneditors: `cq.authoring.editor.sites.page`
   * Experience Fragment-Editor-Kategorie: `cq.authoring.editor.sites.page`
   * Kategorie des Vorlageneditors: `cq.authoring.editor.sites.template`

## Anzeigen des Codes {#view-the-code}

[Siehe Code auf GitHub](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## Zusätzliche Ressourcen {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)

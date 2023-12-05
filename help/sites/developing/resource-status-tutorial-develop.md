---
title: Entwickeln von Ressourcenstatus in AEM Sites
description: Die Ressourcenstatus-API von Adobe Experience Manager ist ein Plug-in-Framework für die Darstellung von Statusmeldungen in den Web-Benutzeroberflächen der verschiedenen AEM-Editoren.
doc-type: Tutorial
version: 6.4, 6.5
duration: 133
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 100%

---


# Entwickeln von Ressourcenstatus {#developing-resource-statuses-in-aem-sites}

Die Ressourcenstatus-API von Adobe Experience Manager ist ein Plug-in-Framework für die Darstellung von Statusmeldungen in den Web-Benutzeroberflächen der verschiedenen AEM-Editoren.

## Übersicht {#overview}

Das Framework „Resource Status for Editors“ bietet Server-seitige und Client-seitige APIs für die standardmäßige und einheitliche Anzeige und Interaktion mit den Editorstatus.

Die Statusleisten jedes Editors sind standardmäßig im Seiten-, Experience Fragment- und Vorlageneditor von AEM verfügbar.

Anwendungsbeispiele für benutzerdefinierte Ressourcenstatusanbieter sind:

* Benachrichtigung für Autorinnen und Autoren, wenn eine Seite innerhalb von 2 Stunden vor einer geplanten Aktivierung liegt
* Benachrichtigung für Autorinnen und Autoren, dass eine Seite innerhalb der letzten 15 Minuten aktiviert wurde
* Benachrichtigung für Autorinnen und Autoren, dass und von wem eine Seite in den letzten 5 Minuten bearbeitet wurde

![Übersicht über den Ressourcenstatus eines AEM-Editors](assets/sample-editor-resource-status-screenshot.png)

## Framework des Ressourcenstatusanbieters {#resource-status-provider-framework}

Bei der Entwicklung benutzerdefinierter Ressourcenstatus besteht die Entwicklungsarbeit aus folgenden Schritten:

1. Der ResourceStatusProvider-Implementierung, die dafür verantwortlich ist, zu bestimmen, ob ein Status erforderlich ist, sowie die grundlegenden Informationen über den Status: Titel, Meldung, Priorität, Variante, Symbol und verfügbare Aktionen.
2. Optional kann GraniteUI-JavaScript verwendet werden, das die Funktionalität aller verfügbaren Aktionen implementiert.

   ![Architektur des Ressourcenstatus](assets/sample-editor-resource-status-application-architecture.png)

3. Der Status-Ressource, die als Teil des Seiten-, Experience Fragment- und Vorlageneditors bereitgestellt wird, wird über die Ressourcen-Eigenschaft „[!DNL statusType]“ ein Typ zugewiesen.

   * Seiteneditor: `editor`
   * Experience Fragment-Editor: `editor`
   * Vorlageneditor: `template-editor`

4. Der `statusType` der Statusressource wird mit dem registrierten `CompositeStatusType` der OSGi-konfigurierten `name`-Eigenschaft abgeglichen.

   Für alle Übereinstimmungen werden die `CompositeStatusType's`-Typen gesammelt und verwendet, um via `ResourceStatusProvider.getType()` die `ResourceStatusProvider`-Implementierungen zu sammeln, die diesen Typ haben.

5. Der passende `ResourceStatusProvider` wird im Editor an die `resource` weitergereicht und bestimmt, ob die `resource` den Status hat, angezeigt zu werden.  Wenn ein Status benötigt wird, ist diese Implementierung für die Erstellung von 0 oder vielen `ResourceStatuses` verantwortlich, die zurückgegeben werden und jeweils einen Status darstellen, der angezeigt werden soll.

   In der Regel gibt ein `ResourceStatusProvider` 0 oder 1 `ResourceStatus` pro `resource` zurück.

6. ResourceStatus ist eine Schnittstelle, die auf Kundenseite implementiert werden kann, oder der hilfreiche `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` kann verwendet werden, um einen Status zu konstruieren.  Ein Status besteht aus:

   * Titel
   * Meldung
   * Symbol
   * Variante
   * Priorität
   * Aktionen
   * Daten

7. Optional, wenn `Actions` für das `ResourceStatus`-Objekt bereitgestellt werden, sind unterstützende Client-Bibliotheken erforderlich, um an die Aktions-Links in der Statusleiste Funktionalitäten zu binden.

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
   * Kategorie des Experience Fragment-Editors: `cq.authoring.editor.sites.page`
   * Kategorie des Vorlageneditors: `cq.authoring.editor.sites.template`

## Anzeigen des Codes {#view-the-code}

[Siehe Code auf GitHub](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## Zusätzliche Ressourcen {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)

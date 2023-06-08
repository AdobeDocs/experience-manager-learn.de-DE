---
title: Integrieren von Adobe Analytics mithilfe des Platform Web SDK
description: Erfahren Sie mehr über den modernen Ansatz zur Integration von Adobe Experience Manager (AEM) und Adobe Analytics mithilfe des Platform Web SDK. Dieses Tutorial führt Sie durch die Erfassung von Seitenansichts- und CTA-Klickdaten, um Dateneinblicke in Adobe Analytics Workspace zu erhalten.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-25T00:00:00Z
jira: KT-13328
thumbnail: KT-13328.jpeg
source-git-commit: 542313c0da6f5eab5befe0da1b80ab38948156ac
workflow-type: tm+mt
source-wordcount: '1647'
ht-degree: 3%

---


# Integrieren von Adobe Analytics mithilfe des Platform Web SDK

Lernen Sie die **moderner Ansatz** Informationen zur Integration von Adobe Experience Manager (AEM) und Adobe Analytics mithilfe des Platform Web SDK. Dieses umfassende Tutorial führt Sie durch den Prozess der nahtlosen Erfassung [WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) Seitenansicht und CTA-Klickdaten. Erhalten Sie wertvolle Einblicke, indem Sie die erfassten Daten in Adobe Analysis Workspace visualisieren, wo Sie verschiedene Metriken und Dimensionen untersuchen können. Erkunden Sie außerdem den Platform-Datensatz, um die Daten zu überprüfen und zu analysieren. Nehmen Sie an dieser Journey teil, um die Macht von AEM und Adobe Analytics für datengestützte Entscheidungen zu nutzen.

## Übersicht

Einblicke in das Benutzerverhalten zu gewinnen, ist ein wichtiges Ziel für jedes Marketing-Team. Indem sie verstehen, wie Benutzer mit ihren Inhalten interagieren, können Teams fundierte Entscheidungen treffen, Strategien optimieren und bessere Ergebnisse erzielen. Das WKND-Marketing-Team, eine fiktive Entität, hat die Implementierung von Adobe Analytics auf ihrer Website ins Auge gefasst, um dieses Ziel zu erreichen. Das Hauptziel besteht darin, Daten zu zwei Schlüsselmetriken zu erfassen: Seitenansichten und CTA-Klicks (homepage call-to-action).

Durch das Tracking von Seitenansichten kann das Team analysieren, welche Seiten von den Benutzern am meisten Aufmerksamkeit erhalten. Außerdem bietet das Tracking von CTA-Klicks auf der Homepage wertvolle Einblicke in die Effektivität der Aktionsaufruf-Elemente des Teams. Diese Daten können Aufschluss darüber geben, welche CTAs bei Benutzern auf Resonanz stoßen, welche angepasst werden müssen, und möglicherweise neue Möglichkeiten zur Verbesserung der Benutzerinteraktion und zur Förderung von Konversionen aufdecken.


>[!VIDEO](https://video.tv.adobe.com/v/3419872?quality=12&learn=on)

## Voraussetzungen

Folgendes ist bei der Integration von Adobe Analytics mit dem Platform Web SDK erforderlich.

Sie haben die Einrichtungsschritte im Abschnitt **[Experience Platform Web SDK integrieren](./web-sdk.md)** Tutorial.

In **AEM als Cloud Service**:

+ [AEM Administratorzugriff auf AEM as a Cloud Service Umgebung](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/accessing/overview.html?lang=de)
+ Zugriff von Deployment Manager auf Cloud Manager
+ Klonen und stellen Sie die [WKND - Adobe Experience Manager-Beispielprojekt](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) in Ihre AEM as a Cloud Service Umgebung.

In **Adobe Analytics**:

+ Zugriff zur Erstellung **Report Suite**
+ Zugriff zur Erstellung **Analysis Workspace**

In **Experience Platform**:

+ Zugriff auf die Standardproduktion, **Prod** Sandbox.
+ Zugriff auf **Schemas** unter Data Management
+ Zugriff auf **Datensätze** unter Data Management
+ Zugriff auf **Datenspeicher** unter Datenerfassung
+ Zugriff auf **Tags** (früher als Launch bezeichnet) unter &quot;Datenerfassung&quot;

Falls Sie nicht über die erforderlichen Berechtigungen verfügen, verwenden Sie Ihr Systemadministrator [Adobe Admin Console](https://adminconsole.adobe.com/) kann die erforderlichen Berechtigungen erteilen.

Bevor wir uns mit dem Integrationsprozess von AEM und Analytics mithilfe des Platform Web SDK befassen, _Zusammenfassen der wesentlichen Komponenten und Schlüsselelemente_ die in [Experience Platform Web SDK integrieren](./web-sdk.md) Tutorial. Es bietet eine solide Grundlage für die Integration.

>[!VIDEO](https://video.tv.adobe.com/v/3419873?quality=12&learn=on)

Nach der Zusammenführung von XDM-Schema, Datastream, Datensatz, Tag-Eigenschaft und AEM- und Tag-Eigenschaftsverbindung beginnen wir mit der Integrations-Journey.

## Dokument zur Definition der Analytics Solution Design Reference (SDR)

Im Rahmen des Implementierungsprozesses wird empfohlen, ein Dokument mit einer Lösungs-Design-Referenz (Solution Design Reference, SDR) zu erstellen. Dieses Dokument spielt eine entscheidende Rolle als Entwurf für die Definition von Geschäftsanforderungen und die Entwicklung effektiver Datenerfassungsstrategien.

Das SDR-Dokument bietet einen umfassenden Überblick über den Implementierungsplan, der sicherstellt, dass alle Interessengruppen abgestimmt sind und die Ziele und den Umfang des Projekts verstehen.


>[!VIDEO](https://video.tv.adobe.com/v/3419874?quality=12&learn=on)

Weitere Informationen zu Konzepten und verschiedenen Elementen, die in das SDR-Dokument aufgenommen werden sollen, finden Sie unter [Erstellen und Verwalten eines SDR-Dokuments (Solution Design Reference)](https://experienceleague.adobe.com/docs/analytics-learn/tutorials/implementation/implementation-basics/creating-and-maintaining-an-sdr.html). Sie können auch eine Excel-Beispielvorlage herunterladen. Es steht jedoch auch eine WKND-spezifische Version zur Verfügung [here](./assets/Initial-WKND-WebSDK-BRD-SDR.xlsx).

## Einrichten von Analytics - Report Suite, Analysis Workspace

Der erste Schritt besteht darin, Adobe Analytics einzurichten, insbesondere die Report Suite mit Konversionsvariablen (oder eVar) und Erfolgsereignissen. Die Konversionsvariablen dienen zur Messung von Ursache und Wirkung. Die Erfolgsereignisse werden zur Verfolgung von Aktionen verwendet.

In diesem Tutorial  `eVar5, eVar6, and eVar7` track  _WKND-Seitenname, WKND-CTA-ID und WKND-CTA-Name_ bzw. `event7` wird verwendet, um  _WKND CTA-Klick-Ereignis_.

Um diese Einblicke aus den erfassten Daten zu analysieren, zu sammeln und mit anderen zu teilen, wird ein Projekt in Analysis Workspace erstellt.

>[!VIDEO](https://video.tv.adobe.com/v/3419875?quality=12&learn=on)

Um mehr über die Einrichtung und Konzepte von Analytics zu erfahren, werden die folgenden Ressourcen dringend empfohlen:

+ [Berichtssuite](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/t-create-a-report-suite.html?lang=de)
+ [Konversionsvariablen](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html)
+ [Erfolgsereignisse](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-events/success-event.html)
+ [Analysis Workspace](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html)

## Aktualisierung des Datenspeichers - Hinzufügen des Analytics-Dienstes

Ein Datastream weist das Platform Edge Network an, wo die erfassten Daten gesendet werden sollen. Im [vorheriges Tutorial](./web-sdk.md), wird ein Datastream konfiguriert, um die Daten an die Experience Platform zu senden. Dieser Datenspeicher wird aktualisiert, um die Daten an die Analytics Report Suite zu senden, die in der [above](#setup-analytics---report-suite-analysis-workspace) Schritt.

>[!VIDEO](https://video.tv.adobe.com/v/3419876?quality=12&learn=on)

## Erstellen eines XDM-Schemas

Mit dem Experience-Datenmodell (XDM)-Schema können Sie die erfassten Daten standardisieren. Im [vorheriges Tutorial](./web-sdk.md), ein XDM-Schema mit `AEP Web SDK ExperienceEvent` eine Feldergruppe erstellt wird. Außerdem wird bei Verwendung dieses XDM-Schemas ein Datensatz erstellt, um die erfassten Daten in der Experience Platform zu speichern.

Dieses XDM-Schema verfügt jedoch nicht über Adobe Analytics-spezifische Feldergruppen, um die eVar- und Ereignisdaten zu senden. Anstatt das vorhandene Schema zu aktualisieren, wird ein neues XDM-Schema erstellt, um die Speicherung der eVar- und Ereignisdaten in der Plattform zu vermeiden.

Das neu erstellte XDM-Schema hat `AEP Web SDK ExperienceEvent` und `Adobe Analytics ExperienceEvent Full Extension` Feldergruppen.

>[!VIDEO](https://video.tv.adobe.com/v/3419877?quality=12&learn=on)


## Tag-Eigenschaft aktualisieren

Im [vorheriges Tutorial](./web-sdk.md), wird eine Tag-Eigenschaft erstellt, sie enthält Datenelemente und eine Regel, um die Seitenansichtsdaten zu erfassen, zuzuordnen und zu senden. Sie muss folgendermaßen verbessert werden:

+ Zuordnen des Seitennamen zu `eVar5`
+ Auslösen der **Seitenansicht** Analytics-Aufruf ( oder Signal senden )
+ Erfassen von CTA-Daten mithilfe der Adobe Client-Datenschicht
+ Zuordnen der CTA-ID und des Namens zu `eVar6` und `eVar7` bzw. Außerdem wird die CTA-Klickanzahl auf `event7`
+ Auslösen der **Link-Klick** Analytics-Aufruf ( oder Signal senden )


>[!VIDEO](https://video.tv.adobe.com/v/3419882?quality=12&learn=on)

>[!TIP]
>
>Das Datenelement und der im Video angezeigte Regel-Ereignis-Code stehen Ihnen als Referenz zur Verfügung. **Erweitern des unten stehenden Accordion-Elements**. Wenn Sie jedoch NICHT die Adobe Client-Datenschicht verwenden, müssen Sie den unten stehenden Code ändern. Es gilt jedoch weiterhin das Konzept der Definition der Datenelemente und ihrer Verwendung in der Regeldefinition.

+++ Datenelement und Regelereigniscode

+ Die `Component ID` Datenelementcode.

  ```javascript
  if(event && event.path && event.path.includes('.')) {    
      // split on the `.` to return just the component ID for e.g. button-06bc532b85, tabs-bb27f4f426-item-cc9c2e6718
      return event.path.split('.')[1];
  }else {
      //return dummy ID
      return "WKND-CTA-ID";
  }
  ```

+ Die `Component Name` Datenelementcode.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // Return the Button, Link, Image, Tab name, for e.g. View Trips, Full Article, See Trips
      return event.component['dc:title'];
  }else {
      //return dummy ID
      return "WKND-CTA-Name";    
  }    
  ```

+ Die `all pages - on load` **Regelbedingung** code

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type') && event.component.hasOwnProperty('xdm:template')) {
      return true;
  }else{
      return false;
  }    
  ```

+ Die `home page - cta click` **Regel-Ereignis** code

  ```javascript
  var componentClickedHandler = function(evt) {
  // defensive coding to avoid a null pointer exception
  if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
      //trigger Tag Rule and pass event
      console.log("cmp:click event: " + evt.eventInfo.path);
  
      var event = {
          //include the path of the component that triggered the event
          path: evt.eventInfo.path,
          //get the state of the component that triggered the event
          component: window.adobeDataLayer.getState(evt.eventInfo.path)
      };
  
      //Trigger the Tag Rule, passing in the new `event` object
      // the `event` obj can now be referenced by the reserved name `event` by other Tag Property data elements
      // i.e `event.component['someKey']`
      trigger(event);
  }
  }
  
  //set the namespace to avoid a potential race condition
  window.adobeDataLayer = window.adobeDataLayer || [];
  //push the event listener for cmp:click into the data layer
  window.adobeDataLayer.push(function (dl) {
  //add event listener for `cmp:click` and callback to the `componentClickedHandler` function
  dl.addEventListener("cmp:click", componentClickedHandler);
  });    
  ```

+ Die `home page - cta click` **Regelbedingung** code

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type')) {
      //Check for Button Type OR Teaser CTA type
      if(event.component['@type'] === 'wknd/components/button' ||
      event.component['@type'] === 'wknd/components/teaser/cta') {
          return true;
      }
  }
  
  // none of the conditions are met, return false
  return false;    
  ```

+++

Weitere Informationen zur Integration AEM Kernkomponenten in die Adobe Client-Datenschicht finden Sie im Abschnitt [Verwenden der Adobe Client-Datenschicht mit AEM Leitfaden zu Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=de).


>[!INFO]
>
>Für ein umfassendes Verständnis der **Variablenzuordnung** Registerkarteneigenschaftsdetails im Dokument Solution Design Reference (SDR) aufrufen, um die abgeschlossene WKND-spezifische Version zum Herunterladen zu öffnen. [here](./assets/Final-WKND-WebSDK-BRD-SDR.xlsx).



## Überprüfen der aktualisierten Tag-Eigenschaft auf WKND

Um sicherzustellen, dass die aktualisierte Tag-Eigenschaft auf den WKND-Site-Seiten erstellt, veröffentlicht und ordnungsgemäß funktioniert. Verwenden des Google Chrome-Webbrowsers [Adobe Experience Platform Debugger-Erweiterung](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob):

+ Um sicherzustellen, dass die Tag-Eigenschaft die neueste Version ist, überprüfen Sie das Build-Datum.

+ Um die XDM-Ereignisdaten für PageView und HomePage CTA Click zu überprüfen, verwenden Sie die Menüoption Experience Platform Web SDK in der Erweiterung.

>[!VIDEO](https://video.tv.adobe.com/v/3419883?quality=12&learn=on)

## Webtraffic simulieren - Selenium-Automatisierung

Um zu Testzwecken einen aussagekräftigen Traffic zu generieren, wird ein Selenium-Automatisierungsskript entwickelt. Dieses benutzerdefinierte Skript simuliert Benutzerinteraktionen mit der WKND-Website, z. B. Seitenansicht und Klicken auf CTAs.

>[!VIDEO](https://video.tv.adobe.com/v/3419884?quality=12&learn=on)

## Datensatzüberprüfung - WKND-Seitenansicht, CTA-Daten

Der Datensatz ist ein Speicher- und Verwaltungskonstrukt für eine Sammlung von Daten wie eine Datenbanktabelle, die einem Schema folgt. Der in der Variablen [vorheriges Tutorial](./web-sdk.md) wird wiederverwendet, um zu überprüfen, ob die Daten für die Seitenansicht und CTA-Klicks in den Experience Platform-Datensatz aufgenommen werden. In der Benutzeroberfläche &quot;Datensatz&quot;werden verschiedene Details wie Datensätze insgesamt, Größe und erfasste Batches zusammen mit einem visuell ansprechenden Balkendiagramm angezeigt.

>[!VIDEO](https://video.tv.adobe.com/v/3419885?quality=12&learn=on)

## Analytics - WKND-Seitenansicht, CTA-Datenvisualisierung

Analysis Workspace ist ein leistungsstarkes Tool in Adobe Analytics, mit dem Daten flexibel und interaktiv analysiert und visualisiert werden können. Es bietet eine Drag &amp; Drop-Benutzeroberfläche, über die Sie benutzerdefinierte Berichte erstellen, erweiterte Segmentierungen durchführen und verschiedene Datenvisualisierungen anwenden können.

Lassen Sie uns das in der [Einrichten von Analytics](#setup-analytics---report-suite-analysis-workspace) Schritt. Im **Top-Seiten** Untersuchen Sie verschiedene Metriken wie Besuche, Unique Visitors, Einstiege, Absprungrate und mehr. Um die Leistung von WKND-Seiten und Startseiten-CTAs zu bewerten, ziehen Sie die WKND-spezifischen Dimensionen (WKND-Seitenname, WKND-CTA-Name) und Metriken (WKND CTA-Klick-Ereignis) per Drag &amp; Drop in den Arbeitsbereich. Diese Einblicke sind für Marketingexperten nützlich, um zu verstehen, welche CTAs effektiver sind, und datengesteuerte Entscheidungen zu treffen, die auf ihre Geschäftsziele abgestimmt sind.

Verwenden Sie die Flussvisualisierung, beginnend mit dem **WKND-Seitenname** und sich in verschiedene Pfade ausdehnen.

>[!VIDEO](https://video.tv.adobe.com/v/3419886?quality=12&learn=on)

## Zusammenfassung

Gute gemacht! Sie haben die Einrichtung von AEM und Adobe Analytics mithilfe des Platform Web SDK abgeschlossen, um die Seitenansicht und CTA-Klickdaten zu erfassen, zu analysieren.

Die Implementierung von Adobe Analytics ist von entscheidender Bedeutung, damit Marketingteams Einblicke in das Benutzerverhalten gewinnen, fundierte Entscheidungen treffen und ihren Inhalt optimieren und datenbasierte Entscheidungen treffen können.

Durch die Implementierung der empfohlenen Schritte und die Verwendung der bereitgestellten Ressourcen, z. B. des Dokuments Solution Design Reference (SDR) und das Verständnis der wichtigsten Analytics-Konzepte, können Marketingexperten Daten effektiv erfassen und analysieren.

>[!VIDEO](https://video.tv.adobe.com/v/3419888?quality=12&learn=on)


>[!AVAILABILITY]
>
>Wenn Sie **End-to-End-Video** , der den gesamten Integrationsprozess anstelle der einzelnen Einrichtungsschritte-Videos abdeckt, können Sie auf [here](https://video.tv.adobe.com/v/3419889/) , um darauf zuzugreifen.


## Zusätzliche Ressourcen

+ [Experience Platform Web SDK integrieren](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform/web-sdk.html)
+ [Verwenden der Adobe Client-Datenschicht in Verbindung mit den Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=de)
+ [Integrieren von Experience Platform-Datenerfassungs-Tags und -AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html)
+ [Übersicht über das Adobe Experience Platform Web SDK und Edge Network](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [Tutorials zur Datenerfassung](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [Übersicht über Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

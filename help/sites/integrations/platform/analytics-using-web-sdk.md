---
title: Integrieren von AEM Sites und Adobe Analytics mit dem Platform Web SDK
description: Integrieren Sie AEM Sites und Adobe Analytics mithilfe des modernen Platform Web SDK-Ansatzes.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-25T00:00:00Z
jira: KT-13328
thumbnail: KT-13328.jpeg
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 0cc3d3bc-e4ea-4ab2-8878-adbcf0c914f5
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '1637'
ht-degree: 100%

---

# Integrieren von AEM Sites und Adobe Analytics mit dem Platform Web SDK

Erfahren Sie mehr über den **modernen Ansatz** zur Integration von Adobe Experience Manager (AEM) und Adobe Analytics mit dem Platform Web SDK. Dieses umfassende Tutorial führt Sie durch den Prozess zur nahtlosen Erfassung von [WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)-Seitenansichten und -CTA-Klickdaten. Erhalten Sie wertvolle Erkenntnisse, indem Sie die erfassten Daten in Adobe Analysis Workspace visualisieren, wo Sie verschiedene Metriken und Dimensionen untersuchen können. Lernen Sie außerdem den Platform-Datensatz kennen, um Daten zu überprüfen und zu analysieren. Begleiten Sie uns auf dieser Journey, um die Leistungskraft von AEM und Adobe Analytics für datenbasierte Entscheidungen zu nutzen.

## Übersicht

Erkenntnisse zum Benutzerverhalten zu erhalten, ist ein wichtiges Ziel für jedes Marketing-Team. Wenn sie verstehen, wie Benutzende mit ihren Inhalten interagieren, können Teams fundierte Entscheidungen treffen, Strategien optimieren und bessere Ergebnisse erzielen. Das WKND-Marketing-Team, eine fiktive Entität, hat die Implementierung von Adobe Analytics auf seiner Website ins Auge gefasst, um dieses Ziel zu erreichen. Das Hauptziel besteht darin, Daten zu zwei Schlüsselmetriken zu erfassen: Seitenansichten und Call-to-Action(CTA)-Klicks auf der Homepage.

Durch das Tracking von Seitenansichten kann das Team analysieren, welche Seiten die meiste Aufmerksamkeit von Benutzenden erhalten. Außerdem bietet das Tracking von CTA-Klicks auf der Homepage wertvolle Erkenntnisse zur Effektivität der CTA-Elemente des Teams. Diese Daten können Aufschluss darüber geben, welche CTAs bei Benutzenden ankommen und welche angepasst werden müssen. Ggf. ergeben sich daraus auch neue Möglichkeiten zur Verbesserung von Benutzerinteraktionen und zur Förderung von Konversionen.


>[!VIDEO](https://video.tv.adobe.com/v/3419872?quality=12&learn=on)

## Voraussetzungen

Folgendes ist bei der Integration von Adobe Analytics mit dem Platform Web SDK erforderlich.

Sie haben die Einrichtungsschritte im Tutorial zum **[Integrieren des Experience Platform Web SDK](./web-sdk.md)** abgeschlossen.

In **AEM as a Cloud Service**:

+ [AEM-Adminzugriff auf die AEM as a Cloud Service-Umgebung](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/accessing/overview.html?lang=de)
+ Zugriff von Bereitstellungs-Manager auf Cloud Manager
+ Klonen und Bereitstellen des [Adobe Experience Manager-WKND-Beispielprojekts](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) in Ihrer AEM as a Cloud Service-Umgebung

In **Adobe Analytics**:

+ Zugriff zur **Report Suite**-Erstellung
+ Zugriff zur **Analysis Workspace**-Erstellung

In **Experience Platform**:

+ Zugriff auf die Standardproduktion, **Prod**-Sandbox
+ Zugriff auf **Schemata** unter Daten-Management
+ Zugriff auf **Datensätze** unter Daten-Management
+ Zugriff auf **Datenströme** unter Datenerfassung
+ Zugriff auf **Tags** (früher als Launch bezeichnet) unter Datenerfassung

Falls Sie nicht über die erforderlichen Berechtigungen verfügen, können Ihre Systemadmins über [Adobe Admin Console](https://adminconsole.adobe.com/) die erforderlichen Berechtigungen erteilen.

Bevor wir uns mit dem Integrationsprozess von AEM und Analytics mit dem Platform Web SDK befassen, _rufen wir uns die wesentlichen Komponenten und Schlüsselelemente ins Gedächtnis_, die im Tutorial zum [Integrieren des Experience Platform Web SDK](./web-sdk.md) vorgestellt wurden. Dies bietet eine solide Grundlage für die Integration.

>[!VIDEO](https://video.tv.adobe.com/v/3419873?quality=12&learn=on)

Nach Wiederholung der Themenbereiche XDM-Schema, Datenstrom, Datensatz, Tag-Eigenschaft und AEM- und Tag-Eigenschaftsverbindung begeben wir uns nun auf den Weg zur Integration.

## Definieren eines Solution Design Reference(SDR)-Analytics-Dokuments

Im Rahmen des Implementierungsprozesses wird empfohlen, ein Solution Design Reference(SDR)-Dokument, also ein Lösungs-Design-Referenzdokument, zu erstellen. Dieses Dokument spielt eine entscheidende Rolle als Blueprint für die Definition von Geschäftsanforderungen und die Konzeption effektiver Datenerfassungsstrategien.

Das SDR-Dokument bietet einen umfassenden Überblick über den Implementierungsplan und stellt sicher, dass alle Verantwortlichen auf Linie sind und die Ziele und den Umfang des Projekts verstehen.


>[!VIDEO](https://video.tv.adobe.com/v/3419874?quality=12&learn=on)

Weitere Informationen zu Konzepten und verschiedenen Elementen, die in das SDR-Dokument aufgenommen werden sollen, finden Sie unter [Erstellen und Verwalten eines Solution Design Reference(SDR)-Dokuments](https://experienceleague.adobe.com/docs/analytics-learn/tutorials/implementation/implementation-basics/creating-and-maintaining-an-sdr.html?lang=de). Sie können auch eine Excel-Beispielvorlage herunterladen. Eine WKND-spezifische Version ist jedoch ebenfalls [hier](./assets/Initial-WKND-WebSDK-BRD-SDR.xlsx) verfügbar.

## Einrichten von Analytics – Report Suite, Analysis Workspace

Der erste Schritt besteht darin, Adobe Analytics einzurichten. Dazu gehört insbesondere eine Report Suite mit Konversionsvariablen (oder eVar) und Erfolgsereignissen. Die Konversionsvariablen dienen zur Messung von Ursache und Wirkung. Die Erfolgsereignisse werden zur Nachverfolgung von Aktionen verwendet.

In diesem Tutorial werden mit `eVar5, eVar6, and eVar7` _der WKND-Seitenname, die WKND-CTA-ID bzw. der WKND-CTA-Name_ und mit `event7` das _WKND-CTA-Klickereignis_ nachverfolgt.

Um eine Analyse durchzuführen, Erkenntnisse aus den erfassten Daten zu gewinnen und diese mit anderen zu teilen, wird ein Projekt in Analysis Workspace erstellt.

>[!VIDEO](https://video.tv.adobe.com/v/3419875?quality=12&learn=on)

Um mehr über die Einrichtung und Konzepte von Analytics zu erfahren, werden die folgenden Ressourcen ausdrücklich empfohlen:

+ [Report Suite](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/t-create-a-report-suite.html?lang=de)
+ [Konversionsvariablen](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html?lang=de)
+ [Erfolgsereignisse](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-events/success-event.html?lang=de)
+ [Analysis Workspace](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html?lang=de)

## Aktualisierung des Datenstroms – Hinzufügen des Analytics-Dienstes

Ein Datenstrom weist das Platform Edge Network an, wohin die erfassten Daten gesendet werden sollen. Im [vorherigen Tutorial](./web-sdk.md) wird ein Datenstrom konfiguriert, um die Daten an Experience Platform zu senden. Dieser Datenstrom wird aktualisiert, um die Daten an die Analytics Report Suite zu senden, die im [obigen](#setup-analytics---report-suite-analysis-workspace) Schritt konfiguriert wurde.

>[!VIDEO](https://video.tv.adobe.com/v/3419876?quality=12&learn=on)

## Erstellen eines XDM-Schemas

Mit dem Experience-Datenmodell(XDM)-Schema können Sie die erfassten Daten standardisieren. Im [vorherigen Tutorial](./web-sdk.md) wird ein XDM-Schema mit `AEP Web SDK ExperienceEvent` als Feldgruppe erstellt. Außerdem wird bei Verwendung dieses XDM-Schemas ein Datensatz erstellt, um die erfassten Daten in Experience Platform zu speichern.

Dieses XDM-Schema verfügt jedoch nicht über Adobe Analytics-spezifische Feldgruppen, um eVar- und Ereignisdaten zu senden. Anstatt das vorhandene Schema zu aktualisieren, wird ein neues XDM-Schema erstellt, um eine eVar- und Ereignisdatenspeicherung in der Plattform zu vermeiden.

Das neu erstellte XDM-Schema verfügt über die Feldgruppen `AEP Web SDK ExperienceEvent` und `Adobe Analytics ExperienceEvent Full Extension`.

>[!VIDEO](https://video.tv.adobe.com/v/3419877?quality=12&learn=on)


## Aktualisieren der Tag-Eigenschaft

Im [vorherigen Tutorial](./web-sdk.md) wird eine Tag-Eigenschaft erstellt. Sie enthält Datenelemente und eine Regel, um Seitenansichtsdaten zu erfassen, zuzuordnen und zu senden. Sie muss für folgende Vorgänge erweitert werden:

+ Zuordnen des Seitennamens zu `eVar5`
+ Auslösen des **Seitenansichts**-Analytics-Aufruf (oder „Beacon senden“)
+ Erfassen von CTA-Daten mithilfe der Adobe Client-Datenschicht
+ Zuordnen der CTA-ID und des Namens zu `eVar6` bzw. `eVar7` und der CTA-Klickanzahl zu `event7`
+ Auslösen der **Link-Klick**- Analytics-Aufrufs (oder „Beacon senden“)


>[!VIDEO](https://video.tv.adobe.com/v/3419882?quality=12&learn=on)

>[!TIP]
>
>Der im Video dargestellte Code für Datenelemente und Regelereignisse ist als Referenz verfügbar. **Erweitern Sie das unten stehende Akkordeon-Element**. Wenn Sie jedoch NICHT die Adobe Client-Datenschicht verwenden, müssen Sie den unten stehenden Code ändern. Es gilt aber weiterhin das Konzept der Definition der Datenelemente und ihrer Verwendung in der Regeldefinition. 

+++ Code für Datenelemente und Regelereignisse

+ Der Code für das Datenelement `Component ID`:

  ```javascript
  if(event && event.path && event.path.includes('.')) {    
      // split on the `.` to return just the component ID for e.g. button-06bc532b85, tabs-bb27f4f426-item-cc9c2e6718
      return event.path.split('.')[1];
  }else {
      //return dummy ID
      return "WKND-CTA-ID";
  }
  ```

+ Der Code für das Datenelement `Component Name`:

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // Return the Button, Link, Image, Tab name, for e.g. View Trips, Full Article, See Trips
      return event.component['dc:title'];
  }else {
      //return dummy ID
      return "WKND-CTA-Name";    
  }    
  ```

+ Der Code für die `all pages - on load`**-Regelbedingung**:

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type') && event.component.hasOwnProperty('xdm:template')) {
      return true;
  }else{
      return false;
  }    
  ```

+ Der Code für das `home page - cta click`**-Regelereignis**:

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

+ Der Code für die `home page - cta click`**-Regelbedingung**:

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

Weitere Informationen zur Integration von AEM-Kernkomponenten in die Adobe Client-Datenschicht finden Sie im [Handbuch zum Verwenden der Adobe Client-Datenschicht mit AEM-Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=de).


>[!INFO]
>
>Um sich ein umfassendes Verständnis der **Variablenzuordnung**-Registerkarteneigenschaftsdetails im Solution Design Reference(SDR)-Dokument anzueignen, können Sie die fertiggestellte WKND-spezifische Version zum Herunterladen [hier](./assets/Final-WKND-WebSDK-BRD-SDR.xlsx) öffnen.



## Überprüfen der aktualisierten Tag-Eigenschaft auf WKND

Um eine ordnungsgemäße Erstellung, Veröffentlichung und Funktionsweise der aktualisierten Tag-Eigenschaft auf den WKND-Site-Seiten sicherzustellen, verwenden Sie die [Adobe Experience Platform Debugger-Erweiterung](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) des Google Chrome-Webbrowsers:

+ Um sicherzustellen, dass die Tag-Eigenschaft der neuesten Version entspricht, überprüfen Sie das Build-Datum.

+ Um die XDM-Ereignisdaten für Seitenansichten und CTA-Klicks auf der Homepage zu überprüfen, verwenden Sie die Experience Platform Web SDK-Menüoption in der Erweiterung.

>[!VIDEO](https://video.tv.adobe.com/v/3419883?quality=12&learn=on)

## Simulieren von Webtraffic – Selenium-Automatisierung

Um zu Testzwecken genügend Traffic zu generieren, wird ein Selenium-Automatisierungsskript entwickelt. Dieses benutzerdefinierte Skript simuliert Benutzerinteraktionen mit der WKND-Website, z. B. Seitenansichten und CTA-Klicks.

>[!VIDEO](https://video.tv.adobe.com/v/3419884?quality=12&learn=on)

## Datensatzüberprüfung – Seitenansichts- und CTA-Daten der WKND-Site

Der Datensatz ist ein Speicher- und Verwaltungskonstrukt für eine Sammlung von Daten wie eine Datenbanktabelle, die einem Schema folgt. Der im [vorherigen Tutorial](./web-sdk.md) erstellte Datensatz wird wiederverwendet, um zu überprüfen, ob die Seitenansichts- und CTA-Klickdaten in den Experience Platform-Datensatz aufgenommen werden. In der Datensatzbenutzeroberfläche werden verschiedene Details wie die Gesamtdatensätze, die Größe und die erfassten Batches zusammen mit einem visuell ansprechenden Balkendiagramm angezeigt.

>[!VIDEO](https://video.tv.adobe.com/v/3419885?quality=12&learn=on)

## Analytics – Visualisierung von Seitenansichts- und CTA-Daten der WKND-Site

Analysis Workspace ist ein leistungsstarkes Tool in Adobe Analytics, mit dem Daten flexibel und interaktiv analysiert und visualisiert werden können. Es bietet eine Drag &amp; Drop-Benutzeroberfläche, über die Sie benutzerdefinierte Berichte erstellen, erweiterte Segmentierungen durchführen und verschiedene Datenvisualisierungen anwenden können.

Öffnen wir das im Schritt [Analytics-Einrichtung](#setup-analytics---report-suite-analysis-workspace) erstellte Analysis Workspace-Projekt. Untersuchen Sie im Abschnitt mit den **angesagtesten Seiten** verschiedene Metriken wie Besuche, Unique Visitors, Einstiege, Absprungrate usw. Um die Leistung von WKND-Seiten und CTAs auf der Homepage zu bewerten, ziehen Sie die WKND-spezifischen Dimensionen (WKND-Seitenname, WKND-CTA-Name) und Metriken (WKND-CTA-Klickereignis) in den Arbeitsbereich. Anhand dieser Erkenntnisse können Marketing-Fachleute nachvollziehen, welche CTAs effektiver sind, und datenbasierte, auf ihre Geschäftsziele abgestimmte Entscheidungen treffen.

Verwenden Sie zur Visualisierung von Benutzer-Journeys die Flussvisualisierung. Beginnen Sie mit dem **WKND-Seitennamen** und machen Sie dann mit den verschiedenen Pfaden weiter.

>[!VIDEO](https://video.tv.adobe.com/v/3419886?quality=12&learn=on)

## Zusammenfassung

Gut gemacht! Sie haben die Einrichtung von AEM und Adobe Analytics mit dem Platform Web SDK abgeschlossen, um Seitenansichts- und CTA-Klickdaten zu erfassen und zu analysieren.

Die Implementierung von Adobe Analytics ist von entscheidender Bedeutung, damit Marketing-Teams Erkenntnisse zum Benutzerverhalten gewinnen, fundiert entscheiden und ihre Inhalte optimieren und datenbasierte Entscheidungen treffen können.

Durch Umsetzen der empfohlenen Schritte, Verwenden der bereitgestellten Ressourcen, darunter das Solution Design Reference(SDR)-Dokument, und Verstehen der wichtigsten Analytics-Konzepte können Marketing-Fachleute Daten effektiv erfassen und analysieren.

>[!VIDEO](https://video.tv.adobe.com/v/3419888?quality=12&learn=on)


>[!AVAILABILITY]
>
>Wenn Sie ein **End-to-End-Video** bevorzugen, das den gesamten Integrationsprozess anstelle der einzelnen Einrichtungsschritte-Videos abdeckt, können Sie [hier](https://video.tv.adobe.com/v/3419889/) klicken, um darauf zuzugreifen.


## Zusätzliche Ressourcen

+ [Integrieren des Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform/web-sdk.html?lang=de)
+ [Verwenden der Adobe Client-Datenschicht in Verbindung mit den Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=de)
+ [Integrieren von Experience Platform-Datenerfassungs-Tags in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html?lang=de)
+ [Überblick über Adobe Experience Platform Web SDK und Edge Network](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html?lang=de)
+ [Tutorials zur Datenerfassung](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html?lang=de)
+ [Überblick über Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=de)

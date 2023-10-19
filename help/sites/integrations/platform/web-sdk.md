---
title: Integrieren von AEM Sites und Experience Platform Web SDK
description: Erfahren Sie, wie Sie AEM Sites as a Cloud Service mit dem Experience Platform Web SDK integrieren. Dieser grundlegende Schritt ist für die Integration von Adobe Experience Cloud-Produkten wie Adobe Analytics, Target oder neueren innovativen Produkten wie Real-time Customer Data Platform, Customer Journey Analytics und Journey Optimizer von entscheidender Bedeutung.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-04-26T00:00:00Z
jira: KT-13156
thumbnail: KT-13156.jpeg
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service" before-title="false"
exl-id: 47df99e6-6418-43c8-96fe-85e3c47034d6
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '1354'
ht-degree: 100%

---

# Integrieren von AEM Sites und Experience Platform Web SDK

Erfahren Sie, wie Sie AEM as a Cloud Service mit dem Experience Platform [Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=de) integrieren. Dieser grundlegende Schritt ist für die Integration von Adobe Experience Cloud-Produkten wie Adobe Analytics, Target oder neueren innovativen Produkten wie Real-time Customer Data Platform, Customer Journey Analytics und Journey Optimizer von entscheidender Bedeutung.

Sie lernen auch, wie Sie Seitenaufrufdaten für das [WKND – Beispielprojekt für Adobe Experience Manager](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) in [Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html?lang=de) sammeln und senden.

Nach Abschluss dieser Einrichtung haben Sie eine solide Grundlage implementiert. Außerdem sind Sie bereit, die Implementierung von Experience Platform mit Anwendungen wie [Real-Time Customer Data Platform (Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html?lang=de), [Customer Journey Analytics (CJA)](https://experienceleague.adobe.com/docs/customer-journey-analytics.html?lang=de), und [Adobe Journey Optimizer (AJO)](https://experienceleague.adobe.com/docs/journey-optimizer.html?lang=de) voranzutreiben. Die erweiterte Implementierung trägt durch die Standardisierung der Web- und Kundendaten zu einer besseren Interaktion mit der Kundschaft bei.

## Voraussetzungen

Bei der Integration des Experience Platform Web SDK sind folgende Elemente erforderlich.

In **AEM as Cloud Service**:

+ AEM-Adminzugriff auf AEM as a Cloud Service-Umgebungen
+ Zugriff von Bereitstellungs-Manager auf Cloud Manager
+ Klonen Sie das [WKND – Adobe Experience Manager-Beispielprojekt](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) und stellen Sie es in Ihre AEM as a Cloud Service-Umgebung bereit.

In **Experience Platform**:

+ Zugriff auf die Standardproduktion, **Prod**-Sandbox
+ Zugriff auf **Schemata** unter Daten-Management
+ Zugriff auf **Datensätze** unter Daten-Management
+ Zugriff auf **Datenströme** unter Datenerfassung
+ Zugriff auf **Tags** (früher als Launch bezeichnet) unter Datenerfassung

Falls Sie nicht über die erforderlichen Berechtigungen verfügen, können Ihre Systemadmins über [Adobe Admin Console](https://adminconsole.adobe.com/) die erforderlichen Berechtigungen erteilen.

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## Erstellen eines XDM-Schemas – Experience Platform

Mit dem Experience-Datenmodell(XDM)-Schema können Sie die Kundenerlebnisdaten standardisieren. Um die **WKND-Seitenaufrufe** zu erfassen, erstellen Sie ein XDM-Schema und verwenden die von Adobe bereitgestellten Feldergruppen `AEP Web SDK ExperienceEvent` für die Web-Datenerfassung.

Es gibt allgemeine und branchenspezifische Datenmodelle, z. B. für den Einzelhandel, Finanzdienstleistungen, das Gesundheitswesen usw. Weitere Informationen finden Sie in der [Übersicht über Branchendatenmodelle](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/industries/overview.html?lang=de).


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

Erfahren Sie mehr über das XDM-Schema und verwandte Konzepte wie Feldergruppen, Typen, Klassen und Datentypen aus der [XDM-Systemübersicht](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html?lang=de).

Die [XDM-Systemübersicht](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html?lang=de) ist eine großartige Ressource, um mehr über das XDM-Schema und verwandte Konzepte wie Feldergruppen, Typen, Klassen und Datentypen zu erfahren. Sie bietet ein umfassendes Verständnis des XDM-Datenmodells und davon, wie XDM-Schemata erstellt und verwaltet werden, um Daten im gesamten Unternehmen zu standardisieren. Lernen Sie das XDM-Schema besser kennen und erfahren Sie, wie es Ihre Datenerfassungs- und -verwaltungsprozesse unterstützen kann.

## Erstellen eines Datenstroms – Experience Platform

Ein Datenstrom weist das Platform Edge Network an, wohin die erfassten Daten gesendet werden sollen. Sie können beispielsweise an Experience Platform, Analytics oder Adobe Target gesendet werden.


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

Machen Sie sich mit dem Konzept von Datenströmen und verwandten Themen wie Data Governance und Konfiguration vertraut, indem Sie die Seite [Datenspeicher-Übersicht](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html?lang=de) besuchen.

## Erstellen einer Tag-Eigenschaft – Experience Platform

Erfahren Sie, wie Sie in Experience Platform eine Tag-Eigenschaft (ehemals Launch) erstellen, um die JavaScript-Bibliothek des Web SDK zur WKND-Website hinzuzufügen. Die neu definierte Tag-Eigenschaft verfügt über die folgenden Ressourcen:

+ Tag-Erweiterungen: [Core](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) und [Adobe Experience Platform Web SDK](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ Datenelemente: Die Datenelemente des benutzerdefinierten Code-Typs, die den Seitennamen, den Site-Abschnitt und den Hostnamen mithilfe der Datenschicht der Adobe Client-WKND-Site extrahieren. Auch das Datenelement vom XDM-Objekttyp, das mit dem neu erstellten WKND-XDM-Schema übereinstimmt, wurde in einem früheren Schritt [Erstellen eines XDM-Schemas](#create-xdm-schema---experience-platform) erstellt.
+ Regel: Senden von Daten an das Platform Edge Network bei jedem Besuch einer WKND-Web-Seite unter Verwendung des von der Adobe Client-Datenschicht ausgelösten Ereignisses `cmp:show`.

Beim Erstellen und Veröffentlichen der Tag-Bibliothek mit dem **Veröffentlichungsfluss** können Sie die Schaltfläche **Alle geänderten Ressourcen hinzufügen** verwenden. So wählen Sie alle Ressourcen wie Datenelemente, Regeln und Tag-Erweiterungen aus, anstatt eine einzelne Ressource zu identifizieren und auszuwählen. Während der Entwicklungsphase können Sie die Bibliothek auch nur in der _Entwicklungsumgebung_ veröffentlichen und sie dann verifizieren und in die _Staging_- oder _Produktionsumgebung_ übertragen.

>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)


>[!TIP]
>
>Der im Video dargestellte Code für Datenelemente und Regelereignisse ist als Referenz verfügbar. **Erweitern Sie das unten stehende Akkordeon-Element**. Wenn Sie jedoch NICHT die Adobe Client-Datenschicht verwenden, müssen Sie den unten stehenden Code ändern. Es gilt aber weiterhin das Konzept der Definition der Datenelemente und ihrer Verwendung in der Regeldefinition. 


+++ Code für Datenelemente und Regelereignisse

+ Der Code für das Datenelement `Page Name`:

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // return value of 'dc:title' from the data layer Page object, which is propogated via 'cmp:show` event
      return event.component['dc:title'];
  }
  ```

+ Der Code für Datenelemente `Site Section`.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('repo:path')) {
  let pagePath = event.component['repo:path'];
  
  let siteSection = '';
  
  //Check of html String in URL.
  if (pagePath.indexOf('.html') > -1) { 
   siteSection = pagePath.substring(0, pagePath.lastIndexOf('.html'));
  
   //replace slash with colon
   siteSection = siteSection.replaceAll('/', ':');
  
   //remove `:content`
   siteSection = siteSection.replaceAll(':content:','');
  }
  
      return siteSection 
  }
  ```

+ Der Code für Datenelemente `Host Name`.

  ```javascript
  if(window && window.location && window.location.hostname) {
      return window.location.hostname;
  }
  ```

+ Der Code für Regelereignisse `all pages - on load`.

  ```javascript
  var pageShownEventHandler = function(evt) {
  // defensive coding to avoid a null pointer exception
  if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
      //trigger Launch Rule and pass event
      console.debug("cmp:show event: " + evt.eventInfo.path);
      var event = {
          //include the path of the component that triggered the event
          path: evt.eventInfo.path,
          //get the state of the component that triggered the event
          component: window.adobeDataLayer.getState(evt.eventInfo.path)
      };
  
      //Trigger the Launch Rule, passing in the new 'event' object
      // the 'event' obj can now be referenced by the reserved name 'event' by other Launch data elements
      // i.e 'event.component['someKey']'
      trigger(event);
      }
  }
  
  //set the namespace to avoid a potential race condition
  window.adobeDataLayer = window.adobeDataLayer || [];
  
  //push the event listener for cmp:show into the data layer
  window.adobeDataLayer.push(function (dl) {
      //add event listener for 'cmp:show' and callback to the 'pageShownEventHandler' function
      dl.addEventListener("cmp:show", pageShownEventHandler);
  });
  ```

+++


Die [Tags-Übersicht](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=de) bietet umfassende Kenntnisse zu wichtigen Konzepten wie Datenelementen, Regeln und Erweiterungen.

Weitere Informationen zur Integration von AEM-Kernkomponenten in die Adobe Client-Datenschicht finden Sie im [Handbuch zum Verwenden der Adobe Client-Datenschicht mit AEM-Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=de).

## Verbinden der Tag-Eigenschaft mit AEM

Erfahren Sie, wie Sie die kürzlich erstellte Tag-Eigenschaft über die Adobe IMS- und Adobe Launch-Konfiguration in AEM mit AEM verknüpfen. Wenn eine AEM as a Cloud Service-Umgebung eingerichtet ist, werden mehrere Konfigurationen des technischen Adobe IMS-Kontos automatisch generiert, einschließlich Adobe Launch. Für AEM Version 6.5 müssen Sie jedoch eine manuell konfigurieren.

Nach Verknüpfung der Tag-Eigenschaft kann die WKND-Site die JavaScript-Bibliothek der Tag-Eigenschaft mithilfe der Cloud Service-Konfiguration von Adobe Launch auf die Web-Seiten laden.

### Überprüfen des Ladens von Tag-Eigenschaften auf WKND

Prüfen Sie mit der Erweiterung Adobe Experience Platform Debugger in [Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) oder [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/), ob die Tag-Eigenschaft auf WKND-Seiten geladen wird. Sie können Folgendes überprüfen:

+ Tag-Eigenschaften wie Erweiterung, Version, Name und mehr.
+ Platform Web SDK-Bibliotheksversion, Datenspeicher-ID
+ XDM-Objekt als Teil des `events`-Attributs im Experience Platform Web SDK

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## Erstellen von Datensätzen – Experience Platform

Die mit dem Web SDK erfassten Seitenansichtsdaten werden im Experience Platform Data Lake als Datensätze gespeichert. Der Datensatz ist ein Speicher- und Verwaltungskonstrukt für eine Sammlung von Daten wie eine Datenbanktabelle, die einem Schema folgt. Erfahren Sie, wie Sie einen Datensatz erstellen und den zuvor erstellten Datenspeicher so konfigurieren, dass Daten an Experience Platform gesendet werden.


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

Die [Datensatz-Übersicht](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html?lang=de) bietet weitere Informationen zu Konzepten, Konfigurationen und anderen Aufnahmefunktionen.


## WKND-Seitenansichtsdaten in Experience Platform

Nach der Einrichtung des Web SDK mit AEM, insbesondere auf der WKND-Site, ist es an der Zeit, Traffic durch die Site-Seiten zu generieren. Bestätigen Sie anschließend, dass die Seitenaufrufdaten in den Experience Platform-Datensatz aufgenommen werden. In der Datensatzbenutzeroberfläche werden verschiedene Details wie die Gesamtdatensätze, die Größe und die erfassten Batches zusammen mit einem visuell ansprechenden Balkendiagramm angezeigt.

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## Zusammenfassung

Gut gemacht! Sie haben die Einrichtung von AEM mit dem Experience Platform Web SDK abgeschlossen, um Daten von einer Website zu erfassen und aufzunehmen. Auf dieser Grundlage können Sie nun weitere Möglichkeiten zur Verbesserung und Integration von Produkten wie Analytics, Target, Customer Journey Analytics (CJA) und vielen anderen erkunden, um umfangreiche, personalisierte Erlebnisse für Ihre Kundschaft zu erstellen. Lernen und forschen Sie weiter, um das gesamte Potenzial von Adobe Experience Cloud auszuschöpfen.

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)


>[!AVAILABILITY]
>
>Wenn Sie ein **End-to-End-Video** bevorzugen, das den gesamten Integrationsprozess anstelle der einzelnen Einrichtungsschritte-Videos abdeckt, können Sie [hier](https://video.tv.adobe.com/v/3418905/) klicken, um darauf zuzugreifen.

## Zusätzliche Ressourcen

+ [Verwenden der Adobe Client-Datenschicht in Verbindung mit den Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=de)
+ [Integrieren von Experience Platform-Datenerfassungs-Tags in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html?lang=de)
+ [Überblick über Adobe Experience Platform Web SDK und Edge Network](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html?lang=de)
+ [Tutorials zur Datenerfassung](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html?lang=de)
+ [Überblick über Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=de)

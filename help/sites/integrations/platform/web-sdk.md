---
title: Integrieren von AEM Sites und Experience Platform Web SDK
description: Erfahren Sie, wie Sie AEM Sites as a Cloud Service mit dem Experience Platform Web SDK integrieren. Dieser grundlegende Schritt ist für die Integration von Adobe Experience Cloud-Produkten wie Adobe Analytics, Target oder neueren innovativen Produkten wie Real-time Customer Data Platform, Customer Journey Analytics und Journey Optimizer unerlässlich.
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
ht-degree: 6%

---

# Integrieren von AEM Sites und Experience Platform Web SDK

Erfahren Sie, wie Sie AEM as a Cloud Service mit Experience Platform integrieren. [Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). Dieser grundlegende Schritt ist für die Integration von Adobe Experience Cloud-Produkten wie Adobe Analytics, Target oder neueren innovativen Produkten wie Real-time Customer Data Platform, Customer Journey Analytics und Journey Optimizer unerlässlich.

Außerdem erfahren Sie, wie Sie [WKND - Adobe Experience Manager-Beispielprojekt](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) Seitenansichtsdaten im [Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html?lang=de).

Nach Abschluss dieser Einrichtung haben Sie eine solide Grundlage implementiert. Außerdem können Sie die Experience Platform-Implementierung mithilfe von Anwendungen wie [Real-time Customer Data Platform (Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html), [Customer Journey Analytics (CJA)](https://experienceleague.adobe.com/docs/customer-journey-analytics.html), und [Adobe Journey Optimizer (AJO)](https://experienceleague.adobe.com/docs/journey-optimizer.html?lang=de). Die erweiterte Implementierung trägt durch Standardisierung der Web- und Kundendaten zu einer besseren Kundeninteraktion bei.

## Voraussetzungen

Bei der Integration des Experience Platform Web SDK ist Folgendes erforderlich.

In **AEM als Cloud Service**:

+ AEM Administratorzugriff auf AEM as a Cloud Service Umgebung
+ Zugriff von Deployment Manager auf Cloud Manager
+ Klonen und stellen Sie die [WKND - Adobe Experience Manager-Beispielprojekt](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) in Ihre AEM as a Cloud Service Umgebung.

In **Experience Platform**:

+ Zugriff auf die Standardproduktion, **Prod** Sandbox.
+ Zugriff auf **Schemas** unter Data Management
+ Zugriff auf **Datensätze** unter Data Management
+ Zugriff auf **Datenspeicher** unter Datenerfassung
+ Zugriff auf **Tags** (früher als Launch bezeichnet) unter &quot;Datenerfassung&quot;

Falls Sie nicht über die erforderlichen Berechtigungen verfügen, verwenden Sie Ihr Systemadministrator [Adobe Admin Console](https://adminconsole.adobe.com/) kann die erforderlichen Berechtigungen erteilen.

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## Erstellen eines XDM-Schemas - Experience Platform

Mit dem Experience-Datenmodell (XDM)-Schema können Sie die Kundenerlebnisdaten standardisieren. So sammeln Sie die **WKND-Seitenansicht** Daten, erstellen Sie ein XDM-Schema und verwenden Sie die Adobe bereitgestellten Feldergruppen `AEP Web SDK ExperienceEvent` für die Webdatenerfassung.

Es gibt generische und branchenspezifische Beispiele für Einzelhandel, Finanzdienstleistungen, Gesundheitswesen und mehr, eine Reihe von Referenzdatenmodellen, siehe [Übersicht über die Datenmodelle in der Branche](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/industries/overview.html) für weitere Informationen.


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

Erfahren Sie mehr über das XDM-Schema und verwandte Konzepte wie Feldgruppen, Typen, Klassen und Datentypen aus [XDM-System - Übersicht](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html).

Die [XDM-System - Übersicht](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html) ist eine großartige Ressource, um mehr über das XDM-Schema und verwandte Konzepte wie Feldergruppen, Typen, Klassen und Datentypen zu erfahren. Es bietet ein umfassendes Verständnis des XDM-Datenmodells und wie XDM-Schemas erstellt und verwaltet werden, um Daten im gesamten Unternehmen zu standardisieren. Erfahren Sie mehr darüber, wie Sie das XDM-Schema besser verstehen und wie es Ihre Datenerfassungs- und -verwaltungsprozesse nutzen kann.

## Erstellen von Datastream - Experience Platform

Ein Datastream weist das Platform Edge Network an, wo die erfassten Daten gesendet werden sollen. Sie kann beispielsweise an Experience Platform, Analytics oder Adobe Target gesendet werden.


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

Machen Sie sich mit dem Konzept von Datastreams und verwandten Themen wie Data Governance und Konfiguration vertraut, indem Sie die [Übersicht über Datenspeicher](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html?lang=de) Seite.

## Tag-Eigenschaft erstellen - Experience Platform

Erfahren Sie, wie Sie eine Tag-Eigenschaft (ehemals Launch) in Experience Platform erstellen, um die JavaScript-Bibliothek des Web SDK zur WKND-Website hinzuzufügen. Die neu definierte Tag-Eigenschaft verfügt über die folgenden Ressourcen:

+ Tag-Erweiterungen: [Core](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) und [Adobe Experience Platform Web SDK](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ Datenelemente: Die Datenelemente des benutzerdefinierten Codetyps, die den Seitennamen, den Site-Abschnitt und den Hostnamen mithilfe der Adobe Client-Datenschicht der WKND-Site extrahieren. Außerdem das Datenelement vom Typ XDM-Objekt , das dem zuvor neu erstellten WKND-XDM-Schema-Build-in entspricht [Erstellen eines XDM-Schemas](#create-xdm-schema---experience-platform) Schritt.
+ Regel: Senden Sie Daten an Platform Edge Network, sobald eine WKND-Webseite mithilfe der Adobe Client-Datenschicht aufgerufen wird. `cmp:show` -Ereignis.

Beim Erstellen und Veröffentlichen der Tag-Bibliothek mithilfe des **Veröffentlichungsfluss**, können Sie die **Alle geänderten Ressourcen hinzufügen** Schaltfläche. So wählen Sie alle Ressourcen wie Datenelemente, Regeln und Tag-Erweiterungen aus, anstatt eine einzelne Ressource zu identifizieren und auszuwählen. Während der Entwicklungsphase können Sie die Bibliothek auch nur in der _Entwicklung_ Umgebung, dann überprüfen und an die _Staging_ oder _Produktion_ Umgebung.

>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)


>[!TIP]
>
>Das Datenelement und der im Video angezeigte Regel-Ereignis-Code stehen Ihnen als Referenz zur Verfügung. **Erweitern des unten stehenden Accordion-Elements**. Wenn Sie jedoch NICHT die Adobe Client-Datenschicht verwenden, müssen Sie den unten stehenden Code ändern. Es gilt jedoch weiterhin das Konzept der Definition der Datenelemente und ihrer Verwendung in der Regeldefinition.


+++ Datenelement und Regelereigniscode

+ Die `Page Name` Datenelementcode.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // return value of 'dc:title' from the data layer Page object, which is propogated via 'cmp:show` event
      return event.component['dc:title'];
  }
  ```

+ Die `Site Section` Datenelementcode.

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

+ Die `Host Name` Datenelementcode.

  ```javascript
  if(window && window.location && window.location.hostname) {
      return window.location.hostname;
  }
  ```

+ Die `all pages - on load` Regel-Ereignis-Code

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


Die [Übersicht über Tags](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=de) bietet umfassende Kenntnisse zu wichtigen Konzepten wie Datenelementen, Regeln und Erweiterungen.

Weitere Informationen zur Integration AEM Kernkomponenten in die Adobe Client-Datenschicht finden Sie im Abschnitt [Verwenden der Adobe Client-Datenschicht mit AEM Leitfaden zu Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=de).

## Tag-Eigenschaft mit AEM verbinden

Erfahren Sie, wie Sie die kürzlich erstellte Tag-Eigenschaft über die Adobe IMS- und Adobe Launch-Konfiguration in AEM mit AEM verknüpfen. Wenn eine AEM as a Cloud Service Umgebung eingerichtet ist, werden automatisch mehrere Konfigurationen des technischen Adobe IMS-Kontos generiert, einschließlich Adobe Launch. Für AEM Version 6.5 müssen Sie jedoch eine manuell konfigurieren.

Nach Verknüpfung der Tag-Eigenschaft kann die WKND-Site die JavaScript-Bibliothek der Tag-Eigenschaft mithilfe der Cloud Service-Konfiguration von Adobe Launch auf die Webseiten laden.

### Laden der Tag-Eigenschaft auf WKND überprüfen

Verwenden von Adobe Experience Platform Debugger [Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) oder [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) -Erweiterung überprüfen, ob die Tag-Eigenschaft auf WKND-Seiten geladen wird. Sie können überprüfen,

+ Tag-Eigenschaftendetails wie Erweiterung, Version, Name und mehr.
+ Platform Web SDK-Bibliotheksversion, Datenspeicher-ID
+ XDM-Objekt als Teil `events` -Attribut im Experience Platform Web SDK

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## Datensatz erstellen - Experience Platform

Die mit dem Web SDK erfassten Seitenansichtsdaten werden im Experience Platform Data Lake als Datensätze gespeichert. Der Datensatz ist ein Speicher- und Verwaltungskonstrukt für eine Sammlung von Daten wie eine Datenbanktabelle, die einem Schema folgt. Erfahren Sie, wie Sie einen Datensatz erstellen und den zuvor erstellten Datenspeicher so konfigurieren, dass Daten an die Experience Platform gesendet werden.


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

Die [Datensätze - Übersicht](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html?lang=de) bietet weitere Informationen zu Konzepten, Konfigurationen und anderen Erfassungsfunktionen.


## WKND-Seitenansichtsdaten in Experience Platform

Nach der Einrichtung des Web SDK mit AEM, insbesondere auf der WKND-Site, ist es an der Zeit, Traffic durch die Site-Seiten zu generieren. Bestätigen Sie dann, dass die Seitenansichtsdaten in den Experience Platform-Datensatz aufgenommen werden. In der Datensatzbenutzeroberfläche werden verschiedene Details wie die Gesamtdatensätze, die Größe und die erfassten Batches zusammen mit einem visuell ansprechenden Balkendiagramm angezeigt.

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## Zusammenfassung

Gute gemacht! Sie haben die Einrichtung von AEM mit dem Experience Platform Web SDK abgeschlossen, um Daten von einer Website zu erfassen und zu erfassen. Auf dieser Grundlage können Sie nun weitere Möglichkeiten zur Verbesserung und Integration von Produkten wie Analytics, Target, Customer Journey Analytics (CJA) und vielen anderen erkunden, um umfangreiche, personalisierte Erlebnisse für Ihre Kunden zu erstellen. Lernen Sie weiter und lernen Sie, um das gesamte Potenzial von Adobe Experience Cloud auszuschöpfen.

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)


>[!AVAILABILITY]
>
>Wenn Sie **End-to-End-Video** , der den gesamten Integrationsprozess anstelle der einzelnen Einrichtungsschritte-Videos abdeckt, können Sie auf [here](https://video.tv.adobe.com/v/3418905/) , um darauf zuzugreifen.

## Zusätzliche Ressourcen

+ [Verwenden der Adobe Client-Datenschicht in Verbindung mit den Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=de)
+ [Integrieren von Experience Platform-Datenerfassungs-Tags und -AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html?lang=de)
+ [Übersicht über das Adobe Experience Platform Web SDK und Edge Network](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [Tutorials zur Datenerfassung](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [Übersicht über Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

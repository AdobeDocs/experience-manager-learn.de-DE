---
title: Experience Platform Web SDK integrieren
description: Erfahren Sie, wie Sie AEM as a Cloud Service mit dem Experience Platform Web SDK integrieren. Dieser grundlegende Schritt ist für die Integration von Adobe Experience Cloud-Produkten wie Adobe Analytics, Target oder neueren innovativen Produkten wie Real-time Customer Data Platform, Customer Journey Analytics und Journey Optimizer von entscheidender Bedeutung.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-04-26T00:00:00Z
jira: KT-13156
thumbnail: KT-13156.jpeg
source-git-commit: 593ef5767a5f2321c689e391f9c9019de7c94672
workflow-type: tm+mt
source-wordcount: '1060'
ht-degree: 4%

---


# Experience Platform Web SDK integrieren

Erfahren Sie, wie Sie AEM as a Cloud Service mit Experience Platform integrieren. [Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). Dieser grundlegende Schritt ist für die Integration von Adobe Experience Cloud-Produkten wie Adobe Analytics, Target oder neueren innovativen Produkten wie Real-time Customer Data Platform, Customer Journey Analytics und Journey Optimizer von entscheidender Bedeutung.

Außerdem erfahren Sie, wie Sie [WKND - Adobe Experience Manager-Beispielprojekt](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) Seitenansichtsdaten im [Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html?lang=de).

Nach Abschluss dieser Einrichtung können Sie die Implementierung von Experience Platform und verwandten Anwendungen wie [Real-time Customer Data Platform (Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html), [Customer Journey Analytics (CJA)](https://experienceleague.adobe.com/docs/customer-journey-analytics.html) und [Adobe Journey Optimizer (AJO)](https://experienceleague.adobe.com/docs/journey-optimizer.html?lang=de). Förderung einer besseren Kundeninteraktion durch Standardisierung der Web- und Kundendaten.

## Voraussetzungen

Bei der Integration des Experience Platform Web SDK sind folgende Elemente erforderlich.

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

Mit dem Experience-Datenmodell (XDM)-Schema können Sie die Kundenerlebnisdaten standardisieren. So sammeln Sie die **WKND-Seitenansicht** Daten, erstellen Sie ein XDM-Schema und verwenden Sie die von der Adobe bereitgestellten Feldergruppen. `AEP Web SDK ExperienceEvent` für die Webdatenerfassung.


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

Erfahren Sie mehr über das XDM-Schema und verwandte Konzepte wie Feldgruppen, Typen, Klassen und Datentypen aus [XDM-System - Übersicht](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html).

Die [XDM-System - Übersicht](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html) ist eine großartige Ressource, um mehr über das XDM-Schema und verwandte Konzepte wie Feldgruppen, Typen, Klassen und Datentypen zu erfahren. Es bietet ein umfassendes Verständnis des XDM-Datenmodells und wie XDM-Schemas erstellt und verwaltet werden, um Daten im gesamten Unternehmen zu standardisieren. Erfahren Sie mehr darüber, wie Sie das XDM-Schema besser verstehen und wie es Ihre Datenerfassungs- und -verwaltungsprozesse nutzen kann.

## Erstellen von DataStream - Experience Platform

Ein DataStream weist das Platform Edge Network an, wo die erfassten Daten gesendet werden sollen. Sie kann beispielsweise an Experience Platform, Analytics oder Adobe Target gesendet werden.


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

Machen Sie sich mit dem Konzept von Datastreams und verwandten Themen wie Data Governance und Konfiguration vertraut, indem Sie die [Übersicht über Datenspeicher](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html?lang=de) Seite.

## Tag-Eigenschaft erstellen - Experience Platform

Erfahren Sie, wie Sie in Experience Platform eine Tag-Eigenschaft (ehemals Launch) erstellen, um die JavaScript-Bibliothek des Web SDK zur WKND-Website hinzuzufügen. Die neu definierte Tag-Eigenschaft verfügt über die folgenden Ressourcen:

+ Tag-Erweiterungen: [Core](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) und [Adobe Experience Platform Web SDK](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ Datenelemente: Die Datenelemente des benutzerdefinierten Codetyps, die den Seitennamen, den Site-Abschnitt und den Hostnamen mithilfe der Adobe Client Data Layer der WKND-Site extrahieren. Außerdem das Datenelement vom Typ XDM-Objekt , das dem zuvor neu erstellten WKND-XDM-Schema-Build-in entspricht [Erstellen eines XDM-Schemas](#create-xdm-schema---experience-platform) Schritt.
+ Regel: Senden von Daten an das Platform Edge Network bei jedem Besuch einer WKND-Webseite mithilfe der Adobe Client Data Layer ausgelöst `cmp:show` -Ereignis.


>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)

Die [Übersicht über Tags](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=de) bietet umfassende Kenntnisse zu wichtigen Konzepten wie Datenelementen, Regeln und Erweiterungen.

Weitere Informationen zur Integration AEM Kernkomponenten in die Adobe Client-Datenschicht finden Sie im Abschnitt [Verwenden der Adobe Client-Datenschicht mit AEM Leitfaden zu Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=de).

## Tag-Eigenschaft mit AEM verbinden

Erfahren Sie, wie Sie die kürzlich erstellte Tag-Eigenschaft über die Adobe IMS- und Adobe Launch-Konfiguration in AEM mit AEM verknüpfen. Wenn eine AEM as a Cloud Service Umgebung eingerichtet ist, werden mehrere Konfigurationen des technischen Adobe IMS-Kontos automatisch generiert, einschließlich Adobe Launch. Für AEM Version 6.5 müssen Sie jedoch eine manuell konfigurieren.

Nach Verknüpfung der Tag-Eigenschaft kann die WKND-Site die JavaScript-Bibliothek der Tag-Eigenschaft mithilfe der Cloud Service-Konfiguration von Adobe Launch auf die Webseiten laden.

### Laden der Tag-Eigenschaft auf WKND überprüfen

Verwenden von Adobe Experience Platform Debugger [Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) oder [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) -Erweiterung überprüfen, ob die Tag-Eigenschaft auf WKND-Seiten geladen wird. Sie können überprüfen,

+ Tag-Eigenschaftendetails wie Erweiterung, Version, Name und mehr.
+ Platform Web SDK-Bibliotheksversion, Datastream-ID
+ XDM-Objekt als Teil `events` -Attribut im Experience Platform Web SDK

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## Datensatz erstellen - Experience Platform

Die mit dem Web SDK erfassten Seitenansichtsdaten werden im Experience Platform Data Lake als Datensätze gespeichert. Der Datensatz ist ein Speicher- und Verwaltungskonstrukt für eine Sammlung von Daten wie eine Datenbanktabelle, die einem Schema folgt. Erfahren Sie, wie Sie einen Datensatz erstellen und den zuvor erstellten Datenspeicher so konfigurieren, dass Daten an die Experience Platform gesendet werden.


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

Die [Datensätze - Übersicht](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html?lang=de) bietet weitere Informationen zu Konzepten, Konfigurationen und anderen Erfassungsfunktionen.


## WKND-Seitenansichtsdaten in Experience Platform

Nach der Einrichtung des Web SDK mit AEM, insbesondere auf der WKND-Site, ist es an der Zeit, Traffic durch die Site-Seiten zu generieren. Bestätigen Sie dann, dass die Seitenansichtsdaten in den Experience Platform-Datensatz aufgenommen werden. In der Benutzeroberfläche &quot;Datensatz&quot;werden verschiedene Details wie Datensätze insgesamt, Größe und erfasste Batches zusammen mit einem visuell ansprechenden Balkendiagramm angezeigt.

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## Zusammenfassung

Gute gemacht! Sie haben die Einrichtung von AEM mit dem Adobe Experience Platform (Experience Platform) Web SDK abgeschlossen, um Daten von einer Website zu erfassen und zu erfassen. Auf dieser Grundlage können Sie nun weitere Möglichkeiten zur Verbesserung und Integration von Produkten wie Analytics, Target, Customer Journey Analytics (CJA) und vielen anderen erkunden, um umfangreiche, personalisierte Erlebnisse für Ihre Kunden zu erstellen. Lernen Sie weiter und lernen Sie, um das gesamte Potenzial von Adobe Experience Cloud auszuschöpfen.

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)

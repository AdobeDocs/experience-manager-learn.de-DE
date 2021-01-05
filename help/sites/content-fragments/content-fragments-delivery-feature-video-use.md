---
title: Bereitstellen von Inhaltsfragmenten in AEM
seo-title: Bereitstellen von Inhaltsfragmenten in Adobe Experience Manager
description: Inhaltsfragmente, unabhängig vom Layout, können direkt in AEM Sites mit Kernkomponenten verwendet werden oder auf kostenlose Weise an nachgeschaltete Kanal geliefert werden.
seo-description: Inhaltsfragmente, unabhängig vom Layout, können direkt in AEM Sites mit Kernkomponenten verwendet werden oder auf kostenlose Weise an nachgeschaltete Kanal geliefert werden.
sub-product: content-services
feature: content-fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 6%

---


# Bereitstellen von Inhaltsfragmenten {#delivering-content-fragments}

Adobe Experience Manager (AEM) Inhaltsfragmente sind textbasierte redaktionelle Inhalte, die mit einigen strukturierten Datenelementen verknüpft sein können, die jedoch ohne Design- oder Layoutinformationen als reiner Inhalt betrachtet werden. Inhaltsfragmente werden in der Regel als Kanal-agnostischer Inhalt erstellt, der für die Verwendung und Wiederverwendung über Kanal hinweg vorgesehen ist, wodurch der Inhalt wiederum in ein kontextspezifisches Erlebnis eingeschlossen wird.

Inhaltsfragmente, unabhängig vom Layout, können direkt in AEM Sites mit Kernkomponenten verwendet werden oder auf kostenlose Weise an nachgeschaltete Kanal geliefert werden.

Diese Videoreihe behandelt die Optionen zum Versand für die Verwendung von Inhaltsfragmenten. Details zum Definieren und [Authoring von Inhaltsfragmenten finden Sie hier](content-fragments-feature-video-use.md).

1. Inhaltsfragmente auf Webseiten verwenden
2. Bereitstellen von Inhaltsfragmenten als JSON mithilfe von AEM Content Services
3. Verwenden der Asset HTTP-API

## Verwenden von Inhaltsfragmenten auf Webseiten {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

Inhaltsfragmente können auf AEM Sites-Seiten oder ähnlich wie Erlebnisfragmente verwendet werden, indem die AEM WCM-Kernkomponenten [Inhaltsfragment-Komponente](https://docs.adobe.com/content/help/de/experience-manager-core-components/using/components/content-fragment-component.html) verwendet werden.

Inhaltsfragmentkomponenten können mit AEM Stilsystem formatiert werden, um den Inhalt nach Bedarf anzuzeigen.

## Inhaltsfragmente als JSON verfügbar machen {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

AEM Content Services erleichtert die Erstellung AEM seitenbasierten HTTP-Endpunkten, die Inhalte in einem normalisierten JSON-Format darstellen.

Im obigen Video wird die Komponente [Inhaltsfragment](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html) verwendet, um einzelne Inhaltsfragmente verfügbar zu machen. Die Komponente [Inhaltsfragment-Liste](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-list.html) ist eine neue Komponente, mit der ein Autor eine Abfrage definieren kann, die die Seite mit einer Liste Inhaltsfragmente dynamisch füllt. Die Komponente &quot;Inhaltsfragment-Liste&quot;wird bevorzugt, wenn mehrere Inhaltsfragmente verfügbar gemacht werden müssen.

*Beispiel für eine JSON-Endpunkt-Nutzlast für Content Services:*\
**[athletes.json](assets/athletes.json)**

## Verwenden der Asset HTTP-API

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

Die erste Einführung in AEM 6.5 ist die verbesserte Unterstützung für Inhaltsfragmente mit der Assets HTTP API. Dies bietet Entwicklern eine einfache Möglichkeit, CRUD-Vorgänge (Create, Read, Update, and Delete) für Inhaltsfragmente durchzuführen.

*Beispiel für POSTMAN-Anforderungen:*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## Welche Versand-Methode

### Webkanal

Der Ansatz zur Bereitstellung eines Inhaltsfragments über einen Web-Kanal ist unkompliziert, wenn Sie die Komponente &quot;Inhaltsfragment&quot;mit AEM Sites verwenden.

### Headless

Es gibt zwei Optionen, um Inhaltsfragment als JSON verfügbar zu machen, um einen Kanal eines Drittanbieters in einem nutzungsfreien Fall zu unterstützen:

1. Verwenden Sie AEM Content Services- und Proxy-API-Seiten (Video Nr. 2), wenn der primäre Anwendungsfall die Bereitstellung von Inhaltsfragmenten für den Verbrauch (schreibgeschützt) durch einen Kanal eines Drittanbieters ist. Das Content Services-Framework bietet mehr Flexibilität und Optionen hinsichtlich der offen gelegten Daten. Entwickler können das Content Services-Framework auch erweitern, um die Daten zu erweitern und/oder zu bereichern.

2. Verwenden Sie die Asset HTTP-API (Video Nr. 3), wenn der Drittanbieter-Kanal Inhaltsfragmente ändern und/oder aktualisieren muss. Ein typischer Anwendungsfall ist die Aufnahme von Drittanbieterinhalten in einer AEM Autorenversion-Umgebung.

## Zusätzliche Ressourcen {#additional-resources}

* [Authoring von Inhaltsfragmenten](content-fragments-feature-video-use.md)
* [AEM WCM-Hauptkomponenten](https://docs.adobe.com/content/help/de/experience-manager-core-components/using/introduction.html)
* [AEM WCM-Kerninhaltsfragment-Komponente](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)

So laden Sie das unten stehende Paket auf eine AEM 6.4+-Instanz für den finalen Status aus der Videoreihe herunter und installieren es:\
**[aem_demo_Fluid-experienceContent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**

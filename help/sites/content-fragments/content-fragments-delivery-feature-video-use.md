---
title: Bereitstellen von Inhaltsfragmenten in AEM
seo-title: Bereitstellen von Inhaltsfragmenten in Adobe Experience Manager
description: Inhaltsfragmente können unabhängig vom Layout direkt in AEM Sites mit Kernkomponenten verwendet oder Headless-Implementierung an nachgelagerte Kanäle durchgeführt werden.
seo-description: Inhaltsfragmente können unabhängig vom Layout direkt in AEM Sites mit Kernkomponenten verwendet oder Headless-Implementierung an nachgelagerte Kanäle durchgeführt werden.
sub-product: content-services
feature: Inhaltsfragmente
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
topic: Content Management
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 7%

---


# Bereitstellen von Inhaltsfragmenten {#delivering-content-fragments}

Adobe Experience Manager (AEM) Inhaltsfragmente sind textbasierte redaktionelle Inhalte, die mit einigen strukturierten Datenelementen verknüpft sein können, aber ohne Design- oder Layoutinformationen als reine Inhalte betrachtet werden. Inhaltsfragmente werden in der Regel als kanalagnostischer Inhalt erstellt, der kanalübergreifend verwendet und wiederverwendet werden soll. Dadurch wird der Inhalt wiederum in ein kontextspezifisches Erlebnis eingeschlossen.

Inhaltsfragmente können unabhängig vom Layout direkt in AEM Sites mit Kernkomponenten verwendet oder Headless-Implementierung an nachgelagerte Kanäle durchgeführt werden.

Diese Videoreihe behandelt die Bereitstellungsoptionen für die Verwendung von Inhaltsfragmenten. Details zum Definieren und [Authoring von Inhaltsfragmenten finden Sie hier](content-fragments-feature-video-use.md).

1. Verwenden von Inhaltsfragmenten auf Webseiten
2. Bereitstellen von Inhaltsfragmenten als JSON mithilfe von AEM Content Services
3. Verwenden der Assets-HTTP-API

## Verwenden von Inhaltsfragmenten in Web-Seiten {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

Inhaltsfragmente können auf AEM Sites-Seiten oder auf ähnliche Weise Experience Fragments verwendet werden, die die AEM WCM-Kernkomponenten [Inhaltsfragmentkomponente](https://docs.adobe.com/content/help/de/experience-manager-core-components/using/components/content-fragment-component.html) verwenden.

Inhaltsfragmentkomponenten können mit AEM Stilsystem formatiert werden, um den Inhalt nach Bedarf anzuzeigen.

## Anzeigen von Inhaltsfragmenten als JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

AEM Content Services ermöglicht die Erstellung von AEM seitenbasierten HTTP-Endpunkten, die Inhalte in ein normalisiertes JSON-Format ausgeben.

Das obige Video verwendet die [Inhaltsfragmentkomponente](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html), um einzelne Inhaltsfragmente anzuzeigen. Die [Inhaltsfragmentlisten-Komponente](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-list.html) ist eine neue Komponente, die es einem Autor ermöglicht, eine Abfrage zu definieren, die die Seite dynamisch mit einer Liste von Inhaltsfragmenten füllt. Die Inhaltsfragmentlisten-Komponente wird bevorzugt, wenn mehrere Inhaltsfragmente verfügbar gemacht werden müssen.

*Beispiel einer JSON-Payload des Content Services-Endpunkts:*\
**[athletes.json](assets/athletes.json)**

## Verwenden der Assets-HTTP-API

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

Erste Einführung in AEM 6.5 ist die verbesserte Unterstützung für Inhaltsfragmente mit der Assets-HTTP-API. Dadurch können Entwickler auf einfache Weise CRUD-Vorgänge (Create, Read, Update, Delete, Erstellen, Lesen und Löschen) für Inhaltsfragmente durchführen.

*Beispiel für POSTMAN-Anforderungen:*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## Welche Versandmethode verwendet wird

### Webkanal

Die Bereitstellung eines Inhaltsfragments über einen Webkanal ist einfach, indem die Inhaltsfragment-Komponente mit AEM Sites verwendet wird.

### Headless

Es gibt zwei Möglichkeiten, Inhaltsfragment als JSON verfügbar zu machen, um einen Drittanbieterkanal in einem Headless-Anwendungsfall zu unterstützen:

1. Verwenden Sie AEM Content Services- und Proxy-API-Seiten (Video Nr. 2), wenn das primäre Anwendungsbeispiel die Bereitstellung von Inhaltsfragmenten für die Verwendung (schreibgeschützt) durch einen Drittanbieterkanal ist. Das Content Services-Framework bietet mehr Flexibilität und Optionen hinsichtlich der offen gelegten Daten. Entwickler können auch das Content Services-Framework erweitern, um die Daten zu erweitern und/oder anzureichern.

2. Verwenden Sie die Assets-HTTP-API (Video Nr. 3), wenn der Kanal eines Drittanbieters Inhaltsfragmente ändern und/oder aktualisieren muss. Ein typisches Anwendungsbeispiel ist die Aufnahme von Inhalten von Drittanbietern in einer AEM Autorenumgebung.

## Zusätzliche Ressourcen {#additional-resources}

* [Erstellen von Inhaltsfragmenten](content-fragments-feature-video-use.md)
* [AEM WCM-Kernkomponenten](https://docs.adobe.com/content/help/de/experience-manager-core-components/using/introduction.html)
* [AEM WCM-Kerninhaltsfragment-Komponente](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)

So laden Sie das unten stehende Paket in einer AEM 6.4+ -Instanz für den finalen Status aus der Videoreihe herunter und installieren es:\
**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**

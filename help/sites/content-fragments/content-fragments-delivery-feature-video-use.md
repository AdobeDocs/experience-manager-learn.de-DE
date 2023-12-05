---
title: Bereitstellen von Inhaltsfragmenten in AEM
description: Inhaltsfragmente können unabhängig vom Layout direkt in AEM Sites mit Kernkomponenten verwendet oder in Headless-Form für nachgelagerte Kanäle bereitgestellt werden.
feature: Content Fragments
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 525cd30c-05bf-4f17-b61b-90609ce757ea
duration: 913
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 100%

---

# Bereitstellen von Inhaltsfragmenten {#delivering-content-fragments}

Adobe Experience Manager(AEM)-Inhaltsfragmente sind textbasierte redaktionelle Inhalte, die mit einigen strukturierten Datenelementen verknüpft sein können, aber ohne Design- oder Layout-Informationen als reine Inhalte betrachtet werden. Inhaltsfragmente werden in der Regel als kanalunabhängiger Inhalt erstellt, der kanalübergreifend verwendet und wiederverwendet werden soll. Dadurch wird der Inhalt wiederum in ein kontextspezifisches Erlebnis eingeschlossen.

Inhaltsfragmente können unabhängig vom Layout direkt in AEM Sites mit Kernkomponenten verwendet oder in Headless-Form für nachgelagerte Kanäle bereitgestellt werden.

In dieser Videoreihe werden die Bereitstellungsoptionen für die Verwendung von Inhaltsfragmenten behandelt. Details zur Definition und Erstellung von Inhaltsfragmenten finden Sie [hier](content-fragments-feature-video-use.md).

1. Verwenden von Inhaltsfragmenten auf Web-Seiten
2. Bereitstellen von Inhaltsfragmenten als JSON mithilfe von AEM Content Services
3. Verwenden der Assets-HTTP-API

## Verwenden von Inhaltsfragmenten auf Web-Seiten {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449?quality=12&learn=on)

Inhaltsfragmente können auf AEM Sites-Seiten eingesetzt werden. Ähnlich anwendbar sind Experience Fragments. Möglich macht dies die [Inhaltsfragment-Komponente](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=de) der AEM WCM-Kernkomponenten.

Inhaltsfragment-Komponenten können mit dem AEM-Stilsystem formatiert werden, um den Inhalt nach Bedarf anzuzeigen.

## Bereitstellen von Inhaltsfragmenten als JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448?quality=12&learn=on)

AEM Content Services ermöglicht die Erstellung von auf AEM-Seiten basierenden HTTP-Endpunkten, durch die eine Ausgabedarstellung der Inhalte in einem normalisierten JSON-Format erfolgt.

Im obigen Video wird die [Inhaltsfragment-Komponente](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=de) verwendet, um einzelne Inhaltsfragmente bereitzustellen. Die [Inhaltsfragmentlisten-Komponente](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-list.html?lang=de) ist eine neue Komponente, die es Autorinnen und Autoren ermöglicht, eine Abfrage zu definieren, die die Seite dynamisch mit einer Liste von Inhaltsfragmenten auffüllt. Die Inhaltsfragmentlisten-Komponente wird bevorzugt, wenn mehrere Inhaltsfragmente bereitgestellt werden müssen.

*Beispiel einer Content Services-Endpunkt-JSON-Payload:*\
**[athletes.json](assets/athletes.json)**

## Verwenden der Assets-HTTP-API

>[!VIDEO](https://video.tv.adobe.com/v/26390?quality=12&learn=on)

Mit der Assets-HTTP-API wurde in AEM 6.5 eine erweiterte Unterstützung für Inhaltsfragmente eingeführt. Dadurch können Entwicklungspersonen auf einfache Weise CRUD-Vorgänge für Inhaltsfragmente durchführen. (CRUD ist die Abkürzung für Create (Erstellen), Read (Lesen), Update (Aktualisieren) und Delete (Löschen).)

*Beispiel für POSTMAN-Anfragen:*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## Versandmethoden und ihre Anwendung

### Web-Kanal

Die Bereitstellung eines Inhaltsfragments über einen Web-Kanal ist durch das Verwenden der Inhaltsfragment-Komponente mit AEM Sites ein einfacher Vorgang.

### Headless

Es gibt zwei Möglichkeiten, ein Inhaltsfragment als JSON bereitzustellen, um einen Drittanbieterkanal in einem Headless-Anwendungsfall zu unterstützen:

1. Verwenden Sie AEM Content Services und Proxy-API-Seiten (Video Nr. 2), wenn der primäre Anwendungsfall die Bereitstellung von Inhaltsfragmenten zur (schreibgeschützten) Nutzung durch einen Drittanbieterkanal ist. Das Content Services-Framework bietet mehr Flexibilität und Optionen dahingehend, welche Daten bereitgestellt werden. Entwicklungspersonen können das Content Services-Framework auch erweitern, um die Daten zu ergänzen und/oder anzureichern.

2. Verwenden Sie die Assets-HTTP-API (Video Nr. 3), wenn für den Kanal eines Drittanbieters Inhaltsfragmente geändert und/oder aktualisiert werden müssen. Ein typischer Anwendungsfall ist hierbei die Aufnahme von Drittanbieterinhalten in einer AEM-Authoring-Umgebung.

## Zusätzliche Ressourcen {#additional-resources}

* [Verfassen von Inhaltsfragmenten](content-fragments-feature-video-use.md)
* [AEM WCM-Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de)
* [AEM WCM-Kerninhaltsfragment-Komponente](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=de)

Laden Sie das folgende Paket herunter und installieren es in einer AEM 6.4-Instanz oder höher, um den Endzustand aus der Videoreihe zu erreichen:\
**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**

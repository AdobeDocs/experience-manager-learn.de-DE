---
title: Erstes Tutorial zu AEM Headless
description: Erfahren Sie alles über eine AEM Headless-First-Anwendung.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: b0ac4b50-5fe5-41a1-9530-8e593d7000c9
duration: 122
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 100%

---

# Erstes Tutorial zu AEM Headless

{{aem-headless-trials-promo}}

Willkommen beim Tutorial zum Erstellen eines Web-Erlebnisses mit React, mit vollständiger Unterstützung von AEM Headless-APIs und GraphQL. In diesem Tutorial führen wir Sie durch den Prozess der Erstellung einer dynamischen und interaktiven Web-Anwendung, indem wir die Leistungsfähigkeit von React, Adobe Experience Manager (AEM) Headless-APIs und GraphQL kombinieren.

React ist eine beliebte JavaScript-Bibliothek zum Erstellen von Benutzeroberflächen, die für ihre Einfachheit, Wiederverwendbarkeit und komponentenbasierte Architektur bekannt ist. AEM bietet leistungsstarke Content-Management-Funktionen und stellt Headless-APIs bereit, mit denen Entwicklerinnen und Entwickler über verschiedene Kanäle und Anwendungen auf in AEM gespeicherte Inhalte und Daten zugreifen können.

Mithilfe von AEM Headless-APIs können Sie Inhalte, Assets und Daten aus Ihrer AEM-Instanz abrufen und zur Unterstützung Ihrer React-Anwendung nutzen. GraphQL, eine flexible Abfragesprache für APIs, bietet eine effiziente und präzise Möglichkeit, spezifische Daten von Ihrer AEM-Instanz anzufordern, was eine nahtlose Integration zwischen React und AEM ermöglicht.

![Erstes Tutorial zu AEM Headless](./assets/overview/overview.png)

In diesem Tutorial führen wir Sie schrittweise durch den Prozess der Erstellung eines Web-Erlebnisses mit React und AEM Headless-APIs mit GraphQL. Sie erfahren, wie Sie Ihre Entwicklungsumgebung einrichten, eine Verbindung zwischen React und AEM herstellen sowie Inhalte mithilfe von GraphQL-Abfragen abrufen und dynamisch in Ihrer Web-Anwendung rendern.

Wir behandeln Themen wie die Konfiguration Ihres React-Projekts, die Einrichtung einer Authentifizierung mit AEM, die Abfrage von Inhalten von AEM mithilfe von GraphQL, die Verarbeitung von Daten in Ihren React-Komponenten und die Leistungsoptimierung durch Zwischenspeicherung und Paginierung.

Am Ende dieses Tutorials verfügen Sie über fundierte Kenntnisse darüber, wie Sie React, AEM Headless-APIs und GraphQL nutzen können, um ein leistungsstarkes und ansprechendes Web-Erlebnis zu erstellen. Tauchen wir ein und beginnen wir mit der Erstellung Ihrer nächsten Web-Anwendung!

## Voraussetzungen

### Kompetenzen

+ React-Kenntnisse
+ GraphQL-Kenntnisse
+ Basiswissen zu AEM as a Cloud Service

### AEM as a Cloud Service

Für dieses Tutorial ist der Administratorzugriff auf eine AEM as a Cloud Service-Umgebung erforderlich.

### Software

+ [Node.js v16+](https://nodejs.org/de/)
   + Überprüfen Sie die Knotenversion, indem Sie `node -v` über die Befehlszeile ausführen
+ [npm 6+](https://www.npmjs.com/)
   + Überprüfen Sie Ihre npm-Version, indem Sie `npm -v` über die Befehlszeile ausführen
+ [Git](https://git-scm.com/)
   + Überprüfen Sie Ihre Git-Version, indem Sie `git -v` über die Befehlszeile ausführen

Verwenden Sie den [Knoten-Versionsmanager (nvm)](https://github.com/nvm-sh/nvm), falls sich mehrere Versionen von node.js auf demselben Computer befinden.

Vergewissern Sie sich, dass Sie berechtigt sind, Software global auf Ihrem Computer zu installieren.

## Nächster Schritt

Nachdem Sie Ihre Umgebung eingerichtet haben, fahren wir mit dem nächsten Schritt fort: [Einrichten und Verfassen von Inhalten in AEM as a Cloud Service](./1-content-modeling.md)

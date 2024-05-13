---
title: Bearbeiten von React-Apps mit dem universellen Editor
description: Erfahren Sie, wie Sie den Inhalt einer React-App mit dem universellen Editor bearbeiten.
version: Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
source-git-commit: 14767141348d3d56c154704cc21d39722bb67aec
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 1%

---


# Bearbeiten von React-Apps mit dem universellen Editor

Erfahren Sie, wie Sie den Inhalt einer React-App mit dem universellen Editor bearbeiten. Die Inhalte werden in Inhaltsfragmenten in AEM gespeichert und mithilfe von GraphQL-APIs abgerufen.

Dieses Tutorial führt Sie durch den Prozess der Einrichtung der lokalen Entwicklungsumgebung, der Instrumentierung der React-App zur Bearbeitung ihres Inhalts und der Bearbeitung des Inhalts mit dem universellen Editor.

## Lernmethoden

Dieses Tutorial behandelt folgende Themen:

- Kurzübersicht über den universellen Editor
- Einrichten der lokalen Entwicklungsumgebung
   - **AEM SDK**: stellt die Inhalte bereit, die in Inhaltsfragmenten für die React-App mithilfe von GraphQL-APIs gespeichert sind.
   - **React-App**: eine einfache Benutzeroberfläche, in der die Inhalte von AEM angezeigt werden.
   - **Universal Editor Service**: a _lokale Kopie des Universal Editor-Dienstes_ die den universellen Editor und das AEM SDK bindet.
- Instrumentieren der React-App zum Bearbeiten des Inhalts mit dem universellen Editor
- Bearbeiten des Inhalts der React-App mit dem universellen Editor


## Überblick über den universellen Editor

Der universelle Editor ermöglicht Autoren und Entwicklern von Inhalten (Frontend und Backend). Lassen Sie uns einige der wichtigsten Vorteile des universellen Editors überprüfen:

- Entwickelt zum Bearbeiten von Headless- und Headful-Inhalten im WYSIWYG-Modus (What-you-see-is-what-you-get).
- Bietet ein konsistentes Content-Editing-Erlebnis für verschiedene Frontend-Technologien wie React, Angular, Vue usw. Auf diese Weise können die Inhaltsautoren den Inhalt bearbeiten, ohne sich um die zugrunde liegende Frontend-Technologie zu sorgen.
- Für die Aktivierung des Universal Editors in der Frontend-Anwendung ist eine sehr minimale Instrumentierung erforderlich. So maximieren Sie die Produktivität des Entwicklers und ermöglichen ihm, sich auf die Erstellung des Erlebnisses zu konzentrieren.
- Trennung von Problemen zwischen drei Rollen, Inhaltsautoren, Frontend-Entwicklern und Backend-Entwicklern, sodass sich jede Rolle auf ihre Kernaufgaben konzentrieren kann.


## Beispiel-React-App

Dieses Tutorial verwendet [**WKND-Teams**](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#react-app---basic-tutorial---teampersons) als Beispiel für eine React-App. Die **WKND-Teams** Die React-App zeigt eine Liste der Teammitglieder und deren Details an.

Die Teamdetails wie Titel, Beschreibung und Teammitglieder werden als _Team_ Inhaltsfragmente in AEM. Ebenso werden die Personendetails wie Name, Biografie und Profilbild als _Person_ Inhaltsfragmente in AEM.

Der Inhalt für die React-App wird von AEM mithilfe von GraphQL-APIs bereitgestellt und die Benutzeroberfläche wird mit zwei React-Komponenten erstellt, `Teams` und `Person`.

Eine entsprechende Anleitung zum Erstellen der **WKND-Teams** React-App. Sie finden das Tutorial [here](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview).

## Nächster Schritt

Erfahren Sie, wie [die lokale Entwicklungsumgebung einrichten](./local-development-setup.md).

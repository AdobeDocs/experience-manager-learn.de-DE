---
title: Bearbeiten von React-Apps mit dem universellen Editor
description: Erfahren Sie, wie Sie den Inhalt einer React-App mit dem universellen Editor bearbeiten.
version: Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 87
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 69ed610e-2eff-43b3-98f9-3dc40594e879
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
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
   - **AEM SDK**: Es stellt die Inhalte bereit, die in Inhaltsfragmenten für die React-App mithilfe von GraphQL-APIs gespeichert sind.
   - **React app**: eine einfache Benutzeroberfläche, in der die Inhalte von AEM angezeigt werden.
   - **Universal Editor Service**: eine _lokale Kopie des Universal Editor-Dienstes_, die den universellen Editor und das AEM SDK bindet.
- Instrumentieren der React-App zum Bearbeiten des Inhalts mit dem universellen Editor
- Bearbeiten des Inhalts der React-App mit dem universellen Editor


## Überblick über den universellen Editor

Der universelle Editor ermöglicht Autoren und Entwicklern von Inhalten (Frontend und Backend). Lassen Sie uns einige der wichtigsten Vorteile des universellen Editors überprüfen:

- Entwickelt zum Bearbeiten von Headless- und Headful-Inhalten im WYSIWYG-Modus (What-you-see-is-what-you-get).
- Bietet ein konsistentes Content-Editing-Erlebnis für verschiedene Frontend-Technologien wie React, Angular, Vue usw. Auf diese Weise können die Inhaltsautoren den Inhalt bearbeiten, ohne sich um die zugrunde liegende Frontend-Technologie zu sorgen.
- Für die Aktivierung des Universal Editors in der Frontend-Anwendung ist eine sehr minimale Instrumentierung erforderlich. So maximieren Sie die Produktivität des Entwicklers und ermöglichen ihm, sich auf die Erstellung des Erlebnisses zu konzentrieren.
- Trennung von Problemen zwischen drei Rollen, Inhaltsautoren, Frontend-Entwicklern und Backend-Entwicklern, sodass sich jede Rolle auf ihre Kernaufgaben konzentrieren kann.


## Beispiel-React-App

In diesem Tutorial wird [**WKND-Teams**](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#react-app---basic-tutorial---teampersons) als React-Beispielanwendung verwendet. Die React-App **WKND-Teams** enthält eine Liste der Teammitglieder und deren Details.

Die Teamdetails wie Titel, Beschreibung und Teammitglieder werden in AEM als _Team_-Inhaltsfragmente gespeichert. Ebenso werden die Personendetails wie Name, Biografie und Profilbild als Inhaltsfragmente für _Person_ in AEM gespeichert.

Der Inhalt für die React-App wird von AEM mit GraphQL-APIs bereitgestellt und die Benutzeroberfläche wird mit zwei React-Komponenten erstellt: `Teams` und `Person`.

Ein entsprechendes Tutorial erfahren Sie, wie Sie die React-App **WKND-Teams** erstellen. Das Tutorial [finden Sie hier](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview).

## Nächster Schritt

Erfahren Sie, wie Sie [die lokale Entwicklungsumgebung einrichten](./local-development-setup.md).

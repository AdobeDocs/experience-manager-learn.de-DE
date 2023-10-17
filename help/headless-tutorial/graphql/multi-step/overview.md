---
title: Praktisches Tutorial zu den ersten Schritten mit AEM Headless – GraphQL
description: Ein durchgehendes Tutorial, das erläutert, wie sich Inhalte mithilfe von AEM GraphQL-APIs aufbauen und bereitstellen lassen.
doc-type: tutorial
mini-toc-levels: 1
kt: 6678
thumbnail: KT-6678.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
exl-id: 41e15a2c-758b-4e7d-9d23-ef671c1dc155
source-git-commit: 4c0770eafbbbb90bfc00ab49be02e84c41d63057
workflow-type: ht
source-wordcount: '287'
ht-degree: 100%

---

# Erste Schritte mit AEM Headless – GraphQL

{{aem-headless-trials-promo}}

Ein durchgehendes Tutorial, in dem erläutert wird, wie sich Inhalte mithilfe von AEM GraphQL-APIs aufbauen und bereitstellen lassen, die von einer externen App in einem Headless-CMS-Szenario genutzt werden.

In diesem Tutorial wird untersucht, wie AEM GraphQL-APIs und Headless-Funktionen verwendet werden können, um die Erlebnisse zu optimieren, die in einer externen App angezeigt werden.

Dieses Tutorial behandelt folgende Themen:

* Erstellen einer Projektkonfiguration
* Erstellen von Inhaltsfragmentmodellen zum Modellieren von Daten
* Erstellen Sie Inhaltsfragmente anhand der zuvor erstellten Modelle.
* Erfahren Sie, wie Inhaltsfragmente in AEM mithilfe des integrierten GraphiQL-Entwicklungs-Tools abgefragt werden können.
* So speichern oder persistieren Sie die GraphQL-Abfragen in AEM
* Verwenden persistierter GraphQL-Abfragen aus einer React-Beispielanwendung

## Voraussetzungen {#prerequisites}

Für dieses Tutorial sind folgende Dinge erforderlich:

* Grundlegende HTML- und JavaScript-Kenntnisse
* Die folgenden Tools müssen lokal installiert sein:
   * [Node.js v18](https://nodejs.org/)
   * [Git](https://git-scm.com/)
   * Eine IDE (z. B. [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

### AEM-Umgebung

Um dieses Tutorial abzuschließen, wird ein AEM Administratorzugriff auf eine AEM as a Cloud Service-Umgebung empfohlen. Wenn Sie keinen Zugriff auf eine AEM as a Cloud Service-Umgebung haben, können Sie das [lokale Schnellstart-SDK für AEM as a Cloud Service](/help/cloud-service/local-development-environment/aem-runtime.md) verwenden. Beachten Sie jedoch, dass einige Bildschirme der Produktoberfläche, wie z. B. die Navigation zu Inhaltsfragmenten, unterschiedlich sind.

## Fangen wir an!

Starten Sie das Tutorial mit [Definieren von Inhaltsfragmentmodellen](content-fragment-models.md).

## GitHub-Projekt

Der Quell-Code und die Inhaltspakete sind verfügbar in den [AEM Handbüchern – WKND GraphQL GitHub-Projekt](https://github.com/adobe/aem-guides-wknd-graphql).

Wenn Sie ein Problem mit dem Tutorial oder dem Code feststellen, hinterlassen Sie ein [GitHub-Problem](https://github.com/adobe/aem-guides-wknd-graphql/issues).

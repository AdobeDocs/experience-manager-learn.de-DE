---
title: AEM Headless OpenAPI-Tutorial | Bereitstellung von Inhaltsfragmenten
description: Ein durchgehendes Tutorial, in dem erläutert wird, wie Inhalte mithilfe der OpenAPI-basierten APIs zur Bereitstellung von Inhaltsfragmenten in AEM erstellt und bereitgestellt werden.
doc-type: Tutorial
mini-toc-levels: 1
jira: KT-6678
thumbnail: KT-6678.jpg
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
duration: 54
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 22%

---

# Erste Schritte mit der Bereitstellung von AEM-Inhaltsfragmenten mit OpenAPIs

In diesem Tutorial wird veranschaulicht, wie AEM-Inhalte mithilfe der Inhaltsfragmentbereitstellung von AEM mit OpenAPIs erstellt und verfügbar gemacht und von einem externen Programm in einem Headless-CMS-Szenario genutzt werden. In diesem Artikel werden diese Konzepte erläutert, indem die Erstellung einer React-App vorgestellt wird, die WKND-Teams und zugehörige Mitgliederdetails anzeigt. Teams und Mitglieder werden mithilfe von AEM-Inhaltsfragmentmodellen modelliert und von der React-App mithilfe der Bereitstellung von AEM-Inhaltsfragmenten mit OpenAPIs genutzt.

![WKND Teams-App](./assets/overview/main.png)

Dieses Tutorial behandelt folgende Themen:

* Erstellen einer Projektkonfiguration
* Erstellen von Inhaltsfragmentmodellen zum Modellieren von Daten
* Erstellen Sie Inhaltsfragmente basierend auf den zuvor erstellten Modellen
* Erfahren Sie, wie Inhaltsfragmente in AEM mithilfe der Funktion „Probieren Sie es aus“ der AEM-Inhaltsfragmentbereitstellung mit OpenAPIs-Dokumentation abgefragt werden können
* Verwenden von Inhaltsfragmentdaten über die Bereitstellung von AEM-Inhaltsfragmenten mit OpenAPI-Aufrufen aus einer React-Beispiel-App
* Verbessern der React-App, damit sie im universellen Editor bearbeitbar ist

## Voraussetzungen {#prerequisites}

Für dieses Tutorial sind folgende Dinge erforderlich:

* AEM Sites as a Cloud Service
* Grundlegende HTML- und JavaScript-Kenntnisse
* Die folgenden Tools müssen lokal installiert sein:
   * [Node.js v22+](https://nodejs.org/)
   * [Git](https://git-scm.com/)
   * Eine IDE (z. B. [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

### AEM as a Cloud Service-Umgebung

Um dieses Tutorial abzuschließen, sollten Sie **AEM-Administrator** Zugriff auf eine AEM as a Cloud Service-Umgebung haben. Eine **Entwicklungs**-Umgebung **eine schnelle** oder eine Umgebung in einem **Sandbox-Programm** kann ebenfalls verwendet werden.

## Fangen wir an!

Starten Sie das Tutorial mit [Definieren von Inhaltsfragmentmodellen](1-content-fragment-models.md).

## GitHub-Projekt

Der Quell-Code und die Inhaltspakete sind im GitHub-Repository {[} der AEM Headless-Tutorials verfügbar.](https://github.com/adobe/aem-tutorials)

Die [`main` Verzweigung enthält den endgültigen Quell-Code ](https://github.com/adobe/aem-tutorials/tree/main/headless/open-api/basic) dieses Tutorials.
Momentaufnahmen des Codes am Ende jedes Schritts sind als Git-Tags verfügbar.

* Beginn von Kapitel 4 - React-App: [`headless_open-api_basic`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic//headless/open-api/basic)
* Ende von Kapitel 4 - React-App: [`headless_open-api_basic_4-end`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_4-end//headless/open-api/basic)
* Ende von Kapitel 5 - Universeller Editor: [`headless_open-api_basic_5-end`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_5-end//headless/open-api/basic)

Wenn Sie ein Problem mit dem Tutorial oder dem Code feststellen, hinterlassen Sie ein [GitHub-Problem](https://github.com/adobe/aem-tutorials/issues).

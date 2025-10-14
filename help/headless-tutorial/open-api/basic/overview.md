---
title: Tutorial zu AEM Headless OpenAPI | Bereitstellung von Inhaltsfragmenten
description: Ein umfassendes Tutorial, das erläutert, wie Sie Inhalte mithilfe der OpenAPI-basierten APIs von AEM für die Bereitstellung von Inhaltsfragmenten erstellen und bereitstellen können.
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
exl-id: 1bb7c415-58f8-4f6c-a0bc-38bdbdb521cf
source-git-commit: e7960fa75058c072b1b52ba1a0d7c99a0280d02f
workflow-type: ht
source-wordcount: '365'
ht-degree: 100%

---

# Erste Schritte mit der AEM-Inhaltsfragmentbereitstellung mit OpenAPI-APIs

Ein umfassendes Tutorial, das erläutert, wie AEM-Inhalte in einem Headless-CMS-Szenario mithilfe der Bereitstellung von Inhaltsfragmenten von AEM mit OpenAPI-APIs erstellt und bereitgestellt und von einer externen App genutzt werden. In diesem Artikel werden diese Konzepte erläutert, indem Sie Schritt für Schritt durch die Erstellung einer React-App geführt werden, die WKND-Teams und zugehörige Mitgliederdetails anzeigt. Teams und Mitglieder werden mithilfe von AEM-Inhaltsfragmentmodellen modelliert und von der React-App mithilfe der AEM-Inhaltsfragmentbereitstellung mit OpenAPI-APIs genutzt.

![WKND Teams-App](./assets/overview/main.png)

Dieses Tutorial behandelt folgende Themen:

* Erstellen einer Projektkonfiguration
* Erstellen von Inhaltsfragmentmodellen zum Modellieren von Daten
* Erstellen von Inhaltsfragmenten basierend auf den zuvor erstellten Modellen
* Erfahren Sie, Sie mit der AEM-Inhaltsfragmentbereitstellung mit der Funktion „Jetzt ausprobieren“ der Dokumentation zu OpenAPI-APIs Inhaltsfragmente in AEM abfragen können.
* Verwenden von Inhaltsfragmentdaten über die AEM-Inhaltsfragmentbereitstellung mit OpenAPI-API-Aufrufen über eine React-Beispiel-App
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

Um dieses Tutorial abzuschließen, sollten Sie über **AEM-Administratorzugriff** auf eine AEM as a Cloud Service-Umgebung verfügen. Sie können auch eine **Entwicklungsumgebung**, eine **schnelle Entwicklungsumgebung** oder eine Umgebung in einem **Sandbox-Programm** verwenden.

## Fangen wir an!

Starten Sie das Tutorial mit [Definieren von Inhaltsfragmentmodellen](1-content-fragment-models.md).

## GitHub-Projekt

Der Quell-Code und die Inhaltspakete sind im GitHub-Repository [AEM Headless Tutorials](https://github.com/adobe/aem-tutorials) verfügbar.

Die [`main`-Verzweigung enthält den endgültigen Quell-Code](https://github.com/adobe/aem-tutorials/tree/main/headless/open-api/basic) für dieses Tutorial.
Momentaufnahmen des Codes am Ende jedes Schritts sind als Git-Tags verfügbar.

* Beginn von Kapitel 4 – React-App: [`headless_open-api_basic`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic//headless/open-api/basic)
* Ende von Kapitel 4 – React-App: [`headless_open-api_basic_4-end`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_4-end//headless/open-api/basic)
* Ende von Kapitel 5 – Universeller Editor: [`headless_open-api_basic_5-end`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_5-end//headless/open-api/basic)

Wenn Sie ein Problem mit dem Tutorial oder dem Code feststellen, hinterlassen Sie ein [GitHub-Problem](https://github.com/adobe/aem-tutorials/issues).

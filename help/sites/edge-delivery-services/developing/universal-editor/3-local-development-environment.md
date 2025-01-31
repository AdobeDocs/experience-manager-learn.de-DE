---
title: Einrichten einer lokalen Entwicklungsumgebung
description: Einrichten einer lokalen Entwicklungsumgebung für Sites, die mit Edge Delivery Services bereitgestellt werden und mit dem universellen Editor bearbeitet werden können.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 700
exl-id: 187c305a-eb86-4229-9896-a74f5d9d822e
source-git-commit: ecf37e1f964d0cda90eeca11b224ab950727d2ad
workflow-type: tm+mt
source-wordcount: '967'
ht-degree: 2%

---

# Einrichten einer lokalen Entwicklungsumgebung

Eine lokale Entwicklungsumgebung ist für die schnelle Entwicklung von Websites, die von Edge Delivery Services bereitgestellt werden, von entscheidender Bedeutung. Die Umgebung verwendet lokal entwickelten Code, während sie Inhalte aus Edge Delivery Services bezieht, sodass Entwickler Code-Änderungen sofort anzeigen können. Ein solches Setup unterstützt eine schnelle, iterative Entwicklung und Tests.

Die Entwicklungstools und -prozesse für ein Edge Delivery Services-Website-Projekt sind so konzipiert, dass sie Web-Entwicklern vertraut sind und ein schnelles und effizientes Entwicklungserlebnis bieten.

## Entwicklungstopologie

Die Entwicklungstopologie für ein Edge Delivery Services-Website-Projekt, das mit dem universellen Editor bearbeitet werden kann, umfasst die folgenden Aspekte:

- **GitHub-**:
   - **Zweck**: hostet den Code der Website (CSS und JavaScript).
   - **Struktur**: Die **Hauptverzweigung** enthält produktionsbereiten Code, während andere Verzweigungen Arbeitscode enthalten.
   - **Funktionalität**: Code aus jeder Verzweigung kann für die **** Produktions- oder **Vorschau**-Umgebungen ausgeführt werden, ohne die Live-Website zu beeinträchtigen.

- **AEM-Autoren-Service**:
   - **Zweck**: Dient als kanonisches Inhalts-Repository, in dem Website-Inhalte bearbeitet und verwaltet werden.
   - **Funktionalität**: Inhalte werden vom **universellen Editor“ gelesen und**. Bearbeitete Inhalte werden in **Edge Delivery Services** in **Produktions-**/**-** veröffentlicht.

- **Universeller Editor**:
   - **Zweck**: Bietet eine WYSIWYG-Oberfläche für die Bearbeitung von Website-Inhalten.
   - **Funktionalität**: Lese- und Schreibvorgänge in den **AEM-Autoren-Service**. Kann so konfiguriert werden, dass Code aus einer beliebigen Verzweigung im GitHub **Repository verwendet**, um Änderungen zu testen und zu validieren.

- **Edge Delivery Services**:
   - **Produktionsumgebung**:
      - **Zweck**: Stellt den Inhalt und Code der Live-Website für Endbenutzer bereit.
      - **Funktionalität**: Stellt Inhalte bereit, die aus dem **AEM Author-Service veröffentlicht**, wobei Code aus der **Hauptverzweigung** des **GitHub-Repositorys verwendet wird**.
   - **Vorschau-Umgebung**:
      - **Zweck**: Bietet eine Staging-Umgebung, in der Inhalte und Code vor der Bereitstellung getestet und in der Vorschau angezeigt werden können.
      - **Funktionalität**: Stellt Inhalte bereit, die aus dem **AEM Author-Service veröffentlicht wurden** wobei Code aus einer beliebigen Verzweigung des **GitHub-Repositorys** verwendet wird. Dies ermöglicht gründliche Tests, ohne dass die Live-Website betroffen ist.

- **Lokale Entwicklungsumgebung**:
   - **Zweck**: Ermöglicht Entwicklern, Code (CSS und JavaScript) lokal zu schreiben und zu testen.
   - **Struktur**:
      - Ein lokaler Klon des **GitHub-Repository** für die verzweigungsbasierte Entwicklung.
      - Die **AEM-CLI**, die als Entwicklungs-Server fungiert, wendet lokale Code-Änderungen auf die **Vorschau-Umgebung** an, um ein Hot-Reload-Erlebnis zu ermöglichen.
   - **Workflow**: Entwicklerinnen und Entwickler schreiben Code lokal, übertragen Änderungen an eine Arbeitsverzweigung, übertragen die Verzweigung auf GitHub, validieren sie im **universellen Editor** (unter Verwendung der angegebenen Verzweigung) und führen sie in die **Hauptverzweigung** zusammen, wenn sie für die Produktionsbereitstellung bereit ist.

## Voraussetzungen

Bevor Sie mit der Entwicklung beginnen, installieren Sie Folgendes auf Ihrem Computer:

1. [Git](https://git-scm.com/)
1. [Node.js und npm](https://nodejs.org)
1. [Microsoft Visual Studio Code](https://code.visualstudio.com/) (oder ähnlicher Code-Editor)

## Klonen des GitHub-Repositorys

Klonen Sie das [GitHub-Repository, das im neuen Codeprojektkapitel erstellt wurde](./1-new-code-project.md) das das AEM Edge Delivery Services-Codeprojekt enthält, in Ihre lokale Entwicklungsumgebung.

![GitHub-Repository-Klon](./assets/3-local-development-environment/github-clone.png)

```bash
$ cd ~/Code
$ git clone git@github.com:<YOUR_ORG>/aem-wknd-eds-ue.git
```

Im `Code` Verzeichnis wird ein neuer `aem-wknd-eds-ue` erstellt, der als Stammordner des Projekts dient. Obwohl das Projekt an einem beliebigen Speicherort auf dem Computer geklont werden kann, verwendet dieses Tutorial `~/Code` als Stammverzeichnis.

## Installieren von Projektabhängigkeiten

Navigieren Sie zum Projektordner und installieren Sie die erforderlichen Abhängigkeiten mit `npm install`. Obwohl Edge Delivery Services-Projekte keine herkömmlichen Node.js-Build-Systeme wie Webpack oder Vite verwenden, benötigen sie dennoch mehrere Abhängigkeiten für die lokale Entwicklung.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install
```

## Installieren Sie die AEM-CLI

Die AEM-CLI ist ein Node.js-Befehlszeilen-Tool, das die Entwicklung Edge Delivery Services-basierter AEM-Websites optimiert und einen lokalen Entwicklungsserver für die schnelle Entwicklung und das Testen Ihrer Website bietet.

Um die AEM-CLI zu installieren, führen Sie Folgendes aus:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install @adobe/aem-cli
```

Die AEM-CLI kann auch global mit `npm install --global @adobe/aem-cli` installiert werden.

## Starten des lokalen AEM-Entwicklungs-Servers

Mit dem Befehl `aem up` wird der lokale Entwicklungs-Server gestartet und automatisch ein Browser-Fenster zur URL des Servers geöffnet. Dieser Server fungiert als Reverse-Proxy für die Edge Delivery Services-Umgebung und stellt Inhalte von dort bereit, während Sie Ihre lokale Codebasis für die Entwicklung verwenden.

```bash
$ cd ~/Code/aem-wknd-eds-ue 
$ aem up

    ___    ________  ___                          __      __ 
   /   |  / ____/  |/  /  _____(_)___ ___  __  __/ /___ _/ /_____  _____
  / /| | / __/ / /|_/ /  / ___/ / __ `__ \/ / / / / __ `/ __/ __ \/ ___/
 / ___ |/ /___/ /  / /  (__  ) / / / / / / /_/ / / /_/ / /_/ /_/ / /
/_/  |_/_____/_/  /_/  /____/_/_/ /_/ /_/\__,_/_/\__,_/\__/\____/_/

info: Starting AEM dev server version x.x.x
info: Local AEM dev server up and running: http://localhost:3000/
info: Enabled reverse proxy to https://main--aem-wknd-eds-ue--<YOUR_ORG>.aem.page
```

Das AEM-CLI öffnet die Website in Ihrem Browser unter `http://localhost:3000/`. Änderungen im Projekt werden automatisch im Webbrowser aktualisiert, während Inhaltsänderungen ([ Veröffentlichung in der Vorschauumgebung erforderlich) ](./6-author-block.md) den Webbrowser aktualisieren.

Wenn die Website mit einer 404-Seite geöffnet wird, ist es wahrscheinlich, dass die [fstab.yaml oder path.](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project), die im [neuen Code-Projekt](./1-new-code-project.md) aktualisiert wurden, falsch konfiguriert sind oder die Änderungen nicht in die `main`-Verzweigung übertragen wurden.

## Erstellen von JSON-Fragmenten

Edge Delivery Services-Projekte, die mit der Boilerplate-XWalk-Vorlage für [AEM erstellt ](https://github.com/adobe-rnd/aem-boilerplate-xwalk), basieren auf JSON-Konfigurationen, die das BlockAuthoring im universellen Editor ermöglichen.

- **JSON-Fragmente** werden mit den zugehörigen Blöcken gespeichert und definieren die Blockmodelle, Definitionen und Filter.
   - **Modellfragmente**: unter `/blocks/example/_example.json` gespeichert.
   - **Definitionsfragmente**: in `/blocks/example/_example.json` gespeichert.
   - **Filterfragmente**: unter `/blocks/example/_example.json` gespeichert.

NPM-Skripte kompilieren diese JSON-Fragmente und platzieren sie an den entsprechenden Stellen im Projektstamm. Verwenden Sie zum Erstellen von JSON-Dateien die bereitgestellten NPM-Skripte. Um beispielsweise alle Fragmente zu kompilieren, führen Sie Folgendes aus:

```bash
# ~/Code/aem-wknd-eds-ue

npm run build:json
```

| NPM-Skript | Beschreibung |
|--------------------|-----------------------------------------------------------------------------|
| `build:json` | Erstellt alle JSON-Dateien aus Fragmenten und fügt sie den entsprechenden `component-*.json` hinzu. |
| `build:json:models` | Erstellt Block-JSON-Fragmente und kompiliert sie in `/component-models.json`. |
| `build:json:definitions` | Erstellt JSON-Fragmente für Seiten und kompiliert sie in `/component-definitions.json`. |
| `build:json:filters` | Erstellt JSON-Fragmente für Seiten und kompiliert sie in `/component-filters.json`. |

>[!TIP]
>
> Führen Sie `npm run build:json` nach allen Änderungen an den Fragmentdateien aus, um die JSON-Hauptdateien neu zu generieren.

## Fußelbildung

Durch das Verknüpfen wird die Code-Qualität und -Konsistenz sichergestellt. Dies ist für Edge Delivery Services-Projekte vor dem Zusammenführen von Änderungen in der `main` erforderlich.

Die NPM-Skripte können über `npm run` ausgeführt werden, zum Beispiel:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

| NPM-Skript | Beschreibung |
|------------------|--------------------------------------------------|
| `lint:js` | Führt JavaScript-Verknüpfungsprüfungen aus. |
| `lint:css` | Führt CSS-Verknüpfungsprüfungen durch. |
| `lint` | Führt sowohl JavaScript- als auch CSS-Verknüpfungsprüfungen aus. |

### Linting-Probleme automatisch beheben

Sie können Verknüpfungsprobleme automatisch beheben, indem Sie die folgenden `scripts` zum `package.json` des Projekts hinzufügen. Sie können über `npm run` ausgeführt werden:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:fix
```

Diese Skripte sind nicht mit der AEM Boilerplate-XWalk-Vorlage vorkonfiguriert, können jedoch zur `package.json`-Datei hinzugefügt werden:

>[!BEGINTABS]

>[!TAB Zusätzliche Skripte]

| NPM-Skript | Befehl | Beschreibung |
|------------------|------------------------------------------------|-------------------------------------------------------|
| `lint:js:fix` | `npm run lint:js -- --fix` | Behebt automatisch Probleme mit JavaScript Linting. |
| `lint:css:fix` | `stylelint blocks/**/*.css styles/*.css -- --fix` | Behebt automatisch CSS-Verknüpfungsprobleme. |
| `lint:fix` | `npm run lint:js:fix && npm run lint:css:fix` | Führt zur schnellen Bereinigung sowohl JS- als auch CSS-Fix-Skripte aus. |

>[!TAB package.json-Beispiel]

Die folgenden Skripteinträge können zum `package.json` `scripts`-Array hinzugefügt werden.

```json
{
  ...
  "scripts": [
    ...,
    "lint:js:fix": "npm run lint:js -- --fix",
    "lint:css:fix": "npm run lint:css -- --fix",
    "lint:fix": "npm run lint:js:fix && npm run lint:css:fix",
    ...
  ]
  ...
}
```

>[!ENDTABS]

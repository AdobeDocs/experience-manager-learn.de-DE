---
title: Einrichten von AEM Agent-Kenntnissen
description: Erfahren Sie, wie Sie AEM Agent Skills für KI-unterstützte Entwicklung einrichten.
feature: Developer Tools
version: Experience Manager as a Cloud Service
role: Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2026-04-24T00:00:00Z
jira: KT-20900
thumbnail: KT-20900.png
exl-id: c92d9124-4b92-4ee1-b04f-b6d1f82d53aa
source-git-commit: f93359e731b6c3fa549e9499ef693042eba3aad7
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 4%

---

# Einrichten von AEM Agent-Kenntnissen

Erfahren Sie, wie Sie AEM Agent Skills für KI-unterstützte Entwicklung einrichten.

Wenn Sie einen Codierungsagenten über eine KI-gestützte IDE auffordern, AEM-Entwicklungsaufgaben zu bearbeiten, kann er **AEM-Agentenkenntnisse** Verfahrensanleitungen von Adobe verwenden, anstatt sich nur auf allgemeine Modellschulungen oder was immer er auch nur aus Ihrem Repository ableiten kann, zu verlassen.

Adobe stellt die Kenntnisse des AEM-Agenten über das Repository [Adobe Skills](https://github.com/adobe/skills) zur Verfügung. Siehe auch [KI-unterstützte Entwicklung](../overview.md) , wie Adobe bei der KI-unterstützten Entwicklung hilft.

In diesem Tutorial installieren Sie die Kenntnisse in einem lokalen Klon des [WKND Sites-Projekts](https://github.com/adobe/aem-guides-wknd). Sie können dieselben Schritte für Ihr eigenes AEM as a Cloud Service-Projekt verwenden.

>[!VIDEO](https://video.tv.adobe.com/v/3484940/?learn=on&enablevpops)

## Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

- Ein lokaler Klon des [WKND Sites-Projekts](https://github.com/adobe/aem-guides-wknd) oder Ihres eigenen AEM as a Cloud Service-Projekts.
- Eine KI-gestützte IDE wie Cursor oder Visual Studio Code mit GitHub Copilot.

## AEM Agent-Kenntnisse installieren

Installieren Sie AEM Agent-Kenntnisse mit dem `npx`-Befehl (erfordert [Node.js](https://nodejs.org/), damit `npx` verfügbar ist). Weitere Installationsoptionen, z. B. Claude-Code-Plug-ins oder die GitHub-CLI-Erweiterung, finden Sie im Abschnitt [Installation](https://github.com/adobe/skills/tree/main#installation) im Repository für Adobe-Kenntnisse.

1. Klonen Sie das [WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd) lokal:

   ```shell
   $ git clone https://github.com/adobe/aem-guides-wknd.git
   ```

1. Öffnen Sie das geklonte Projekt in Ihrer KI-gestützten IDE (z. B. Cursor) und öffnen Sie das integrierte Terminal.
   ![Öffnen Sie das Terminal](../assets/agent-skills/wknd-in-cursor-ide-open-terminal.png)

1. Führen Sie den folgenden Befehl aus, um AEM Agent-Kenntnisse für Cursor hinzuzufügen:

   ```shell
   $ npx skills add https://github.com/adobe/skills/tree/main/plugins/aem/cloud-service --agent cursor
   ```

   Informationen zu anderen Agententypen finden Sie im [Installation](https://github.com/adobe/skills/tree/main#installation) im Adobe Skills Repository.

1. Wählen Sie nach Aufforderung aus, welche AEM Agent-Kenntnisse installiert werden sollen.
   ![Wählen Sie die zu installierenden AEM Agent Skills aus](../assets/agent-skills/select-aem-agent-skills-to-install.png)

   Wählen Sie die Qualifikation **Ensure-agents-md** aus, damit das Installationsprogramm **AGENTS.md**- und **CLAUDE.md**-Dateien im Repository-Stamm erstellen kann. Diese Bootstrap-Kenntnisse untersuchen Ihr Projekt, z. B. die `pom.xml` und Module, und generieren eine maßgeschneiderte Agentenführung.

   Wenn **AGENTS.md** bereits vorhanden ist, wird **nicht**.

1. Wählen Sie den Installationsumfang aus. In dieser exemplarischen Vorgehensweise ist der Umfang **Projekt** typisch, damit Dateien mit Kenntnissen im Repository live sind.
   ![Wählen Sie den Installationsumfang aus](../assets/agent-skills/select-installation-scope.png)

1. Bestätigen Sie die Installation unter `.agents/skills`. Sie sollten **SKILLS.md** und die zugehörigen Referenz- und Asset-Ordner sehen.
   ![Überprüfen der installierten Kenntnisse](../assets/agent-skills/review-installed-skills.png)

1. Wenn Adobe Kenntnisse hinzufügt oder aktualisiert, verwenden Sie die CLI, um diese hinzuzufügen, zu aktualisieren, zu entfernen oder aufzulisten. So zeigen Sie alle Befehle an:

   ```shell
   $ npx skills --help
   ```

   ![Überprüfen Sie die verfügbaren Qualifikationsbefehle](../assets/agent-skills/review-available-skills-commands.png)

## Anwendungsszenarien

<!-- 
CARDS
{target = _self}

* ../use-cases/component-development.md    
    {title = Create AEM Component with AI-assisted development}
    {description = Learn how to use AI-assisted development to develop AEM components.}
    {image = ../assets/component-development/review-generated-code.png}
    {cta = Create AEM Component}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create AEM Component with AI-assisted development">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../use-cases/component-development.md" title="Erstellen einer AEM-Komponente mit KI-unterstützter Entwicklung" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/component-development/review-generated-code.png" alt="Erstellen einer AEM-Komponente mit KI-unterstützter Entwicklung"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../use-cases/component-development.md" target="_self" rel="referrer" title="Erstellen einer AEM-Komponente mit KI-unterstützter Entwicklung">Erstellen der AEM-Komponente mit KI-unterstützter Entwicklung</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie mit KI-unterstützter Entwicklung AEM-Komponenten entwickeln können.</p>
                </div>
                <a href="../use-cases/component-development.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">AEM-Komponente erstellen</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Zusätzliche Ressourcen

- [Lokale Entwicklung mit KI-Tools](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools)

- [Adobe-Kenntnisse für KI-Codierungs-Agenten](https://github.com/adobe/skills)

- [AGENTS.md](https://agents.md/)

- [Agent-Kenntnisse](https://agentskills.io/home)

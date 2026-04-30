---
title: Komponentenentwicklung mit AEM Agent Skills
description: Erfahren Sie, wie Sie mit AEM Agent Skills eine AEM-Komponente als Teil der KI-unterstützten Entwicklung entwickeln können.
version: Experience Manager as a Cloud Service
feature: Developer Tools
role: Developer, Architect
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2026-04-24T00:00:00Z
jira: KT-20901
thumbnail: KT-20901.png
exl-id: bd9b74e8-81ab-4d42-bd0a-5443248b5770
source-git-commit: f93359e731b6c3fa549e9499ef693042eba3aad7
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 2%

---

# Komponentenentwicklung mit AEM Agent Skills

Erfahren Sie, wie Sie mit AEM Agent Skills eine AEM-Komponente als Teil der [KI-unterstützten Entwicklung“ &#x200B;](../overview.md).

In dieser exemplarischen Vorgehensweise verwenden Sie natürliche Sprache in einer KI-gestützten IDE (z. B. Cursor), um eine **Promo-Banner**-Komponente im [WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd) zu entwickeln. Der Codierungsagent wendet die `create-component` AEM Agent Skill an, um die Implementierung zu generieren.

>[!VIDEO](https://video.tv.adobe.com/v/3484952/?learn=on&enablevpops)

## Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

- Eine KI-gestützte IDE wie Cursor oder Visual Studio Code mit GitHub Copilot.
- Ein lokaler Klon des [WKND Sites-Projekts](https://github.com/adobe/aem-guides-wknd), der erstellt und in einer _lokalen AEM SDK_-Instanz bereitgestellt wird.
- _AEM Agent Skills_ in diesem Projekt installiert. Wenn Sie dies noch nicht getan haben, schließen Sie [Einrichten von AEM Agent-Kenntnissen](../setup/agent-skills.md) ab.

## Komponentenbedarf

Nehmen wir an, das WKND-Team möchte ein Promo-Banner auf der Startseite anzeigen, und die Design-Referenz lautet wie folgt:

![Design-Referenz für Promo-Banner](../assets/component-development/promo-banner-design-reference.png)

Autoren müssen in der Lage sein, die Felder _Promo-Bezeichnung_, _CTA-_ und _CTA-_ im Komponentendialogfeld festzulegen.

Die Design-Referenz ist ein Screenshot, der über Wireframe, Mockup oder statische Markup-Erfassung erhalten wird.

## Entwickeln der Komponente

1. Öffnen Sie das WKND-Projekt in Ihrer IDE. Vergewissern Sie sich, dass AEM Agent Skills vorhanden sind (z. B. unter `.agents/skills`), und starten Sie dann einen neuen Agent Chat.
   ![Überprüfen Sie, ob AEM Agent Skills installiert sind](../assets/component-development/verify-aem-agent-skills-installed.png)

1. Geben Sie eine Eingabeaufforderung wie die folgende ein. Hängen Sie den Screenshot zum Komponentendesign (erhalten über Wireframe, Mockup oder statische Markup-Erfassung) an, wenn Ihre IDE Bilder im Chat unterstützt:

   ```text
   Create a WKND Promo Banner Component. Please see attached screenshot for design reference.
   
   Dialog specification are:
   
   1. Promo Label - Textfield, required
   2. CTA Text - Textfield, required
   3. CTA Link - Pathfield, required
   ```

1. Der Codierungsagent verwendet die `create-component` AEM Agent Skill, um die Komponente zu generieren. Überprüfen Sie die vorgeschlagenen HTL-, Sling-Modell-, Dialog-XML- und zugehörigen Dateien.
   ![Überprüfen des generierten Codes](../assets/component-development/review-generated-code.png)

>[!TIP]
>
>Anstatt die Designreferenz als Screenshot bereitzustellen, können Sie auch ein Figma-Design über den [Figma MCP Server](https://www.figma.com/mcp-catalog/) bereitstellen, um die Komponente zu erzeugen. Die `create-component`-Kompetenz unterstützt die [Figma Design Integration](https://github.com/adobe/skills/blob/main/plugins/aem/cloud-service/skills/create-component/references/figma-design-rules.md)


1. Stellen Sie die Komponente in der lokalen AEM-Instanz/SDK bereit.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Platzieren Sie im Authoring das Promo-Banner auf der Startseite und validieren Sie das Verhalten. Verfeinern Sie die Implementierung, wenn sie immer noch von der Entwurfsreferenz abweicht.
   ![Erstellen der Promo-Bannerkomponente](../assets/component-development/author-promo-banner-component.png)

1. Überprüfen Sie die neu erstellte Komponente, indem Sie die Seite veröffentlichen oder Als veröffentlicht anzeigen.
   ![Überprüfen Sie die neu erstellte Komponente](../assets/component-development/review-newly-created-component.png)

Herzlichen Glückwunsch! Sie haben mit AEM Agent Skills im Rahmen der KI-unterstützten Entwicklung erfolgreich eine neue AEM-Komponente erstellt.

## Über einfache Komponenten hinaus

In dieser exemplarischen Vorgehensweise wird eine einfache Komponente verwendet. Die gleiche `create-component`-Qualifikation unterstützt auch umfangreichere Fälle, darunter:

- Felder mit mehreren Feldern und verschachtelten Dialogfeldern
- Erweiterungen der AEM-Kernkomponenten (einschließlich Sling Resource Merger-Mustern)
- Figma-Datei- oder Frame-URLs für Layout und Stil, wenn der Figma MCP-Server (z. B. `plugin-figma-figma`) in Ihrer IDE aktiviert ist

Für Feldtypen, Dialogmuster, Figmaregeln und Beispiele lesen Sie `SKILL.md` in Ihrem installierten Ordner für Kenntnisse, z. B. `.agents/skills/create-component/SKILL.md`.

Einen Überblick, Installationspfade nach IDE und Fehlerbehebung finden Sie unter [AEM Component Development Agent](https://github.com/adobe/skills/blob/main/plugins/aem/cloud-service/skills/create-component/README.md) im Adobe Skills Repository.

## AGENTS.md

Bevor wir abschließen, sollten wir verstehen, wie AGENTS.md im Rahmen der Erstellung der Komponente generiert wurde.

Bei AEM as a Cloud Service-Projekten erstellt die `ensure-agents-md` Bootstrap-Kenntnisse (ausgewählt unter [Einrichten von AEM-](../setup/agent-skills.md)) `AGENTS.md` im Repository-Stamm, wenn sie **fehlt**. Es verwendet, was es aus Ihrem Projekt-Layout lernt.

Eine vorhandene `AGENTS.md`-Datei **nicht**.

![AGENTS.md-Erstellung](../assets/component-development/agents-md-creation.png)

## Zusätzliche Ressourcen

- [Lokale Entwicklung mit KI-Tools](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools)

- [Adobe-Kenntnisse für KI-Codierungs-Agenten](https://github.com/adobe/skills)

- [AGENTS.md](https://agents.md/)

- [Agent-Kenntnisse](https://agentskills.io/home)

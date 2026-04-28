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
source-git-commit: e3ef450cfe9005ba940ff1897c216681654341b3
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 2%

---


# Komponentenentwicklung mit AEM Agent Skills

Erfahren Sie, wie Sie mit AEM Agent Skills eine AEM-Komponente als Teil der [KI-unterstützten Entwicklung“ &#x200B;](../overview.md).

In dieser exemplarischen Vorgehensweise verwenden Sie natürliche Sprache in einer KI-gestützten IDE (z. B. Cursor), um eine **Promo-Banner**-Komponente im [WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd) zu entwickeln. Der Codierungsagent wendet die `create-component` AEM Agent Skill an, um die Implementierung zu generieren.

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

1. The coding agent uses the `create-component` AEM Agent Skill to generate the component. Review the proposed HTL, Sling Model, dialog XML, and related files.
   ![Review the generated code](../assets/component-development/review-generated-code.png)

>[!TIP]
>
>Instead of providing the design reference as a screenshot, you can also provide a Figma design via the [Figma MCP server](https://www.figma.com/mcp-catalog/) to generate the component. The `create-component` skill supports the [Figma design integration](https://github.com/adobe/skills/blob/main/plugins/aem/cloud-service/skills/create-component/references/figma-design-rules.md)


1. Deploy the component to the local AEM instance/SDK.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. In authoring, place the Promo Banner on the home page and validate behavior. Refine the implementation if it still diverges from the design reference.
   ![Author the Promo Banner component](../assets/component-development/author-promo-banner-component.png)

1. Review the newly created component by publishing the page or View as Published.
   ![Review the newly created component](../assets/component-development/review-newly-created-component.png)

Herzlichen Glückwunsch! You have successfully created a new AEM component using AEM Agent Skills as part of AI-assisted development.

## Beyond simple components

This walkthrough uses a simple component. The same `create-component` skill also supports richer cases, including:

- Multifields and nested dialogs fields
- AEM Core Components extensions (including Sling Resource Merger patterns)
- Figma file or frame URLs for layout and styling, when the Figma MCP server (for example `plugin-figma-figma`) is enabled in your IDE

For field types, dialog patterns, Figma rules, and examples, read `SKILL.md` in your installed skill folder, for example, `.agents/skills/create-component/SKILL.md`.

For an overview, installation paths by IDE, and troubleshooting, see [AEM Component Development Agent](https://github.com/adobe/skills/blob/main/plugins/aem/cloud-service/skills/create-component/README.md) in the Adobe Skills repository.

## AGENTS.md

Before we wrap up, let&#39;s understand how AGENTS.md was generated as part of creating the component.

For AEM as a Cloud Service projects, the `ensure-agents-md` bootstrap skill (selected during [Setup AEM Agent Skills](../setup/agent-skills.md)) creates `AGENTS.md` at the repository root when it is **missing**. It uses what it learns from your project layout.

It does **not** overwrite an existing `AGENTS.md` file.

![AGENTS.md-Erstellung](../assets/component-development/agents-md-creation.png)

## Zusätzliche Ressourcen

- [Lokale Entwicklung mit KI-Tools](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools)

- [Adobe-Kenntnisse für KI-Codierungs-Agenten](https://github.com/adobe/skills)

- [AGENTS.md](https://agents.md/)

- [Agent-Kenntnisse](https://agentskills.io/home)

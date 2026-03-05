---
title: MCP-Server in AEM
description: Erfahren Sie, wie Sie die AEM Model Context Protocol (MCP)-Server aus Ihren bevorzugten KI-gestützten IDE- oder Chat-basierten Anwendungen verwenden, um Ihre AEM-Inhaltsarbeit zu optimieren und zu beschleunigen.
version: Experience Manager as a Cloud Service
role: Leader, User, Developer
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2026-03-04T00:00:00Z
jira: KT-20473
source-git-commit: c5f1c7f57181b1e9de6dd91aa2428f2fe1a04893
workflow-type: tm+mt
source-wordcount: '881'
ht-degree: 0%

---

# MCP-Server in AEM

Erfahren Sie, wie Sie die AEM _Model Context Protocol (MCP_-Server) Ihrer bevorzugten KI-gestützten IDE- oder Chat-basierten Anwendungen verwenden, um Ihre AEM-Inhaltsarbeit zu optimieren und zu beschleunigen. Sie beschreiben, was Sie in natürlicher Sprache wünschen, anstatt API-Code auf niedriger Ebene zu schreiben oder durch die AEM-Benutzeroberfläche zu navigieren.

## Liste der AEM MCP-Server

Alle AEM MCP-Server sind unter `https://mcp.adobeaemcloud.com/adobe/mcp/` verfügbar. Weitere Informationen finden [ unter „Verwenden von MCP ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/using-mcp-with-aem-as-a-cloud-service) AEM as a Cloud Service&quot;.

- **Inhalt** (`/content`) - Vollständiger Zugriff zum Erstellen, Lesen, Aktualisieren und Löschen von Seiten, Fragmenten und Assets.
- **Inhalt (schreibgeschützt)** (`/content-readonly`) - Schreibgeschützt, um Seiten, Fragmente und Assets aufzulisten und abzurufen (keine Änderungen).
- **Cloud Manager** (`/cloudmanager`) - Zum Verwalten von Programmen, Umgebungen, Repositorys und Pipelines für Adobe Cloud Manager.

>[!TIP]
>
>Die Tools, die jeder Server bereitstellt, können sich im Laufe der Zeit ändern. Um zu sehen, was jetzt verfügbar ist, bitten Sie Ihre KI, alle AEM-MCP-Tools aufzulisten (z. B. `List all AEM MCP tools available from this server and describe what they do`), oder geben Sie die `tools/list` in Ihrer IDE ein.

## Verwendungsmuster für den MCP-Server

Bevor Sie mit der Verwendung der AEM MCP-Server beginnen, sollten Sie die beiden Hauptverwendungsmuster für die MCP-Server verstehen:

- **Am Menschen orientiert** - Sie sitzen auf dem Fahrersitz. Sie fragen, die KI empfiehlt oder führt Tools für Sie in der IDE aus.
- **Agent** - Eine Agent-Anwendung (Agent oder Subagent) ruft den Server eigenständig auf, wählt Tools aus und arbeitet auf ein Ziel hin, das nur wenig menschliche Eingaben erfordert.

So vergleichen sich diese beiden Nutzungsmuster:

| Aspekt | am Menschen orientiert | Agentin |
| ------ | ------------- | ------- |
| **Wer steuert Aktionen** | Sie. <br> Die KI empfiehlt oder führt Tools für Sie in der IDE oder im Chat-basierten Programm aus. | Die KI. <br> Es wählt die Werkzeuge aus, die verwendet werden sollen, und arbeitet mit minimaler Anleitung. |
| **Entscheidungsbehörde** | Sie behalten die Kontrolle. Sie genehmigen oder Trigger für jeden Schritt. | Die KI hat mehr Freiheit. Für die wirkungsvollen Aktionen sind möglicherweise Schutzmaßnahmen oder Genehmigungen erforderlich. |
| **Typisches Nutzungsmuster** | **Pro-Entwickler** verwenden Sie es von Ihrer eigenen IDE oder Chat-basierten Anwendung, ein Entwickler pro Sitzung, gut für die tägliche Entwicklungsarbeit. | **Shared** über eine Agentenanwendung als freigegebene Services und Gateways für viele Benutzer oder Agenten. |
| **Am besten geeignet für** | Überprüfen von Inhalten, Durchführen geführter Aktualisierungen, Erkunden oder Wiederholen von Aufgaben, während Sie in der Schleife bleiben. | Agent-Workflows, Batch-Vorgänge, Pipelines und Ziele, bei denen das System mit minimalem Eingriff ausgeführt werden soll. |

### Bei Verwendung von MCP in Agentensystemen

MCP-Server sind für **vom Menschen betriebene MCP-Clients** mit interaktiver UX und menschlicher Aufsicht konzipiert. Die MCP-Tools-Spezifikation empfiehlt _einen Menschen in der Schleife_ der Tool-Aufrufe genehmigen oder ablehnen kann.

Wenn Sie MCP-Server in einem agenten oder autonomen System verwenden, behandeln Sie dies als separate Kompatibilitätsstufe. Auf die Zulassungsliste setzen **nicht fest programmieren** Tool-Namen in _Aufforderungen_, __ oder _Routing-Logik_. In MCP ist _Tool-Name_ eine programmgesteuerte Kennung, die _Beschreibung_ ist der modellorientierte Hinweis für das LLM. Funktionen oder Beschreibungen bevorzugen, die auf Eingabeaufforderungen und Auswahl basieren.

Implementieren Sie die Laufzeiterkennung über `tools/list`, verarbeiten Sie Toollistenänderungen (`notifications/tools/list_changed`) und stimmen Sie sich beim Onboarding und Versionieren mit dem MCP-Serveranbieter ab, wenn Sie Stabilitätsgarantien benötigen, die über die Protokollgrundlinie hinausgehen.

## MCP-Entitäten und deren Zuordnung

MCP basiert auf drei Entitäten: **host**, **client** und **server**. Die [MCP-Spezifikation](https://modelcontextprotocol.io/docs/getting-started/intro) definiert sie formal. In der folgenden Tabelle werden jedoch die einzelnen Begriffe und ihre Zuordnung bei der Verwendung von AEM MCP-Servern erläutert.

| Komponente | Standarddefinition | Bei Verwendung von AEM MCP-Servern |
| --------- | ------------------- | ---------------- |
| **Host** | Die App, die alles ausführt, Kontext sammelt, mit der KI kommuniziert, Berechtigungen verarbeitet und Clients erstellt. | Ihre **IDE** (Cursor) oder Chat-basierte Anwendung ist der Host. Es führt den MCP-Client aus und entscheidet, welche Tools und Server Ihre Sitzung verwenden kann. |
| **Client** | Eine einzelne Verbindung vom Host zu einem Server. Er übergibt Nachrichten hin und her und hält den Zugriff dieses Servers von anderen getrennt. | Der **MCP-Client** ist in Ihrer IDE oder Ihrem Chat-basierten Programm verfügbar. Wenn Sie den AEM Content MCP Server in den Einstellungen hinzufügen, erstellt die IDE oder das Chat-basierte Programm einen Client, der mit diesem Server kommuniziert. Ihre Eingabeaufforderungen und Tool-Aufrufe gehen über diesen Client. |
| **Server** | Ein Service, der Tools, Daten und Eingabeaufforderungen über MCP bereitstellt. Es kann auf Ihrem Computer oder remote ausgeführt werden. | Die von Adobe gehosteten **AEM MCP-Server** bieten Tools zum Erstellen, Lesen, Aktualisieren und Löschen von Seiten, Inhaltsfragmenten und Assets, damit die KI in Ihrer IDE oder Ihrem Chat-basierten Programm mit Ihrer AEM-Umgebung zusammenarbeiten kann. |

Einfach ausgedrückt: **Host** ist Ihre IDE- oder Chat-basierte Anwendung, **Client** ist die Verbindung von der IDE oder Chat-basierten Anwendung zu AEM, **Server** sind die von Adobe gehosteten AEM MCP-Server, die die Arbeit tun.

## Einrichtung

AEM MCP-Server sind für die Verwendung mit einer definierten Gruppe von MCP-kompatiblen Anwendungen konzipiert. Die folgenden Anwendungen werden offiziell unterstützt:

- [Anthrope Claude](https://claude.com/product/overview)
- [Cursor](https://www.cursor.com/)
- [OpenAI ChatGPT](https://chatgpt.com/)
- [Microsoft Copilot Studio](https://www.microsoft.com/en-us/microsoft-365-copilot/microsoft-copilot-studio)

Weitere Informationen finden [ unter ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/using-mcp-with-aem-as-a-cloud-service#setup-overview) .

## Anwendungsfälle

<!-- CARDS
{target = _self}

* ./accelerate-content-operations-with-aem-mcp-server.md    
  {title = Accelerate Content Operations with AEM MCP Server}
  {description = Learn how to use the AEM Content MCP Server from Cursor IDE to streamline and accelerate your AEM content work.}
  {image = ../assets/content-mcp-server/update-adventure-price-prompt-response.png}
  {cta = Learn Content MCP Server}

* ./cloud-manager.md
  {title = Cloud Manager MCP Server}
  {description = Learn how to use the AEM Cloud Manager MCP Server from Cursor IDE to streamline and accelerate your AEM cloud manager work.}
  {image = ../assets/cm-mcp-server/start-pipeline.png}
  {cta = Learn Cloud Manager MCP Server}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Accelerate Content Operations with AEM MCP Server">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./accelerate-content-operations-with-aem-mcp-server.md" title="Beschleunigen von Inhaltsvorgängen mit AEM MCP Server" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/content-mcp-server/update-adventure-price-prompt-response.png" alt="Beschleunigen von Inhaltsvorgängen mit AEM MCP Server"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./accelerate-content-operations-with-aem-mcp-server.md" target="_self" rel="referrer" title="Beschleunigen von Inhaltsvorgängen mit AEM MCP Server">Beschleunigen von Inhaltsvorgängen mit dem AEM MCP-Server</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie mit dem AEM Content MCP Server von der Cursor-IDE Ihre AEM-Inhaltsarbeit optimieren und beschleunigen können.</p>
                </div>
                <a href="./accelerate-content-operations-with-aem-mcp-server.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">MCP-Server für Lerninhalte</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Cloud Manager MCP Server">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./cloud-manager.md" title="Cloud Manager MCP-Server" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/cm-mcp-server/start-pipeline.png" alt="Cloud Manager MCP-Server"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./cloud-manager.md" target="_self" rel="referrer" title="Cloud Manager MCP-Server">Cloud Manager MCP-Server</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie mit dem AEM Cloud Manager MCP-Server von der Cursor-IDE Ihre AEM Cloud Manager-Arbeit optimieren und beschleunigen können.</p>
                </div>
                <a href="./cloud-manager.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Cloud Manager MCP-Server kennenlernen</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

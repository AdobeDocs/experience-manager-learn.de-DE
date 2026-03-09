---
title: Beschleunigen von AEM-Inhaltsvorgängen mithilfe des Content MCP-Servers
description: Erfahren Sie, wie Sie den AEM Content MCP-Server aus Ihrer bevorzugten KI-gestützten IDE wie Cursor verwenden, um Ihren AEM-Inhaltsbetrieb zu optimieren und zu beschleunigen, den manuellen Aufwand zu reduzieren und die Produktivität zu steigern.
version: Experience Manager as a Cloud Service
role: Leader, User, Developer
level: Beginner
doc-type: tutorial
duration: null
last-substantial-update: 2026-03-04T00:00:00Z
jira: KT-20474
exl-id: 843209cb-2f31-466c-b5b1-a9fb26965bc0
source-git-commit: 6a0eb6e8f5fa9d7152f46d6b8054dc89ff656507
workflow-type: tm+mt
source-wordcount: '850'
ht-degree: 0%

---

# Beschleunigen von AEM-Inhaltsvorgängen mithilfe des Content MCP-Servers

Verwenden Sie den **Content MCP Server** von einer KI-gestützten IDE wie [Cursor-IDE](https://www.cursor.com/), um mit AEM-Inhalten in natürlicher Sprache zu arbeiten, ohne API-Code auf niedriger Ebene oder Benutzeroberflächennavigation.

In diesem Tutorial _Sie_ Details zu Adventure-Inhaltsfragmenten, _aktualisieren_ ein Fragment (z. B. den Preis eines Abenteuers) und _überprüfen_ die Änderung in der [WKND Adventures React-App](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app) alle von Ihrer IDE aus in einer _unteren AEM-Umgebung_ (RDE oder Entwicklung), ohne den MCP-Fluss verlassen zu müssen.

>[!VIDEO](https://video.tv.adobe.com/v/3480895/?learn=on&enablevpops)

## Überblick

AEM as a Cloud Service stellt _MCP-Server_ bereit, damit Ihre IDE- oder Chat-App sicher mit AEM arbeiten kann. Der **Content MCP Server** unterstützt Seiten, Fragmente und Assets. Weitere Informationen finden [&#x200B; unter „MCP-](./overview.md) in AEM&quot;.

## So können Entwickler es verwenden

Verbinden Sie die [Cursor-IDE](https://www.cursor.com/) mit dem Content MCP-Server und führen Sie das folgende Szenario aus.

### Setup - Content MCP Server im Cursor

Mit diesen Schritten richten wir den Content MCP Server in Cursor ein.

1. Öffnen Sie den Cursor auf Ihrem Computer.

1. Gehen Sie **Cursor** Menü „Einstellungen **> Cursor-Einstellungen**, um das Einstellungsfenster zu öffnen.
   ![Cursoreinstellungen](../assets/content-mcp-server/cursor-settings.png)

1. Klicken Sie in der linken Seitenleiste auf **Tools und MCP**, um dieses Bedienfeld zu öffnen.
   ![Tools und MCP](../assets/content-mcp-server/tools-mcp.png)

1. Klicken Sie **Benutzerdefinierten MCP hinzufügen** oder **Neuer MCP-Server**, um `mcp.json` zu öffnen, und fügen Sie dann diese Konfiguration ein:

   ```json
   {
       "mcpServers": {
           // Use this for create, read, update, and delete operations
           "AEM-RDE-Content": {
               "url": "https://mcp.adobeaemcloud.com/adobe/mcp/content"
           },
           //Use this for read-only operations
           "AEM-RDE-Content-Read-Only": {
               "url": "https://mcp.adobeaemcloud.com/adobe/mcp/content-readonly"
           }
       }
   }
   ```

   >[!CAUTION]
   >
   > Zu Tutorial-Zwecken werden mit der obigen Konfiguration **Inhalt** und **Inhalt (schreibgeschützt)** für dieses Tutorial hinzugefügt. In der Praxis enthält **Inhalt** bereits alles **Inhalt (schreibgeschützt)** Angebote sowie Tools zum Erstellen/Aktualisieren/Löschen.
   >
   >
   > Wenn Sie keine Möglichkeit zum Erstellen, Ändern oder Löschen von Inhalten haben möchten, konfigurieren Sie nur **Inhalt (schreibgeschützt)** (`/content-readonly`) und lassen Sie **Inhalt** (`/content`) aus. Auf diese Weise vermeiden Sie versehentliche Änderungen.

   ![AEM MCP-Server hinzufügen](../assets/content-mcp-server/mcp-json-file.png)

1. Klicken Sie im Fenster Cursor-Einstellungen auf **Verbinden**, um den Authentifizierungsprozess einzuleiten. Der OAuth 2.0 PKCE-Fluss wird verwendet, um das **benutzerspezifische Zugriffstoken) für** Zugriff auf den AEM MCP-Server abzurufen.
   ![Verbindung zum AEM MCP-Server herstellen](../assets/content-mcp-server/connect-to-aem-mcp-server.png)

1. Melden Sie sich mit Ihrer Adobe ID an und kehren Sie dann zurück zum Fenster „Cursor-Einstellungen“.
   ![Mit Adobe ID anmelden](../assets/content-mcp-server/login-with-adobe-id.png)

1. Bestätigen Sie, dass **AEM-RDE-Content-Read-Only** und **AEM-RDE-Content** als verbunden angezeigt werden. Sie können jeden Server erweitern, um die Tools anzuzeigen.

   ![AEM MCP-Server](../assets/content-mcp-server/connected-aem-mcp-servers.png)

### Einrichtung - WKND Adventures React App

Richten Sie als Nächstes die [WKND Adventures React App](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app) in Cursor ein.

1. Klonen Sie diese beiden Repos auf Ihrem Computer:

   ```bash
   ## WKND GraphQL repo, the `react-app` folder is the WKND Adventures app
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   
   ## WKND Site repo, you deploy this to RDE so the app can use its content fragments data via GraphQL
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Stellen Sie das [WKND Site](https://github.com/adobe/aem-guides-wknd)-Projekt in Ihrer RDE bereit. Ausführliche Anweisungen finden Sie unter [Verwenden der schnellen Entwicklungsumgebung](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use#deploy-aem-artifacts-using-the-aem-rde-plugin).

1. Öffnen Sie den `react-app` Ordner in Ihrer IDE.

1. `.env.development` bearbeiten und festlegen:
   - `REACT_APP_HOST_URI`: Ihre RDE-Autoren-URL
   - `REACT_APP_AUTH_METHOD`: zu `basic`
   - `REACT_APP_BASIC_AUTH_USER` und `REACT_APP_AEM_AUTH_PASSWORD`: zu `aem-headless` (erstellen Sie diesen Benutzer in der RDE und fügen Sie ihn der `administrators` hinzu)

1. Führen Sie am IDE-Terminal Folgendes aus:

   ```bash
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Navigieren Sie in Ihrem Browser zu [http://localhost:3000](http://localhost:3000), um die WKND Adventures-App anzuzeigen.

   ![React-App - WKND-Adventures](../assets/content-mcp-server/react-app-wknd-adventures.png)

### Produktivitätsszenario - Überprüfung und Aktualisierung des AEM-Inhalts

Angenommen, Sie müssen auf Adventure _Karten ein „HOT DEAL_-Banner anzeigen, wenn eine einfache Regel erfüllt ist. Der übliche Ansatz wäre:

- Sehen Sie sich den Code der Adventure-Kartenkomponente an
- Hinzufügen der Logik für die Anzeige des Banners
- Überprüfen des Adventure-Inhaltsfragmentmodells in AEM
- Ändern Sie eine oder mehrere Adventure-Fragmenteigenschaften, um die Regel zu testen

Um die Dinge einfach zu halten, zeigen wir das Banner _HOT DEAL_, wenn der Preis des Abenteuers unter 100 $ liegt.

Da die React-App ihre Daten aus Ihrer RDE-Umgebung erhält, müssen Sie das Adventure-Inhaltsfragmentmodell kennen und dann die richtigen Fragmenteigenschaften aktualisieren. Genau dabei kann der AEM Content MCP Server helfen. So geht&#39;s.

1. Öffnen Sie in Cursor einen neuen Chat und geben Sie Folgendes ein:

   ```text
   I want to review my Content Fragment Models from AEM RDE, can you list the Adventure Content Fragment details.
   ```

   ![Inhaltsfragmentmodelle überprüfen](../assets/content-mcp-server/review-content-fragment-models-prompt-response.png)


   Vor dem Aufrufen des Content MCP Servers wird um Bestätigung gebeten, um fortzufahren. Auf diese Weise behalten Sie die Kontrolle über die Inhaltsvorgänge.

   Die KI verwendet den Content MCP Server, um die Daten abzurufen und sie dann auf klare, strukturierte Weise darzustellen. Sie enthält Details zum Inhaltsfragmentmodell, die Anzahl der Fragmente und Zusammenfassungsinformationen.

1. Um das Banner _HOT DEAL_ Trigger, aktualisieren Sie den Preis eines Abenteuers. Im selben Chat versuchen Sie Folgendes:

   ```text
   Can you update adventure Beervana in Portland's price to 99.99
   ```

   ![Adventure-Preis aktualisieren](../assets/content-mcp-server/update-adventure-price-prompt-response.png)

   In ähnlicher Weise bittet die KI um Bestätigung, bevor der Inhalt aktualisiert wird. Außerdem wird der Inhaltsvorgang vor und nach der Aktualisierung zusammengefasst.

1. Bestätigen Sie in der React-App, dass auf der Beervana-Karte jetzt das Banner _HOT DEAL_ angezeigt wird.

   ![Überprüfen des HOT DEAL-Banners](../assets/content-mcp-server/verify-hot-deal-banner.png)

### Zusätzliche Eingabeaufforderungen

Probieren Sie diese inhaltsorientierten Eingabeaufforderungen in Ihrer IDE (mit angeschlossenem Content MCP-Server) aus, um weitere Workflows und Funktionen zu erkunden.

- Entdecken Sie Inhalte:

  ```text
  List all content fragments in the WKND Adventures folder
  
  List all WKND Site pages from US English site
  
  Can you give me page metadata for Tahoe Skiing English page? 
  
  List assets of Bali Surf camp
  
  What Content Fragment models are available in this environment?
  ```

- Suchen nach Inhalten:

  ```text
  Search for content fragments that mention 'cycling'
  
  Do we have a magazine page in US English site with "Camping" in it
  ```

- Inhalt aktualisieren:

  ```text
  In WKND US English create a copy of Downhill Skiing Wyoming as "Test Downhill Skiing Wyoming"
  
  In newly created "Test Downhill Skiing Wyoming" please change title to "Duplicated Page"
  ```

- Veröffentlichen oder Veröffentlichung aufheben:

  ```text
  Can you publish the page at /us/en/adventures/test-downhill-skiing-wyoming and give me publish page URL
  
  Can you unpublish the test-downhill-skiing-wyoming page
  ```

## Zusammenfassung

Sie richten den AEM Content MCP Server in Cursor ein und verbinden ihn mit Ihrer RDE (oder Entwicklungsumgebung). Anschließend haben Sie die WKND Adventures React-App verwendet und in natürlicher Sprache gechattet, um Details zu Adventure-Inhaltsfragmenten zu überprüfen. Sie haben auch den Preis eines Fragments aktualisiert, indem Sie vor jedem Inhaltsvorgang von der KI nach Ihrer Bestätigung gefragt wurden. Sie haben die Änderung in der laufenden App überprüft. Sie können denselben am Menschen orientierten Fluss von Ihrer IDE zur Überprüfung, Aktualisierung und Erstellung von AEM-Inhalten verwenden, ohne zur AEM-Benutzeroberfläche zu wechseln oder API-Code auf niedriger Ebene zu schreiben.

---
title: Projekt erstellen | Erste Schritte mit dem AEM SPA Editor und React
description: Erfahren Sie, wie Sie ein Adobe Experience Manager-Maven-Projekt (AEM) als Ausgangspunkt für eine mit dem AEM SPA Editor integrierte React-Anwendung erstellen.
sub-product: sites
feature: SPA Editor, AEM Project Archetype
version: Cloud Service
kt: 413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 57c8fc16-fed5-4af4-b98b-5c3f0350b240
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 3%

---

# Projekt erstellen {#spa-editor-project}

Erfahren Sie, wie Sie ein Adobe Experience Manager-Maven-Projekt (AEM) als Ausgangspunkt für eine mit dem AEM SPA Editor integrierte React-Anwendung erstellen.

## Ziele

1. Generieren Sie ein Projekt, für das der SPA Editor aktiviert ist, mithilfe des Projektarchetyps AEM.
2. Stellen Sie das Starterprojekt auf einer lokalen Instanz von AEM bereit.

## Was Sie erstellen werden {#what-build}

In diesem Kapitel wird ein neues AEM-Projekt generiert, das auf dem [AEM Projektarchetyp](https://github.com/adobe/aem-project-archetype) basiert. Das AEM Projekt wird mit einem sehr einfachen Ausgangspunkt für die React-SPA bootstrapping durchgeführt.

**Was ist ein Maven-Projekt?** -  [Apache ](https://maven.apache.org/) Mavenis ist ein Software-Management-Tool zum Erstellen von Projekten. *Alle Adobe Experience* Manager-Implementierungen verwenden Maven-Projekte zum Erstellen, Verwalten und Bereitstellen von benutzerdefiniertem Code zusätzlich zu AEM.

**Was ist ein Maven-Archetyp?** - Ein  [Maven-](https://maven.apache.org/archetype/index.html) Archetyp ist eine Vorlage oder ein Muster zum Generieren neuer Projekte. Der Archetyp AEM Projekts ermöglicht es uns, ein neues Projekt mit einem benutzerdefinierten Namespace zu generieren und eine Projektstruktur einzufügen, die Best Practices einhält und unser Projekt erheblich beschleunigt.

## Voraussetzungen

Überprüfen Sie die erforderlichen Tools und Anweisungen zum Einrichten einer [lokalen Entwicklungsumgebung](overview.md#local-dev-environment). Stellen Sie sicher, dass eine neue Instanz von Adobe Experience Manager, die im Modus **author** gestartet wurde, lokal ausgeführt wird.

## Projekt erstellen {#create}

>[!NOTE]
>
>In diesem Tutorial wird die Version **27** des Archetyps verwendet. Es empfiehlt sich immer, die **neueste** Version des Archetyps zu verwenden, um ein neues Projekt zu erstellen.

1. Öffnen Sie ein Befehlszeilen-Terminal und geben Sie den folgenden Maven-Befehl ein:

   ```shell
   mvn -B archetype:generate \
    -D archetypeGroupId=com.adobe.aem \
    -D archetypeArtifactId=aem-project-archetype \
    -D archetypeVersion=27 \
    -D appTitle="WKND SPA React" \
    -D appId="wknd-spa-react" \
    -D artifactId="aem-guides-wknd-spa.react" \
    -D groupId="com.adobe.aem.guides.wkndspa.react" \
    -D frontendModule="react" \
    -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > Bei der Zielgruppenbestimmung AEM 6.5.5+ `aemVersion="cloud"` durch `aemVersion="6.5.5"` ersetzen. Verwenden Sie bei der Zielgruppenbestimmung ab Version 6.4.8 `aemVersion="6.4.8"`.

   Beachten Sie die Eigenschaft `frontendModule=react` . Dies weist den AEM Projektarchetyp an, das Projekt mit einem Starter [React code base](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) zu bootstrapping durchzuführen, der mit dem AEM SPA Editor verwendet werden soll. Eigenschaften wie `appTitle`, `appId`, `artifactId` und `groupId` werden verwendet, um das Projekt und den Zweck zu identifizieren.

   Eine vollständige Liste der verfügbaren Eigenschaften zum Konfigurieren eines Projekts [finden Sie hier](https://github.com/adobe/aem-project-archetype#available-properties).

1. Die folgende Ordner- und Dateistruktur wird vom Maven-Archetyp auf Ihrem lokalen Dateisystem generiert:

   ```plain
   |--- aem-guides-wknd-spa.react/
       |--- all/
       |--- analyse/
       |--- core/
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.config/
       |--- ui.content/
       |--- ui.frontend/
       |--- ui.tests /
       |--- it.tests/
       |--- dispatcher/
       |--- analyse/
       |--- pom.xml
       |--- README.md
       |--- .gitignore
   ```

   Jeder Ordner stellt ein einzelnes Maven-Modul dar. In diesem Tutorial arbeiten wir hauptsächlich mit dem `ui.frontend`-Modul, der React-App. Weitere Informationen zu einzelnen Modulen finden Sie in der [AEM Dokumentation zum Projektarchetyp](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html).

## Projekt bereitstellen und erstellen

Kompilieren, erstellen und stellen Sie anschließend mithilfe von Maven den Projektcode auf einer lokalen Instanz von AEM bereit.

1. Stellen Sie sicher, dass eine Instanz von AEM lokal am Port **4502** ausgeführt wird.
1. Navigieren Sie über die Befehlszeile zum Projektverzeichnis `aem-guides-wknd-spa.react`.

   ```shell
   $ cd aem-guides-wknd-spa.react
   ```

1. Führen Sie den folgenden Befehl aus, um das gesamte Projekt zu erstellen und AEM bereitzustellen:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Der Build dauert etwa eine Minute und sollte mit der folgenden Meldung enden:

   ```shell
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd-spa.react 1.0.0-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd-spa.react .......................... SUCCESS [  0.257 s]
   [INFO] WKND SPA React - Core .............................. SUCCESS [ 12.553 s]
   [INFO] WKND SPA React - UI Frontend ....................... SUCCESS [01:46 min]
   [INFO] WKND SPA React - Repository Structure Package ...... SUCCESS [  1.082 s]
   [INFO] WKND SPA React - UI apps ........................... SUCCESS [  8.237 s]
   [INFO] WKND SPA React - UI content ........................ SUCCESS [  5.633 s]
   [INFO] WKND SPA React - UI config ......................... SUCCESS [  0.234 s]
   [INFO] WKND SPA React - All ............................... SUCCESS [  0.643 s]
   [INFO] WKND SPA React - Integration Tests ................. SUCCESS [ 12.377 s]
   [INFO] WKND SPA React - Dispatcher ........................ SUCCESS [  0.066 s]
   [INFO] WKND SPA React - UI Tests .......................... SUCCESS [  0.074 s]
   [INFO] WKND SPA React - Project Analyser .................. SUCCESS [ 31.287 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   Das Maven-Profil `autoInstallSinglePackage` kompiliert die einzelnen Module des Projekts und stellt ein einzelnes Paket in der AEM-Instanz bereit. Standardmäßig wird dieses Paket auf einer AEM-Instanz bereitgestellt, die lokal auf Port **4502** und mit den Anmeldedaten von `admin:admin` ausgeführt wird.

1. Navigieren Sie in Ihrer lokalen AEM-Instanz zu **Package Manager**: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

1. Es sollten mehrere Pakete mit dem Präfix `aem-guides-wknd-spa.react` angezeigt werden.

   ![WKND SPA Packages](assets/create-project/package-manager.png)

   *AEM Package Manager*

   Der gesamte für das Projekt benötigte benutzerspezifische Code wird in diesen Paketen gebündelt und in der AEM-Umgebung installiert.

## Autoreninhalt

Öffnen Sie als Nächstes die SPA, die vom Archetyp generiert wurde, und aktualisieren Sie einige Inhalte.

1. Navigieren Sie zur Konsole **Sites**: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   Die WKND-SPA enthält eine grundlegende Site-Struktur mit einem Land, einer Sprache und einer Homepage. Diese Hierarchie basiert auf den Standardwerten des Archetyps für `language_country` und `isSingleCountryWebsite`. Diese Werte können überschrieben werden, indem die [verfügbaren Eigenschaften](https://github.com/adobe/aem-project-archetype#available-properties) beim Generieren eines Projekts aktualisiert werden.

2. Öffnen Sie die Seite **us** > **en** > **WKND SPA React Home Page** , indem Sie die Seite auswählen und auf die Schaltfläche **Bearbeiten** in der Menüleiste klicken:

   ![Site-Konsole](./assets/create-project/open-home-page.png)

3. Eine Komponente **Text** wurde der Seite bereits hinzugefügt. Sie können diese Komponente wie jede andere Komponente in AEM bearbeiten.

   ![Textkomponente aktualisieren](./assets/create-project/update-text-component.gif)

4. Fügen Sie der Seite eine zusätzliche Komponente **Text** hinzu.

   Beachten Sie, dass das Authoring-Erlebnis dem herkömmlichen AEM Sites-Seitenerlebnis ähnelt. Derzeit ist eine begrenzte Anzahl von Komponenten verfügbar, die verwendet werden können. Im Laufe des Tutorials werden weitere Informationen hinzugefügt.

## Inspect für Einzelseiten-Apps

Überprüfen Sie anschließend mithilfe der Entwicklertools Ihres Browsers, ob es sich um eine Einzelseiten-App handelt.

1. Klicken Sie im **Seiten-Editor** auf die Schaltfläche **Seiteninformationen** > **Als veröffentlicht anzeigen**:

   ![Schaltfläche &quot;Als veröffentlicht anzeigen&quot;](./assets/create-project/view-as-published.png)

   Dadurch wird eine neue Registerkarte mit dem Abfrageparameter `?wcmmode=disabled` geöffnet, wodurch der AEM Editor effektiv deaktiviert wird: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. Zeigen Sie die Quelle der Seite an und beachten Sie, dass der Textinhalt **[!DNL Hello World]** oder ein anderer Inhalt nicht gefunden wurde. Stattdessen sollte HTML wie folgt angezeigt werden:

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.lc-xxxx-lc.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` ist die React-SPA, die auf die Seite geladen wird und für die Wiedergabe des Inhalts verantwortlich ist.

   *Woher kommt der Inhalt?*

3. Kehren Sie zur Registerkarte zurück: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. Öffnen Sie die Entwicklertools des Browsers und überprüfen Sie den Netzwerk-Traffic der Seite während einer Aktualisierung. Zeigen Sie die Anforderungen **XHR** an:

   ![XHR-Anfragen](./assets/create-project/xhr-requests.png)

   Es sollte eine Anfrage an [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) geben. Dies enthält den gesamten Inhalt, formatiert in JSON, der die SPA steuert.

5. Öffnen Sie in einer neuen Registerkarte [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   Die Anfrage `en.model.json` stellt das Inhaltsmodell dar, das die Anwendung steuern wird. Inspect die JSON-Ausgabe und Sie sollten in der Lage sein, den Codeausschnitt zu finden, der die **[!UICONTROL Text]**-Komponente(n) darstellt.

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
   }
   ...
   ```

   Im nächsten Kapitel werden wir untersuchen, wie dieser JSON-Inhalt von AEM Komponenten zu SPA Komponenten zugeordnet wird, um die Grundlage für das AEM SPA Editor zu bilden.

   >[!NOTE]
   >
   > Es kann hilfreich sein, eine Browsererweiterung zu installieren, um die JSON-Ausgabe automatisch zu formatieren.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben gerade Ihr erstes AEM SPA Editor Projekt erstellt!

Die SPA ist ganz einfach. In den nächsten Kapiteln werden weitere Funktionen hinzugefügt.

### Nächste Schritte {#next-steps}

[Integrieren eines SPA](integrate-spa.md)  - Erfahren Sie, wie der SPA-Quellcode in das AEM-Projekt integriert ist, und lernen Sie die Tools kennen, die zur schnellen Entwicklung des SPA verfügbar sind.

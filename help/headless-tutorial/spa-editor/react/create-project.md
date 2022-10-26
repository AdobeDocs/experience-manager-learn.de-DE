---
title: Projekt erstellen | Erste Schritte mit dem AEM SPA Editor und React
description: Erfahren Sie, wie Sie ein Adobe Experience Manager-Maven-Projekt (AEM) als Ausgangspunkt für eine mit dem AEM SPA Editor integrierte React-Anwendung erstellen.
feature: SPA Editor, AEM Project Archetype
version: Cloud Service
kt: 413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 57c8fc16-fed5-4af4-b98b-5c3f0350b240
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1082'
ht-degree: 3%

---

# Projekt erstellen {#spa-editor-project}

Erfahren Sie, wie Sie ein Adobe Experience Manager-Maven-Projekt (AEM) als Ausgangspunkt für eine mit dem AEM SPA Editor integrierte React-Anwendung erstellen.

## Ziel

1. Generieren Sie ein Projekt, für das der SPA Editor aktiviert ist, mithilfe des Projektarchetyps AEM.
2. Stellen Sie das Starterprojekt auf einer lokalen Instanz von AEM bereit.

## Was Sie erstellen werden {#what-build}

In diesem Kapitel wird ein neues AEM-Projekt basierend auf der Variablen [AEM Projektarchetyp](https://github.com/adobe/aem-project-archetype). Das AEM Projekt ist mit einem sehr einfachen Ausgangspunkt für die React-SPA bootstrasiert.

**Was ist ein Maven-Projekt?** - [Apache Maven](https://maven.apache.org/) ist ein Software-Management-Tool zum Erstellen von Projekten. *Alle Adobe Experience Manager* -Implementierungen verwenden Maven-Projekte zum Erstellen, Verwalten und Bereitstellen von benutzerdefiniertem Code zusätzlich zu AEM.

**Was ist ein Maven-Archetyp?** - A [Maven-Archetyp](https://maven.apache.org/archetype/index.html) ist eine Vorlage oder ein Muster zum Generieren neuer Projekte. Der Archetyp AEM Projekts ermöglicht es uns, ein neues Projekt mit einem benutzerdefinierten Namespace zu generieren und eine Projektstruktur einzufügen, die Best Practices einhält und unser Projekt erheblich beschleunigt.

## Voraussetzungen

Überprüfen Sie die erforderlichen Tools und Anweisungen zum Einrichten eines [lokale Entwicklungsumgebung](overview.md#local-dev-environment). Stellen Sie sicher, dass eine neue Instanz von Adobe Experience Manager in **author** -Modus, wird lokal ausgeführt.

## Projekt erstellen {#create}

>[!NOTE]
>
>Dieses Tutorial verwendet Version **39** des Archetyps. Es empfiehlt sich immer, die **latest** Version des Archetyps, um ein neues Projekt zu generieren.

1. Öffnen Sie ein Befehlszeilen-Terminal und geben Sie den folgenden Maven-Befehl ein:

   ```shell
   mvn -B archetype:generate \
    -D archetypeGroupId=com.adobe.aem \
    -D archetypeArtifactId=aem-project-archetype \
    -D archetypeVersion=39 \
    -D appTitle="WKND SPA React" \
    -D appId="wknd-spa-react" \
    -D artifactId="aem-guides-wknd-spa.react" \
    -D groupId="com.adobe.aem.guides.wkndspa.react" \
    -D frontendModule="react" \
    -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > Wenn AEM 6.5.5 oder höher ausgewählt ist, ersetzen Sie `aemVersion="cloud"` mit `aemVersion="6.5.5"`. Verwenden Sie bei der Zielgruppenbestimmung ab Version 6.4.8 `aemVersion="6.4.8"`.

   Beachten Sie die `frontendModule=react` -Eigenschaft. Dadurch wird der AEM Projektarchetyp angewiesen, das Projekt mit einem Starter zu bootstrapping durchzuführen [React-Code-Basis](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) für die Verwendung mit dem AEM SPA Editor. Eigenschaften wie `appTitle`, `appId`, `artifactId`und `groupId` werden verwendet, um das Projekt und den Zweck zu identifizieren.

   Eine vollständige Liste der verfügbaren Eigenschaften zum Konfigurieren eines Projekts [finden Sie hier .](https://github.com/adobe/aem-project-archetype#available-properties).

1. Die folgende Ordner- und Dateistruktur wird vom Maven-Archetyp in Ihrem lokalen Dateisystem generiert:

   ```plain
   |--- aem-guides-wknd-spa.react/
       |--- LICENSE
       |--- README.md
       |--- all/
       |--- archetype.properties
       |--- core/
       |--- dispatcher/
       |--- it.tests/
       |--- pom.xml
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.config/
       |--- ui.content/
       |--- ui.frontend/
       |--- ui.tests /
       |--- .gitignore
   ```

   Jeder Ordner stellt ein einzelnes Maven-Modul dar. In diesem Tutorial arbeiten wir hauptsächlich mit dem `ui.frontend` -Modul, das die React-App ist. Weitere Informationen zu den einzelnen Modulen finden Sie im [Dokumentation zum AEM Projektarchetyp](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html).

## Projekt bereitstellen und erstellen

Kompilieren, erstellen und stellen Sie anschließend mithilfe von Maven den Projektcode auf einer lokalen Instanz von AEM bereit.

1. Stellen Sie sicher, dass eine Instanz von AEM lokal am Port ausgeführt wird. **4502**.
1. Navigieren Sie in der Befehlszeile zum `aem-guides-wknd-spa.react` Projektverzeichnis.

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

   Das Maven-Profil `autoInstallSinglePackage` kompiliert die einzelnen Module des Projekts und stellt ein einzelnes Paket in der AEM-Instanz bereit. Standardmäßig wird dieses Paket in einer AEM-Instanz bereitgestellt, die lokal am Port ausgeführt wird **4502** und mit den Anmeldeinformationen von `admin:admin`.

1. Navigieren Sie zu **Package Manager** in Ihrer lokalen AEM-Instanz: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

1. Es sollten mehrere Pakete mit dem Präfix `aem-guides-wknd-spa.react`.

   ![WKND SPA Packages](assets/create-project/package-manager.png)

   *AEM Package Manager*

   Der gesamte für das Projekt benötigte benutzerspezifische Code wird in diesen Paketen gebündelt und in der AEM-Umgebung installiert.

## Autoreninhalt

Öffnen Sie als Nächstes die SPA, die vom Archetyp generiert wurde, und aktualisieren Sie einige Inhalte.

1. Navigieren Sie zum **Sites** console: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   Die WKND-SPA enthält eine grundlegende Site-Struktur mit einem Land, einer Sprache und einer Homepage. Diese Hierarchie basiert auf den Standardwerten des Archetyps für `language_country` und `isSingleCountryWebsite`. Diese Werte können überschrieben werden, indem die [Verfügbare Eigenschaften](https://github.com/adobe/aem-project-archetype#available-properties) beim Generieren eines Projekts.

2. Öffnen Sie die **us** > **en** > **WKND SPA React-Homepage** Seite durch Auswahl der Seite und Klicken auf **Bearbeiten** in der Menüleiste:

   ![Site-Konsole](./assets/create-project/open-home-page.png)

3. A **Text** wurde der Seite bereits hinzugefügt. Sie können diese Komponente wie jede andere Komponente in AEM bearbeiten.

   ![Textkomponente aktualisieren](./assets/create-project/update-text-component.gif)

4. Hinzufügen eines zusätzlichen **Text** -Komponente auf der Seite.

   Beachten Sie, dass das Authoring-Erlebnis dem herkömmlichen AEM Sites-Seitenerlebnis ähnelt. Derzeit ist eine begrenzte Anzahl von Komponenten verfügbar, die verwendet werden können. Im Laufe des Tutorials werden weitere Informationen hinzugefügt.

## Inspect für Einzelseiten-Apps

Überprüfen Sie anschließend mithilfe der Entwicklertools Ihres Browsers, ob es sich um eine Einzelseiten-App handelt.

1. Im **Seiten-Editor**, klicken Sie auf die **Seiteninformationen** button > **Als veröffentlicht anzeigen**:

   ![Schaltfläche &quot;Als veröffentlicht anzeigen&quot;](./assets/create-project/view-as-published.png)

   Dadurch wird eine neue Registerkarte mit dem Abfrageparameter geöffnet. `?wcmmode=disabled` , wodurch der AEM Editor effektiv deaktiviert wird: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. Zeigen Sie die Quelle der Seite an und beachten Sie, dass der Textinhalt **[!DNL Hello World]** oder der andere Inhalt nicht gefunden wird. Stattdessen sollten Sie HTML wie folgt sehen:

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

   Allerdings *Woher stammt der Inhalt?*

3. Kehren Sie zur Registerkarte zurück: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. Öffnen Sie die Entwicklertools des Browsers und überprüfen Sie den Netzwerk-Traffic der Seite während einer Aktualisierung. Anzeigen der **XHR** Anforderungen:

   ![XHR-Anfragen](./assets/create-project/xhr-requests.png)

   Es sollte eine Anfrage an [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Dies enthält den gesamten Inhalt, formatiert in JSON, der die SPA steuert.

5. Öffnen Sie in einer neuen Registerkarte [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   Die Anfrage `en.model.json` stellt das Inhaltsmodell dar, das die Anwendung steuern wird. Inspect die JSON-Ausgabe und Sie sollten den Codeausschnitt finden können, der die **[!UICONTROL Text]** Komponenten.

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

[SPA integrieren](integrate-spa.md) - Erfahren Sie, wie der SPA Quellcode in das AEM Projekt integriert ist, und lernen Sie die verfügbaren Tools kennen, um die SPA schnell zu entwickeln.

---
title: SPA Editor-Projekt | Erste Schritte mit dem AEM SPA Editor und Angular
description: Erfahren Sie, wie Sie ein Adobe Experience Manager-Maven-Projekt (AEM) als Ausgangspunkt für eine mit dem AEM SPA Editor integrierte Angular-Anwendung verwenden.
sub-product: Sites
feature: SPA Editor, AEM Projektarchetyp
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5309
thumbnail: 5309-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 375df47a13b1820911a7ceb73af0dad15c68740e
workflow-type: tm+mt
source-wordcount: '1115'
ht-degree: 3%

---


# SPA Editor-Projekt {#create-project}

Erfahren Sie, wie Sie ein Adobe Experience Manager-Maven-Projekt (AEM) als Ausgangspunkt für eine mit dem AEM SPA Editor integrierte Angular-Anwendung verwenden.

## Vorgabe

1. Machen Sie sich mit der Struktur eines neuen AEM SPA Editor-Projekts vertraut, das aus einem Maven-Archetyp erstellt wurde.
2. Stellen Sie das Starterprojekt auf einer lokalen Instanz von AEM bereit.

## Was Sie erstellen werden

In diesem Kapitel wird ein neues AEM-Projekt bereitgestellt, das auf dem [AEM Projektarchetyp](https://github.com/adobe/aem-project-archetype) basiert. Das AEM Projekt wird mit einem sehr einfachen Ausgangspunkt für die Angular-SPA bootstrapping durchgeführt. Das in diesem Kapitel verwendete Projekt wird als Grundlage für die Umsetzung des WKND-SPA dienen und in künftigen Kapiteln aufbauen.

![WKND SPA Angular Starter Project](./assets/create-project/what-you-will-build.png)

*Eine klassische Nachricht vom Typ Hello World .*

## Voraussetzungen

Überprüfen Sie die erforderlichen Tools und Anweisungen zum Einrichten einer [lokalen Entwicklungsumgebung](overview.md#local-dev-environment). Stellen Sie sicher, dass eine neue Instanz von Adobe Experience Manager, die im Modus **author** gestartet wurde, lokal ausgeführt wird.

## Projekt abrufen

Es gibt mehrere Möglichkeiten, ein Maven-Projekt mit mehreren Modulen für AEM zu erstellen. In diesem Tutorial wurde der neueste [AEM Projektarchetyp](https://github.com/adobe/aem-project-archetype) als Grundlage für den Tutorial-Code verwendet. Der Projektcode wurde geändert, um mehrere Versionen von AEM zu unterstützen. Lesen Sie [den Hinweis zur Abwärtskompatibilität](overview.md#compatibility).

>[!CAUTION]
>
>Es empfiehlt sich, die **neueste** Version von [archetype](https://github.com/adobe/aem-project-archetype) zu verwenden, um ein neues Projekt für eine reale Implementierung zu erstellen. AEM Projekte sollten eine einzelne Version von AEM mit der Eigenschaft `aemVersion` des Archetyps abrufen.

1. Laden Sie den Ausgangspunkt für dieses Tutorial über Git herunter:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/create-project-start
   ```

2. Die folgende Ordner- und Dateistruktur stellt das AEM Projekt dar, das vom Maven-Archetyp im lokalen Dateisystem generiert wurde:

   ```plain
   |--- aem-guides-wknd-spa
       |--- all/
       |--- core/
       |--- dispatcher/
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.content/
       |--- ui.frontend /
       |--- it.tests/
       |--- pom.xml
       |--- README.md
       |--- .gitignore
       |--- archetype.properties
   ```

3. Die folgenden Eigenschaften wurden beim Generieren des AEM-Projekts aus dem [AEM Projektarchetyp](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14) verwendet:

   | Eigenschaft | Wert |
   |-----------------|---------------------------------------|
   | aemVersion | Wolke |
   | appTitle | WKND SPA Angular |
   | appId | wknd-spa-angular |
   | groupId | com.adobe.aem.guides |
   | frontendModule | angular |
   | package | com.adobe.aem.guides.wknd.spa.angular |
   | includeExample | n |

   >[!NOTE]
   >
   > Beachten Sie die Eigenschaft `frontendModule=angular` . Dies weist den AEM Projektarchetyp an, das Projekt mit einem Starter [Angular-Code-Base](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html) zu bootstrapping durchzuführen, der mit dem AEM SPA Editor verwendet werden soll.

## Projekt erstellen

Kompilieren, erstellen und stellen Sie anschließend mithilfe von Maven den Projektcode auf einer lokalen Instanz von AEM bereit.

1. Stellen Sie sicher, dass eine Instanz von AEM lokal am Port **4502** ausgeführt wird.
2. Überprüfen Sie im Befehlszeilenterminal, ob Maven installiert ist:

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. Führen Sie den folgenden Maven-Befehl aus dem Verzeichnis `aem-guides-wknd-spa` aus, um das Projekt zu erstellen und AEM bereitzustellen:

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ```

   Bei Verwendung von [AEM 6.x](overview.md#compatibility):

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

   Die verschiedenen Module des Projekts sollten kompiliert und AEM bereitgestellt werden.

   ```plain
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for wknd-spa-angular 1.0.0-SNAPSHOT:
   [INFO] 
   [INFO] wknd-spa-angular ................................... SUCCESS [  0.473 s]
   [INFO] WKND SPA Angular - Core ............................ SUCCESS [ 54.866 s]
   [INFO] wknd-spa-angular.ui.frontend - UI Frontend ......... SUCCESS [02:10 min]
   [INFO] WKND SPA Angular - Repository Structure Package .... SUCCESS [  0.694 s]
   [INFO] WKND SPA Angular - UI apps ......................... SUCCESS [  6.351 s]
   [INFO] WKND SPA Angular - UI content ...................... SUCCESS [  2.885 s]
   [INFO] WKND SPA Angular - All ............................. SUCCESS [  1.736 s]
   [INFO] WKND SPA Angular - Integration Tests Bundles ....... SUCCESS [  2.563 s]
   [INFO] WKND SPA Angular - Integration Tests Launcher ...... SUCCESS [  1.846 s]
   [INFO] WKND SPA Angular - Dispatcher ...................... SUCCESS [  0.270 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   Das Maven-Profil ***autoInstallSinglePackage*** kompiliert die einzelnen Module des Projekts und stellt ein einzelnes Paket in der AEM-Instanz bereit. Standardmäßig wird dieses Paket auf einer AEM-Instanz bereitgestellt, die lokal auf Port **4502** ausgeführt wird, und mit den Anmeldedaten von **admin:admin**.

4. Navigieren Sie in Ihrer lokalen AEM-Instanz zu **[!UICONTROL Package Manager]**: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

5. Es sollten drei Pakete für `wknd-spa-angular.all`, `wknd-spa-angular.ui.apps` und `wknd-spa-angular.ui.content` angezeigt werden.

   ![WKND SPA Packages](./assets/create-project/package-manager.png)

   Der gesamte für das Projekt benötigte benutzerspezifische Code wird in diesen Paketen gebündelt und zur AEM Laufzeit installiert.

6. Sie sollten auch mehrere Pakete für `spa.project.core` und `core.wcm.components` sehen. Hierbei handelt es sich um Abhängigkeiten, die automatisch vom Archetyp eingeschlossen werden. Weitere Informationen zu [AEM Kernkomponenten finden Sie hier](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/introduction.html).

## Autoreninhalt

Öffnen Sie als Nächstes die SPA, die vom Archetyp generiert wurde, und aktualisieren Sie einige Inhalte.

1. Navigieren Sie zur Konsole **[!UICONTROL Sites]**: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   Die WKND-SPA enthält eine grundlegende Site-Struktur mit einem Land, einer Sprache und einer Homepage. Diese Hierarchie basiert auf den Standardwerten des Archetyps für `language_country` und `isSingleCountryWebsite`. Diese Werte können überschrieben werden, indem die [verfügbaren Eigenschaften](https://github.com/adobe/aem-project-archetype#available-properties) beim Generieren eines Projekts aktualisiert werden.

2. Öffnen Sie die Seite **[!DNL us]** > **[!DNL en]** > **[!DNL WKND SPA Angular Home Page]** , indem Sie die Seite auswählen und auf die Schaltfläche **[!UICONTROL Bearbeiten]** in der Menüleiste klicken:

   ![Site-Konsole](./assets/create-project/open-home-page.png)

3. Eine Komponente **[!UICONTROL Text]** wurde der Seite bereits hinzugefügt. Sie können diese Komponente wie jede andere Komponente in AEM bearbeiten.

   ![Textkomponente aktualisieren](./assets/create-project/update-text-component.gif)

4. Fügen Sie der Seite eine zusätzliche Komponente **[!UICONTROL Text]** hinzu.

   Beachten Sie, dass das Authoring-Erlebnis dem herkömmlichen AEM Sites-Seitenerlebnis ähnelt. Derzeit ist eine begrenzte Anzahl von Komponenten verfügbar, die verwendet werden können. Im Laufe des Tutorials werden weitere Informationen hinzugefügt.

## Inspect für Einzelseiten-Apps

Überprüfen Sie anschließend mithilfe der Entwicklertools Ihres Browsers, ob es sich um eine Einzelseiten-App handelt.

1. Klicken Sie im **[!UICONTROL Seiten-Editor]** auf das Menü **[!UICONTROL Seiteninformationen]** > **[!UICONTROL Als veröffentlicht anzeigen]**:

   ![Schaltfläche &quot;Als veröffentlicht anzeigen&quot;](./assets/create-project/view-as-published.png)

   Dadurch wird eine neue Registerkarte mit dem Abfrageparameter `?wcmmode=disabled` geöffnet, wodurch der AEM Editor effektiv deaktiviert wird: [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)

2. Zeigen Sie die Quelle der Seite an und beachten Sie, dass der Textinhalt **[!DNL Hello World]** oder ein anderer Inhalt nicht gefunden wurde. Stattdessen sollte HTML wie folgt angezeigt werden:

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-angular/clientlibs/clientlib-angular.min.js"></script>
       ...
   </body>
   ...
   ```

   `clientlib-angular.min.js` ist die Angular-SPA, die auf die Seite geladen wird und für die Wiedergabe des Inhalts verantwortlich ist.

   *Woher kommt der Inhalt?*

3. Kehren Sie zur Registerkarte zurück: [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)
4. Öffnen Sie die Entwicklertools des Browsers und überprüfen Sie den Netzwerk-Traffic der Seite während einer Aktualisierung. Zeigen Sie die Anforderungen **XHR** an:

   ![XHR-Anfragen](./assets/create-project/xhr-requests.png)

   Es sollte eine Anfrage an [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) geben. Dies enthält den gesamten Inhalt, formatiert in JSON, der die SPA steuert.

5. Öffnen Sie in einer neuen Registerkarte [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   Die Anfrage `en.model.json` stellt das Inhaltsmodell dar, das die Anwendung steuern wird. Inspect die JSON-Ausgabe und Sie sollten in der Lage sein, den Codeausschnitt zu finden, der die **[!UICONTROL Text]**-Komponente(n) darstellt.

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
   },
   ...
   ```

   Im nächsten Kapitel werden wir untersuchen, wie der JSON-Inhalt von AEM Komponenten zu SPA Komponenten zugeordnet wird, um die Grundlage für das AEM SPA Editor zu bilden.

   >[!NOTE]
   >
   > Es kann hilfreich sein, eine Browsererweiterung zu installieren, um die JSON-Ausgabe automatisch zu formatieren.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben gerade Ihr erstes AEM SPA Editor Projekt erstellt!

Es ist jetzt ganz einfach, aber in den nächsten Kapiteln wird mehr Funktionalität hinzugefügt.

### Nächste Schritte {#next-steps}

[Integrieren Sie die SPA](integrate-spa.md)  - Erfahren Sie, wie der SPA-Quellcode in das AEM-Projekt integriert ist, und lernen Sie die Tools kennen, die zur schnellen Entwicklung des SPA verfügbar sind.

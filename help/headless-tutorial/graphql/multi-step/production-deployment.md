---
title: Produktionsbereitstellung mit einem AEM-Veröffentlichungsdienst - Erste Schritte mit AEM Headless - GraphQL
description: Erfahren Sie mehr über die AEM-Autoren- und Veröffentlichungsdienste und das empfohlene Bereitstellungsmuster für Headless-Anwendungen. In diesem Tutorial erfahren Sie, wie Sie Umgebungsvariablen verwenden können, um einen GraphQL-Endpunkt basierend auf der Zielumgebung dynamisch zu ändern. Erfahren Sie, wie Sie AEM für Cross-Origin Resource Sharing (CORS) ordnungsgemäß konfigurieren.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 7131
thumbnail: KT-7131.jpg
exl-id: 8c8b2620-6bc3-4a21-8d8d-8e45a6e9fc70
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '2357'
ht-degree: 10%

---

# Produktionsbereitstellung mit einem AEM-Veröffentlichungsdienst

In diesem Tutorial richten Sie eine lokale Umgebung ein, um zu simulieren, wie Inhalte von einer Autoreninstanz an eine Veröffentlichungsinstanz verteilt werden. Sie generieren außerdem den Produktions-Build einer React-App, die für die Verwendung von Inhalten aus der AEM-Veröffentlichungsumgebung mithilfe der GraphQL-APIs konfiguriert ist. Außerdem erfahren Sie, wie Sie Umgebungsvariablen effektiv verwenden und die AEM CORS-Konfigurationen aktualisieren können.

## Voraussetzungen

Dieses Tutorial ist Teil eines mehrteiligen Tutorials. Es wird davon ausgegangen, dass die in den vorherigen Teilen beschriebenen Schritte abgeschlossen sind.

## Ziele

Erfahren Sie mehr über:

* Machen Sie sich mit der Architektur von AEM Author und Publish vertraut.
* Erfahren Sie mehr über Best Practices für die Verwaltung von Umgebungsvariablen.
* Erfahren Sie, wie Sie AEM für Cross-Origin Resource Sharing (CORS) ordnungsgemäß konfigurieren.

## Bereitstellungsmuster für Autoren- und Veröffentlichungsinstanz {#deployment-pattern}

Eine vollständige AEM-Umgebung besteht aus einer Autoren-, Veröffentlichungs- und Dispatcher-Komponente. Der Autoren-Service, mit dem interne Anwender Inhalte erstellen, verwalten und in der Vorschau anzeigen. Der Veröffentlichungsdienst wird als &quot;Live&quot;-Umgebung betrachtet und ist in der Regel das, mit dem Endbenutzer interagieren. Inhalte werden nach der Bearbeitung und Genehmigung im Autoren-Service an den Veröffentlichungs-Service weitergeleitet.

Das häufigste Bereitstellungsmuster bei AEM Headless-Programmen besteht darin, die Produktionsversion des Programms mit dem Veröffentlichungs-Service von AEM zu verbinden.

![Implementierungsmuster auf hoher Ebene](assets/publish-deployment/high-level-deployment.png)

Das obige Diagramm zeigt dieses allgemeine Implementierungsmuster.

1. A **Inhaltsautor** verwendet den AEM-Autorendienst zum Erstellen, Bearbeiten und Verwalten von Inhalten.
2. Der **Inhaltsautor** und andere interne Benutzern können die Inhalte direkt im Autoren-Service in der Vorschau anzeigen. Es kann eine Vorschauversion des Programms eingerichtet werden, die eine Verbindung zum Autoren-Service herstellt.
3. Sobald der Inhalt validiert wurde, kann er **veröffentlicht** zum AEM-Veröffentlichungsdienst.
4. **Endbenutzer interagieren mit der Produktionsversion des Programms.** Die Produktionsanwendung stellt eine Verbindung zum Veröffentlichungsdienst her und verwendet die GraphQL-APIs, um Inhalte anzufordern und zu nutzen.

Das Tutorial simuliert die oben genannte Bereitstellung, indem eine AEM-Veröffentlichungsinstanz zur aktuellen Einrichtung hinzugefügt wird. In früheren Kapiteln fungierte die React App als Vorschau, indem sie eine direkte Verbindung zur Autoreninstanz herstellte. Ein Produktions-Build der React-App wird auf einem statischen Node.js-Server bereitgestellt, der eine Verbindung zur neuen Veröffentlichungsinstanz herstellt.

Am Ende werden drei lokale Server ausgeführt:

* http://localhost:4502 - Autoreninstanz
* http://localhost:4503 - Veröffentlichungsinstanz
* http://localhost:5000 - React-App im Produktionsmodus, die eine Verbindung zur Veröffentlichungsinstanz herstellt.

## AEM SDK installieren - Veröffentlichungsmodus {#aem-sdk-publish}

Derzeit befindet sich eine laufende Instanz des SDK in **Autor** -Modus. Das SDK kann auch in **Veröffentlichen** -Modus, um eine AEM-Veröffentlichungsumgebung zu simulieren.

Eine detailliertere Anleitung zum Einrichten einer lokalen Entwicklungsumgebung [finden Sie hier .](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=de#local-development-environment-set-up).

1. Erstellen Sie auf Ihrem lokalen Dateisystem einen dedizierten Ordner, um die Veröffentlichungsinstanz zu installieren, d. h. `~/aem-sdk/publish`.
1. Kopieren Sie die Schnellstart-JAR-Datei, die in vorherigen Kapiteln für die Autoreninstanz verwendet wurde, und fügen Sie sie in die Datei `publish` Verzeichnis. Alternativ können Sie zur [Software Distribution-Portal](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) und laden Sie das neueste SDK herunter und extrahieren Sie die Schnellstart-JAR-Datei.
1. Benennen Sie die JAR-Datei in `aem-publish-p4503.jar`.

   Die `publish` -Zeichenfolge gibt an, dass die Schnellstart-JAR-Datei im Veröffentlichungsmodus gestartet wird. Die `p4503` gibt an, dass der Quickstart-Server auf Port 4503 ausgeführt wird.

1. Öffnen Sie ein neues Terminal-Fenster und navigieren Sie zu dem Ordner, der die JAR-Datei enthält. Installieren und starten Sie die AEM-Instanz:

   ```shell
   $ cd ~/aem-sdk/publish
   $ java -jar aem-publish-p4503.jar
   ```

1. Geben Sie ein Administratorkennwort als `admin`. Jedes Administratorkennwort ist akzeptabel. Es wird jedoch empfohlen, den Standard für die lokale Entwicklung zu verwenden, um zusätzliche Konfigurationen zu vermeiden.
1. Wenn die Installation der AEM-Instanz abgeschlossen ist, wird ein neues Browser-Fenster unter [http://localhost:4503/content.html](http://localhost:4503/content.html)

   Es wird erwartet, dass eine 404 Not Found -Seite zurückgegeben wird. Dies ist eine brandneue AEM-Instanz, und es wurde kein Inhalt installiert.

## Beispielinhalt und GraphQL-Endpunkte installieren {#wknd-site-content-endpoints}

Genau wie in der -Autoreninstanz muss für die Veröffentlichungsinstanz die GraphQL-Endpunkte aktiviert sein und Beispielinhalte erforderlich sein. Installieren Sie anschließend die WKND-Referenz-Site auf der Veröffentlichungsinstanz.

1. Laden Sie das neueste kompilierte AEM-Paket für die WKND-Site herunter: [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Laden Sie die Standardversion herunter, die mit AEM as a Cloud Service kompatibel ist. **not** die `classic` -Version.

1. Melden Sie sich bei der Veröffentlichungsinstanz an, indem Sie direkt zu folgenden Elementen navigieren: [http://localhost:4503/libs/granite/core/content/login.html](http://localhost:4503/libs/granite/core/content/login.html) mit dem Benutzernamen `admin` und Kennwort `admin`.
1. Navigieren Sie dann zu Package Manager unter [http://localhost:4503/crx/packmgr/index.jsp](http://localhost:4503/crx/packmgr/index.jsp).
1. Klicken **Paket hochladen** und wählen Sie das im vorherigen Schritt heruntergeladene WKND-Paket. Klicken Sie auf **Installieren**, um das Paket zu installieren.
1. Nach der Installation des Pakets ist die WKND-Referenz-Website jetzt unter [http://localhost:4503/content/wknd/us/en.html](http://localhost:4503/content/wknd/us/en.html).
1. Abmelden als `admin` Benutzer durch Klicken auf die Schaltfläche &quot;Abmelden&quot; in der Menüleiste.

   ![WKND-Sign-out-Referenz-Site](assets/publish-deployment/sign-out-wknd-reference-site.png)

   Im Gegensatz zur AEM-Autoreninstanz wird für die AEM-Veröffentlichungsinstanzen standardmäßig der anonyme schreibgeschützte Zugriff verwendet. Wir möchten das Erlebnis eines anonymen Benutzers beim Ausführen der React-Anwendung simulieren.

## Aktualisieren von Umgebungsvariablen, um auf die Veröffentlichungsinstanz zu verweisen {#react-app-publish}

Aktualisieren Sie anschließend die von der React-Anwendung verwendeten Umgebungsvariablen, um auf die Veröffentlichungsinstanz zu verweisen. Die React-App sollte **only** Verbindung zur Veröffentlichungsinstanz im Produktionsmodus herstellen.

Als Nächstes fügen Sie eine neue Datei hinzu `.env.production.local` , um das Produktionserlebnis zu simulieren.

1. Öffnen Sie die WKND GraphQL React-App in Ihrer IDE.

1. darunter `aem-guides-wknd-graphql/react-app`, fügen Sie eine Datei mit dem Namen `.env.production.local`.
1. Füllen Sie `.env.production.local` wie folgt:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   ```

   ![Neue Umgebungsvariablendatei hinzufügen](assets/publish-deployment/env-production-local-file.png)

   Die Verwendung von Umgebungsvariablen erleichtert das Umschalten des GraphQL-Endpunkts zwischen einer Autoren- oder Veröffentlichungsumgebung, ohne dass im Anwendungscode zusätzliche Logik hinzugefügt wird. Weitere Informationen [Benutzerdefinierte Umgebungsvariablen für React finden Sie hier .](https://create-react-app.dev/docs/adding-custom-environment-variables).

   >[!NOTE]
   >
   > Beachten Sie, dass keine Authentifizierungsinformationen enthalten sind, da Veröffentlichungsumgebungen standardmäßig anonymen Zugriff auf Inhalte bieten.

## Bereitstellen eines statischen Knotenservers {#static-server}

Die React-App kann über den Webpack-Server gestartet werden, dies dient jedoch nur der Entwicklung. Simulieren Sie als Nächstes eine Produktionsimplementierung mithilfe von [serve](https://github.com/vercel/serve) , um einen Produktionsaufbau der React-App mit Node.js zu hosten.

1. Öffnen Sie ein neues Terminal-Fenster und navigieren Sie zum `aem-guides-wknd-graphql/react-app` directory

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. Installieren [serve](https://github.com/vercel/serve) mit dem folgenden Befehl:

   ```shell
   $ npm install serve --save-dev
   ```

1. Öffnen Sie die Datei `package.json` unter `react-app/package.json`. Skript mit dem Namen `serve`:

   ```diff
    "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   +   "serve": "npm run build && serve -s build"
   },
   ```

   Die `serve` -Skript führt zwei Aktionen aus. Zunächst wird ein Produktions-Build der React-App generiert. Außerdem wird der Node.js-Server gestartet und der Produktions-Build verwendet.

1. Kehren Sie zum Terminal zurück und geben Sie den Befehl ein, um den statischen Server zu starten:

   ```shell
   $ npm run serve
   
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.111:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

1. Öffnen Sie einen neuen Browser und navigieren Sie zu [http://localhost:5000/](http://localhost:5000/). Sie sollten sehen, wie die React App bereitgestellt wird.

   ![React App bereitgestellt](assets/publish-deployment/react-app-served-port5000.png)

   Beachten Sie, dass die GraphQL-Abfrage auf der Startseite funktioniert. Inspect **XHR** Anforderung mit Ihren Entwicklertools. Beachten Sie, dass die GraphQL-POST für die Veröffentlichungsinstanz unter `http://localhost:4503/content/graphql/global/endpoint.json`.

   Alle Bilder sind jedoch auf der Startseite beschädigt!

1. Klicken Sie auf eine der Abenteuer-Detailseiten.

   ![Adventure-Detailfehler](assets/publish-deployment/adventure-detail-error.png)

   Beachten Sie, dass ein GraphQL-Fehler für `adventureContributor`. In den nächsten Übungen werden die beschädigten Bilder und die `adventureContributor` -Probleme behoben.

## Absolute Bildverweise {#absolute-image-references}

Die Bilder scheinen beschädigt zu sein, da die `<img src` -Attribut auf einen relativen Pfad festgelegt ist und letztendlich auf den statischen Node-Server unter verweist. `http://localhost:5000/`. Stattdessen sollten diese Bilder auf die AEM-Veröffentlichungsinstanz verweisen. Dazu gibt es mehrere mögliche Lösungen. Wenn Sie den Webpack Development Server verwenden, die Datei `react-app/src/setupProxy.js` einen Proxy zwischen dem Webpack-Server und der AEM Autoreninstanz für alle Anfragen an `/content`. Eine Proxy-Konfiguration kann in einer Produktionsumgebung verwendet werden, muss jedoch auf der Ebene des Webservers konfiguriert werden. Beispiel: [Apache-Proxymodul](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html).

Die App kann aktualisiert werden, um mithilfe der `REACT_APP_HOST_URI` Umgebungsvariable. Verwenden wir stattdessen eine Funktion AEM GraphQL-API, um eine absolute URL zum Bild anzufordern.

1. Beenden Sie den Node.js-Server.
1. Kehren Sie zur IDE zurück und öffnen Sie die Datei . `Adventures.js` at `react-app/src/components/Adventures.js`.
1. Fügen Sie die `_publishUrl` -Eigenschaft auf `ImageRef` innerhalb der `allAdventuresQuery`:

   ```diff
   const allAdventuresQuery = `
   {
       adventureList {
       items {
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
           ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
           }
           }
       }
       }
   }
   `;
   ```

   `_publishUrl` und `_authorUrl` sind Werte, die in `ImageRef` -Objekt, um das Einschließen absoluter URLs zu vereinfachen.

1. Wiederholen Sie die obigen Schritte, um die in der `filterQuery(activity)` -Funktion, die `_publishUrl` -Eigenschaft.
1. Ändern Sie die `AdventureItem` Komponente bei `function AdventureItem(props)` auf die `_publishUrl` anstelle der `_path` -Eigenschaft beim Erstellen der `<img src=''>` Tag:

   ```diff
   - <img className="adventure-item-image" src={props.adventurePrimaryImage._path} alt={props.adventureTitle}/>
   + <img className="adventure-item-image" src={props.adventurePrimaryImage._publishUrl} alt={props.adventureTitle}/>
   ```

1. Öffnen Sie die Datei `AdventureDetail.js` unter `react-app/src/components/AdventureDetail.js`.
1. Wiederholen Sie die gleichen Schritte, um die GraphQL-Abfrage zu ändern und die `_publishUrl` Eigenschaft für das Abenteuer

   ```diff
    adventureByPath (_path: "${_path}") {
       item {
           _path
           adventureTitle
           adventureActivity
           adventureType
           adventurePrice
           adventureTripLength
           adventureGroupSize
           adventureDifficulty
           adventurePrice
           adventurePrimaryImage {
               ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
               }
           }
           adventureDescription {
               html
           }
           adventureItinerary {
               html
           }
           adventureContributor {
               fullName
               occupation
               pictureReference {
                   ...on ImageRef {
                       _path
   +                   _publishUrl
                   }
               }
           }
       }
       }
   } 
   ```

1. Ändern Sie die beiden `<img>` Tags für das Primäre Adventure-Bild und die Referenz zum Contributor-Bild in `AdventureDetail.js`:

   ```diff
   /* AdventureDetail.js */
   ...
   <img className="adventure-detail-primaryimage"
   -       src={adventureData.adventurePrimaryImage._path} 
   +       src={adventureData.adventurePrimaryImage._publishUrl} 
           alt={adventureData.adventureTitle}/>
   ...
   pictureReference =  <img className="contributor-image" 
   -                        src={props.pictureReference._path}
   +                        src={props.pictureReference._publishUrl} 
                            alt={props.fullName} />
   ```

1. Kehren Sie zum Terminal zurück und starten Sie den statischen Server:

   ```shell
   $ npm run serve
   ```

1. Navigieren Sie zu [http://localhost:5000/](http://localhost:5000/) und beachten Sie, dass Bilder angezeigt werden und dass die `<img src''>` Attributpunkte auf `http://localhost:4503`.

   ![Fehlerhafte Bilder behoben](assets/publish-deployment/broken-images-fixed.png)

## Simulieren der Inhaltsveröffentlichung {#content-publish}

Erinnern Sie sich daran, dass ein GraphQL-Fehler für `adventureContributor` bei Anforderung einer Seite mit Details zum Abenteuer . Die **Mitarbeiter** Das Inhaltsfragmentmodell ist noch nicht in der Veröffentlichungsinstanz vorhanden. An den **Abenteuer** Das Inhaltsfragmentmodell ist auch nicht in der Veröffentlichungsinstanz verfügbar. Diese Änderungen wurden direkt an der Autoreninstanz vorgenommen und müssen an die Veröffentlichungsinstanz verteilt werden.

Dies ist beim Rollout neuer Aktualisierungen für eine Anwendung zu beachten, die auf Aktualisierungen eines Inhaltsfragments oder eines Inhaltsfragmentmodells angewiesen sind.

Als Nächstes wird die Simulation der Inhaltsveröffentlichung zwischen der lokalen Autoren- und Veröffentlichungsinstanz ermöglicht.

1. Starten Sie die Autoreninstanz (falls noch nicht gestartet) und navigieren Sie zu Package Manager unter [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Paket herunterladen [EnableReplicationAgent.zip](./assets/publish-deployment/EnableReplicationAgent.zip) und installieren Sie es mit Package Manager.

   Dieses Paket installiert eine Konfiguration, die es der Autoreninstanz ermöglicht, Inhalte in der Veröffentlichungsinstanz zu veröffentlichen. Manuelle Schritte für [Diese Konfiguration finden Sie hier .](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en#content-distribution).

   >[!NOTE]
   >
   > In einer AEM as a Cloud Service Umgebung wird die Autorenstufe automatisch so eingerichtet, dass Inhalte an die Veröffentlichungsstufe verteilt werden.

1. Aus dem **AEM Start** Menü, navigieren Sie zu **Instrumente** > **Assets** > **Inhaltsfragmentmodelle**.

1. Klicken Sie in die **WKND-Site** Ordner.

1. Wählen Sie alle drei Modelle aus und klicken Sie auf **Veröffentlichen**:

   ![Inhaltsfragmentmodelle veröffentlichen](assets/publish-deployment/publish-contentfragment-models.png)

   Ein Bestätigungsdialogfeld wird angezeigt, klicken Sie auf **Veröffentlichen**.

1. Navigieren Sie zum Inhaltsfragment des Bali Surf Camp unter [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp).

1. Klicken Sie auf **Veröffentlichen** in der oberen Menüleiste.

   ![Klicken Sie im Inhaltsfragment-Editor auf die Schaltfläche &quot;Veröffentlichen&quot;](assets/publish-deployment/publish-bali-content-fragment.png)

1. Der Veröffentlichungsassistent zeigt alle abhängigen Assets an, die veröffentlicht werden sollen. In diesem Fall wird das referenzierte Fragment **stacey-roswells** aufgelistet ist und mehrere Bilder referenziert werden. Die referenzierten Assets werden zusammen mit dem Fragment veröffentlicht.

   ![Referenzierte Assets zur Veröffentlichung](assets/publish-deployment/referenced-assets.png)

   Klicken Sie auf **Veröffentlichen** erneut auf, um das Inhaltsfragment und die abhängigen Assets zu veröffentlichen.

1. Kehren Sie zur React-App zurück, die unter [http://localhost:5000/](http://localhost:5000/). Sie können jetzt in das Bali Surf Camp klicken, um die Abenteuerdetails zu sehen.

1. Wechseln Sie zurück zur AEM-Autoreninstanz unter [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) und aktualisieren Sie die **Titel** des Fragments. **Speichern und schließen** das Fragment. Dann **publish** das Fragment.
1. Zurück zu [http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) und beobachten Sie die veröffentlichten Änderungen.

   ![Update der Veröffentlichung des Bali Surf Camp](assets/publish-deployment/bali-surf-camp-update.png)

## Aktualisieren der COR-Konfiguration

AEM ist standardmäßig sicher und erlaubt es nicht AEM Webeigenschaften nicht, clientseitige Aufrufe durchzuführen. AEM Cross-Origin Resource Sharing (CORS)-Konfiguration kann es bestimmten Domänen ermöglichen, Aufrufe an AEM durchzuführen.

Experimentieren Sie anschließend mit der CORS-Konfiguration der AEM-Veröffentlichungsinstanz.

1. Kehren Sie zum Terminal-Fenster zurück, in dem die React App mit dem Befehl ausgeführt wird `npm run serve`:

   ```shell
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.205:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

   Beachten Sie, dass zwei URLs bereitgestellt werden. Eins `localhost` und eine andere, die die IP-Adresse des lokalen Netzwerks verwenden.

1. Navigieren Sie zur -Adresse, beginnend mit [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000). Die Adresse ist für jeden lokalen Computer etwas anders. Beachten Sie, dass beim Abrufen der Daten ein CORS-Fehler auftritt. Dies liegt daran, dass die aktuelle CORS-Konfiguration nur Anforderungen von `localhost`.

   ![CORS-Fehler](assets/publish-deployment/cors-error-not-fetched.png)

   Aktualisieren Sie anschließend die AEM Publish CORS-Konfiguration, um Anforderungen von der Netzwerk-IP-Adresse zuzulassen.

1. Navigieren Sie zu [http://localhost:4503/content/wknd/us/en/errors/sign-in.html](http://localhost:4503/content/wknd/us/en/errors/sign-in.html) und melden Sie sich mit dem Benutzernamen an `admin` und Kennwort `admin`.

1. Navigieren Sie zu [http://localhost:4503/system/console/configMgr](http://localhost:4503/system/console/configMgr) und suchen Sie die WKND GraphQL-Konfiguration unter `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`.

1. Aktualisieren Sie die **Zulässiger Ursprung** -Feld, um die Netzwerk-IP-Adresse einzuschließen:

   ![Aktualisieren der CORS-Konfiguration](assets/publish-deployment/cors-update.png)

   Es ist auch möglich, einen regulären Ausdruck einzufügen, um alle Anforderungen einer bestimmten Subdomain zuzulassen. Speichern Sie die Änderungen.

1. Suchen Sie nach **Apache Sling Referrer Filter** und überprüfen Sie die Konfiguration. Die **Leere erlauben** Außerdem ist eine Konfiguration erforderlich, um GraphQL-Anforderungen von einer externen Domäne zu aktivieren.

   ![Sling Referrer-Filter](assets/publish-deployment/sling-referrer-filter.png)

   Diese wurden als Teil der WKND-Referenz-Website konfiguriert. Sie können den vollständigen Satz der OSGi-Konfigurationen über [das GitHub-Repository](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig).

   >[!NOTE]
   >
   > OSGi-Konfigurationen werden in einem AEM Projekt verwaltet, das der Quell-Code-Verwaltung zugewiesen ist. Ein AEM-Projekt kann mithilfe von Cloud Manager in AEM als Cloud Service-Umgebungen bereitgestellt werden. Die [AEM Projektarchetyp](https://github.com/adobe/aem-project-archetype) kann dabei helfen, ein Projekt für eine bestimmte Implementierung zu erstellen.

1. Kehren Sie zur React-App zurück, beginnend mit [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000) und beachten Sie, dass die Anwendung keinen CORS-Fehler mehr ausgibt.

   ![CORS-Fehler korrigiert](assets/publish-deployment/cors-error-corrected.png)

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben jetzt eine vollständige Produktionsbereitstellung mithilfe einer AEM-Veröffentlichungsumgebung simuliert. Sie haben auch gelernt, wie die CORS-Konfiguration in AEM verwendet wird.

## Sonstige -Ressourcen

Weitere Informationen zu Inhaltsfragmenten und GraphQL finden Sie in den folgenden Ressourcen:

* [Headless-Bereitstellung von Inhalten mithilfe von Inhaltsfragmenten mit GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html?lang=de)
* [AEM GraphQL-API zur Verwendung mit Inhaltsfragmenten](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=de)
* [Token-basierte Authentifizierung](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=de#authentication)
* [Bereitstellen von Code für AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-manager/devops/deploy-code.html?lang=en#cloud-manager)

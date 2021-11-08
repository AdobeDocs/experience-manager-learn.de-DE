---
title: Quick Setup - Erste Schritte mit AEM Headless - GraphQL
description: Erste Schritte mit Adobe Experience Manager (AEM) und GraphQL. Installieren Sie das AEM SDK, fügen Sie Beispielinhalte hinzu und stellen Sie eine Anwendung bereit, die mithilfe ihrer GraphQL-APIs Inhalte aus AEM verbraucht. Erfahren Sie, wie AEM kanalübergreifende Erlebnisse ermöglicht.
version: Cloud Service
mini-toc-levels: 1
kt: 6386
thumbnail: KT-6386.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d2da6efa-1f77-4391-adda-e3180c42addc
source-git-commit: 846400cd3ac4eb1b04ece055dfcbbd677f11e88e
workflow-type: tm+mt
source-wordcount: '1819'
ht-degree: 3%

---

# Schnelleinstellungen {#setup}

Dieses Kapitel bietet eine schnelle Einrichtung einer lokalen Umgebung, um zu sehen, wie eine externe Anwendung Inhalte von AEM mithilfe AEM GraphQL-APIs nutzt. Spätere Kapitel im Tutorial werden von dieser Einrichtung aufbauen.

## Voraussetzungen {#prerequisites}

Die folgenden Tools sollten lokal installiert werden:

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## Ziele {#objectives}

1. Laden Sie das AEM SDK herunter und installieren Sie es.
1. Laden Sie Beispielinhalte von der WKND-Referenz-Website herunter und installieren Sie sie.
1. Laden Sie eine Beispielanwendung herunter und installieren Sie sie, um Inhalte mithilfe der GraphQL-APIs zu nutzen.

## Installieren des AEM SDK {#aem-sdk}

In diesem Tutorial wird die [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en#aem-as-a-cloud-service-sdk) , um AEM GraphQL-APIs zu erkunden. Dieser Abschnitt enthält eine kurze Anleitung zum Installieren des AEM SDK und dessen Ausführung im Autorenmodus. Eine detailliertere Anleitung zum Einrichten einer lokalen Entwicklungsumgebung [finden Sie hier .](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=de#local-development-environment-set-up).

>[!NOTE]
>
> Es ist auch möglich, das Tutorial mit einer AEM as a Cloud Service Umgebung zu verfolgen. Weitere Hinweise zur Verwendung einer Cloud-Umgebung finden Sie im Tutorial.

1. Navigieren Sie zum **[Software Distribution-Portal](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM as a Cloud Service** und laden Sie die neueste Version des **AEM SDK**.

   ![Software Distribution-Portal](assets/setup/software-distribution-portal-download.png)

   >[!CAUTION]
   >
   > Die GraphQL-Funktion ist standardmäßig nur im AEM SDK 2021-02-04 oder höher aktiviert.

1. Entpacken Sie den Download und kopieren Sie die Schnellstart-JAR (`aem-sdk-quickstart-XXX.jar`) in einen bestimmten Ordner, d. h. `~/aem-sdk/author`.
1. Benennen Sie die JAR-Datei erneut in `aem-author-p4502.jar`.

   Die `author` name gibt an, dass die Schnellstart-JAR-Datei im Autorenmodus gestartet wird. Die `p4502` gibt an, dass der Quickstart-Server an Port 4502 ausgeführt wird.

1. Öffnen Sie ein neues Terminal-Fenster und navigieren Sie zu dem Ordner, der die JAR-Datei enthält. Führen Sie den folgenden Befehl aus, um die AEM-Instanz zu installieren und zu starten:

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. Geben Sie ein Administratorkennwort als `admin`. Jedes Administratorkennwort ist akzeptabel. Es wird jedoch empfohlen, `admin` für die lokale Entwicklung, um die Notwendigkeit einer Neukonfiguration zu verringern.
1. Nach einigen Minuten wird die Installation der AEM-Instanz abgeschlossen und ein neues Browser-Fenster sollte geöffnet werden unter [http://localhost:4502](http://localhost:4502).
1. Anmelden mit dem Benutzernamen `admin` und das während AEM ersten Starts ausgewählte Kennwort (normalerweise `admin`).

## Beispielinhalt und GraphQL-Endpunkte installieren {#wknd-site-content-endpoints}

Beispielinhalt aus dem **WKND-Referenz-Site** installiert, um das Tutorial zu beschleunigen. Die WKND ist eine fiktive Lebensmarke, die häufig in Verbindung mit AEM Training verwendet wird.

Die WKND-Referenz-Site enthält Konfigurationen, die zum Anzeigen einer [GraphQL-Endpunkt](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=en#graphql-aem-endpoint). Führen Sie in einer realen Implementierung die dokumentierten Schritte aus, um [GraphQL-Endpunkte einschließen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=en#graphql-aem-endpoint) in Ihrem Kundenprojekt. A [CORS](#cors-config) wurde auch als Teil der WKND-Site verpackt. Eine CORS-Konfiguration ist erforderlich, um Zugriff auf eine externe Anwendung zu gewähren. Weitere Informationen finden Sie unter [CORS](#cors-config) finden Sie unten.

1. Laden Sie das neueste kompilierte AEM-Paket für die WKND-Site herunter: [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Laden Sie die Standardversion herunter, die mit AEM as a Cloud Service kompatibel ist. **not** die `classic` -Version.

1. Aus dem **AEM Start** Menünavigation zu **Instrumente** > **Implementierung** > **Pakete**.

   ![Navigieren Sie zu Pakete .](assets/setup/navigate-to-packages.png)

1. Klicken **Paket hochladen** und wählen Sie das im vorherigen Schritt heruntergeladene WKND-Paket. Klicken **Installieren** um das Paket zu installieren.

1. Aus dem **AEM Start** Menünavigation zu **Assets** > **Dateien**.
1. Klicken Sie durch die Ordner, um zu **WKND-Site** > **englisch** > **Abenteuer**.

   ![Ordneransicht von Adventures](assets/setup/folder-view-adventures.png)

   Dies ist ein Ordner aller Assets, aus denen die verschiedenen Adventures der Marke WKND bestehen. Dazu gehören traditionelle Medientypen wie Bilder und Videos sowie AEM Medien. **Inhaltsfragmente**.

1. Klicken Sie in die **Skifahren auf dem Nachhauseweg** und klicken Sie auf **Nachlassendes Überspringen von Wyoming-Inhaltsfragment** Karte:

   ![Herunterfahren der Inhaltsfragmentkarte](assets/setup/downhill-skiing-cntent-fragment.png)

1. Die Benutzeroberfläche des Inhaltsfragmente-Editors wird für das Abenteuer &quot;Wyoming beim Skifahren&quot;geöffnet.

   ![Nachlassendes Überspringen von Inhaltsfragmenten](assets/setup/down-hillskiing-fragment.png)

   Beachten Sie, dass verschiedene Felder wie **Titel**, **Beschreibung** und **Aktivität** das Fragment definieren.

   **Inhaltsfragmente** sind eine der Möglichkeiten, Inhalte in AEM zu verwalten. Inhaltsfragmente sind wiederverwendbare, präsentationsunabhängige Inhalte, die aus strukturierten Datenelementen wie Text, Rich-Text, Daten oder Verweisen auf andere Inhaltsfragmente bestehen. Inhaltsfragmente werden später im Tutorial genauer untersucht.

1. Klicken **Abbrechen** , um das Fragment zu schließen. Sie können auch in andere Ordner navigieren und die anderen Adventure-Inhalte durchsuchen.

>[!NOTE]
>
> Informationen zur Verwendung einer Cloud Service-Umgebung finden Sie in der Dokumentation . [Bereitstellung einer Codebasis wie der WKND-Referenz-Site in einer Cloud Service-Umgebung](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=en#coding-against-the-right-aem-version).

## Installieren der Beispielanwendung{#sample-app}

Eines der Ziele dieses Tutorials besteht darin, zu zeigen, wie AEM Inhalte von einer externen Anwendung mit den GraphQL-APIs genutzt werden können. In diesem Tutorial wird eine React-App-Beispielanwendung verwendet, die teilweise abgeschlossen wurde, um das Tutorial zu beschleunigen. Die gleichen Lektionen und Konzepte gelten für Apps, die mit iOS, Android oder anderen Plattformen erstellt wurden. Die React-App ist absichtlich einfach, um unnötige Komplexität zu vermeiden. Es soll keine Referenzimplementierung sein.

1. Öffnen Sie ein neues Terminal-Fenster und klonen Sie die Tutorial-Starterverzweigung mithilfe von Git:

   ```shell
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Öffnen Sie in der IDE Ihrer Wahl die Datei . `.env.development` at `aem-guides-wknd-graphql/react-app/.env.development`. Stellen Sie sicher, dass `REACT_APP_AUTHORIZATION` nicht kommentiert ist und die Datei wie folgt aussieht:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   # Use Authorization when connecting to an AEM Author environment
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   Stellen Sie sicher, dass `React_APP_HOST_URI` entspricht Ihrer lokalen AEM-Instanz. In diesem Kapitel verbinden wir die React App direkt mit dem AEM **Autor** Umgebung. **Autor** -Umgebungen müssen standardmäßig authentifiziert werden. Daher stellt unsere App die Verbindung zum `admin` Benutzer. Dies ist eine gängige Praxis bei der Entwicklung, da es uns ermöglicht, schnell Änderungen an der AEM-Umgebung vorzunehmen und sie sofort in der App widerzuspiegeln.

   >[!NOTE]
   >
   > In einem Produktionsszenario stellt die App eine Verbindung zu einem AEM her **Veröffentlichen** Umgebung. Weitere Informationen hierzu finden Sie im Abschnitt [Produktionsbereitstellung](production-deployment.md) Kapitel.

1. Navigieren Sie zur `aem-guides-wknd-graphql/react-app` Ordner. Installieren und starten Sie das Programm:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Ein neues Browser-Fenster sollte die App automatisch unter [http://localhost:3000](http://localhost:3000).

   ![React Starter App](assets/setup/react-starter-app.png)

   Eine Liste des aktuellen Abenteuer-Inhalts von AEM sollte angezeigt werden.

1. Klicken Sie auf eines der Abenteuerbilder, um die Details des Abenteuers anzuzeigen. Es wird AEM gebeten, das Detail für ein Abenteuer zurückzugeben.

   ![Ansicht &quot;Adventure Details&quot;](assets/setup/adventure-details-view.png)

1. Verwenden Sie die Entwickler-Tools des Browsers, um die **Netzwerk** -Anfragen. Anzeigen der **XHR** Anforderungen und Beobachtung mehrerer POST-Anfragen an `/content/graphql/global/endpoint.json`, der für AEM konfigurierte GraphQL-Endpunkt.

   ![GraphQL Endpoint XHR-Anfrage](assets/setup/endpoint-gql.png)

1. Sie können die Parameter und die JSON-Antwort auch anzeigen, indem Sie die Netzwerkanforderung überprüfen. Es kann hilfreich sein, eine Browsererweiterung wie [GraphQL-Netzwerkinspektor](https://chrome.google.com/webstore/detail/graphql-network-inspector/ndlbedplllcgconngcnfmkadhokfaaln) für Chrome, um ein besseres Verständnis der Abfrage und Antwort zu erhalten.

## Inhaltsfragment ändern

Nachdem die React-App ausgeführt wird, aktualisieren Sie den Inhalt in AEM und sehen Sie sich die Änderung in der App an.

1. Navigieren Sie zu AEM [http://localhost:4502](http://localhost:4502).
1. Navigieren Sie zu **Assets** > **Dateien** > **WKND-Site** > **englisch** > **Abenteuer** > **[Bali Surf Camp](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)**.

   ![Ordner des Bali Surf Camp](assets/setup/bali-surf-camp-folder.png)

1. Klicken Sie in die **Bali Surf Camp** Inhaltsfragment , um den Inhaltsfragment-Editor zu öffnen.
1. Ändern Sie die **Titel** und **Beschreibung** Abenteuer

   ![Inhaltsfragment ändern](assets/setup/modify-content-fragment-bali.png)

1. Klicken Sie auf **Speichern**, um die Änderungen zu speichern.
1. Navigieren Sie zurück zur React-App unter [http://localhost:3000](http://localhost:3000) und aktualisieren, um Ihre Änderungen zu sehen:

   ![Update des Bali Surf Camp Adventure](assets/setup/overnight-bali-surf-camp-changes.png)

## Installieren des GraphiQL-Tools {#install-graphiql}

[GraphiQL](https://github.com/graphql/graphiql) ist ein Entwicklungstool und wird nur in Umgebungen auf niedrigerer Ebene wie einer Entwicklungs- oder einer lokalen Instanz benötigt. Mit der GraphiQL IDE können Sie die zurückgegebenen Abfragen und Daten schnell testen und verfeinern. GraphiQL bietet außerdem einfachen Zugriff auf die Dokumentation, sodass Sie leicht wissen können, welche Methoden verfügbar sind.

1. Navigieren Sie zum **[Software Distribution-Portal](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM as a Cloud Service**.
1. Suchen Sie nach &quot;GraphiQL&quot;(stellen Sie sicher, dass Sie die **i** in **GraphiQL**.
1. Neueste Version herunterladen **GraphiQL Content Package v.x.x.x**

   ![Herunterladen des GraphiQL-Pakets](assets/explore-graphql-api/software-distribution.png)

   Die ZIP-Datei ist ein AEM Paket, das direkt installiert werden kann.

1. Aus dem **AEM Start** Menünavigation zu **Instrumente** > **Implementierung** > **Pakete**.
1. Klicken **Paket hochladen** und wählen Sie das im vorherigen Schritt heruntergeladene Paket aus. Klicken **Installieren** um das Paket zu installieren.

   ![Installieren des GraphiQL-Pakets](assets/explore-graphql-api/install-graphiql-package.png)
1. Navigieren Sie zur GraphiQL-IDE unter [http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html) und beginnen Sie mit der Erforschung der GraphQL-APIs.

   >[!NOTE]
   >
   > Das GraphQL-Tool und die GraphQL-API lautet [detaillierter im Tutorial](./explore-graphql-api.md).

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben jetzt eine externe Anwendung, die AEM Inhalte mit GraphQL verbraucht. Sie können den Code in der React-App prüfen und mit der Änderung vorhandener Inhaltsfragmente experimentieren.

## Nächste Schritte {#next-steps}

Im nächsten Kapitel [Definieren von Inhaltsfragmentmodellen](content-fragment-models.md), erfahren Sie, wie Sie Inhalte modellieren und ein Schema mit **Inhaltsfragmentmodelle**. Sie werden vorhandene Modelle überprüfen und ein neues Modell erstellen. Außerdem erfahren Sie mehr über die verschiedenen Datentypen, mit denen ein Schema als Teil des Modells definiert werden kann.

## (Bonus) CORS-Konfiguration {#cors-config}

AEM blockiert standardmäßig ursprungsübergreifende Anforderungen und verhindert so, dass nicht autorisierte Anwendungen eine Verbindung zu ihrem Inhalt herstellen und ihn aufdecken.

Damit die React-App dieses Tutorials mit AEM GraphQL-API-Endpunkten interagieren kann, wurde im WKND-Site-Referenzprojekt eine ursprungsübergreifende Konfiguration für die Ressourcenfreigabe definiert.

![Cross-Origin Resource Sharing-Konfiguration](assets/setup/cross-origin-resource-sharing-configuration.png)

So zeigen Sie die bereitgestellte Konfiguration an:

1. Navigieren Sie zur Web-Konsole AEM SDK unter [http://localhost:4502/system/console](http://localhost:4502/system/console).

   >[!NOTE]
   >
   > Die Web-Konsole ist nur im SDK verfügbar. In einer AEM as a Cloud Service Umgebung können diese Informationen über [Entwicklerkonsole](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html).

1. Klicken Sie im oberen Menü auf **OSGi** > **Konfiguration** die [OSGi-Konfigurationen](http://localhost:4502/system/console/configMgr).
1. Scrollen Sie nach unten **Adobe Granite Cross-Origin Resource Sharing**.
1. Klicken Sie auf die Konfiguration für `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`.
1. Die folgenden Felder wurden aktualisiert:
   * Zulässiger Ursprung (Regex): `http://localhost:.*`
      * Ermöglicht alle lokalen Host-Verbindungen.
   * Zulässige Pfade: `/content/graphql/global/endpoint.json`
      * Dies ist der einzige derzeit konfigurierte GraphQL-Endpunkt. Als Best Practice sollten die COR-Konfigurationen so restriktiv wie möglich sein.
   * Zulässige Methoden: `GET`, `HEAD`, `POST`
      * Nur `POST` ist für GraphQL erforderlich, die anderen Methoden können jedoch bei der Headless-Interaktion mit AEM nützlich sein.
   * Unterstützte Kopfzeilen: **Autorisierung** wurde hinzugefügt, um grundlegende Authentifizierung in der Autorenumgebung zu übergeben.
   * Unterstützt Anmeldedaten: `Yes`
      * Dies ist erforderlich, da unsere React-App mit den geschützten GraphQL-Endpunkten im AEM-Autorendienst kommuniziert.

Diese Konfiguration und die GraphQL-Endpunkte sind Teil des AEM WKND-Projekts. Sie können alle [OSGi-Konfigurationen hier](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig).

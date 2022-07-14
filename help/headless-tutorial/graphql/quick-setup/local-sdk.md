---
title: AEM Headless-Schnelleinrichtung mit dem lokalen SDK
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
source-git-commit: 64086f3f7b340b143bd281e2f6f802af07554ecf
workflow-type: tm+mt
source-wordcount: '1258'
ht-degree: 3%

---

# AEM Headless-Schnelleinrichtung mit dem lokalen SDK {#setup}

Mit der AEM Headless-Schnelleinrichtung erhalten Sie praktische Informationen zu AEM Headless mithilfe von Inhalten aus dem WKND Site-Beispielprojekt und einer React App (einem SPA), die den Inhalt über AEM Headless GraphQL-APIs nutzt. In diesem Handbuch werden die [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html).

## Voraussetzungen {#prerequisites}

Die folgenden Tools sollten lokal installiert werden:

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## 1. Installieren des AEM SDK {#aem-sdk}

Dieses Setup verwendet die [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?#aem-as-a-cloud-service-sdk) , um AEM GraphQL-APIs zu erkunden. Dieser Abschnitt enthält eine kurze Anleitung zum Installieren des AEM SDK und dessen Ausführung im Autorenmodus. Eine detailliertere Anleitung zum Einrichten einer lokalen Entwicklungsumgebung [finden Sie hier .](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html#local-development-environment-set-up).

>[!NOTE]
>
> Es ist auch möglich, dem Tutorial mit einem [AEM as a Cloud Service Umgebung](./cloud-service.md). Weitere Hinweise zur Verwendung einer Cloud-Umgebung finden Sie im Tutorial.

1. Navigieren Sie zum **[Software Distribution-Portal](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)** > **AEM as a Cloud Service** und laden Sie die neueste Version des **AEM SDK**.

   ![Software Distribution-Portal](assets/quick-setup/aem-sdk/downloads__aem-sdk.png)

1. Entpacken Sie den Download und kopieren Sie die Schnellstart-JAR (`aem-sdk-quickstart-XXX.jar`) in einen bestimmten Ordner, d. h. `~/aem-sdk/author`.
1. Benennen Sie die JAR-Datei in `aem-author-p4502.jar`.

   Die `author` name gibt an, dass die Schnellstart-JAR-Datei im Autorenmodus gestartet wird. Die `p4502` gibt den Schnellstart-Vorgang für Port 4502 an.

1. Um die AEM zu installieren und zu starten, öffnen Sie eine Eingabeaufforderung in dem Ordner, der die JAR-Datei enthält, und führen Sie den folgenden Befehl aus:

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. Geben Sie ein Administratorkennwort als `admin`. Jedes Administratorkennwort ist akzeptabel. Es wird jedoch empfohlen, `admin` für die lokale Entwicklung, um die Notwendigkeit einer Neukonfiguration zu verringern.
1. Wenn die Installation des AEM-Dienstes abgeschlossen ist, sollte ein neues Browserfenster unter [http://localhost:4502](http://localhost:4502).
1. Anmelden mit dem Benutzernamen `admin` und das während AEM ersten Starts ausgewählte Kennwort (normalerweise `admin`).

## 2. Beispielinhalt installieren {#install-sample-content}

Beispielinhalt aus dem **WKND-Referenz-Site** wird verwendet, um das Tutorial zu beschleunigen. Die WKND ist eine fiktive Lebensmarke, die oft mit AEM Training verwendet wird.

Die WKND-Site enthält Konfigurationen, die erforderlich sind, um eine [GraphQL-Endpunkt](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html). Führen Sie in einer realen Implementierung die dokumentierten Schritte aus, um [GraphQL-Endpunkte einschließen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html) in Ihrem Kundenprojekt. A [CORS](#cors-config) wurde auch als Teil der WKND-Site verpackt. Eine CORS-Konfiguration ist erforderlich, um Zugriff auf eine externe Anwendung zu gewähren. Weitere Informationen finden Sie unter [CORS](#cors-config) finden Sie unten.

1. Laden Sie das neueste kompilierte AEM-Paket für die WKND-Site herunter: [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Laden Sie die Standardversion herunter, die mit AEM as a Cloud Service kompatibel ist. **not** die `classic` -Version.

1. Aus dem **AEM Start** Menü, navigieren Sie zu **Instrumente** > **Implementierung** > **Pakete**.

   ![Navigieren Sie zu Pakete .](assets/quick-setup/aem-sdk/aem-sdk__packages.png)

1. Klicken **Paket hochladen** und wählen Sie das im vorherigen Schritt heruntergeladene WKND-Paket. Klicken Sie auf **Installieren**, um das Paket zu installieren.

1. Aus dem **AEM Start** Menü, navigieren Sie zu **Assets** > **Dateien** > **WKND Shared** > **englisch** > **Abenteuer**.

   ![Ordneransicht von Adventures](assets/quick-setup/aem-sdk/aem-sdk__assets-folder.png)

   Dies ist ein Ordner aller Assets, aus denen die verschiedenen Adventures der Marke WKND bestehen. Dazu gehören herkömmliche Medientypen wie Bilder und Videos sowie AEM Medien. **Inhaltsfragmente**.

1. Klicken Sie in die **Skifahren auf dem Nachhauseweg** und klicken Sie auf **Nachlassendes Überspringen von Wyoming-Inhaltsfragment** Karte:

   ![Inhaltsfragmentkarte](assets/quick-setup/aem-sdk/aem-sdk__content-fragment.png)

1. Der Inhaltsfragmente-Editor wird für das Abenteuer &quot;Wyoming beim Skifahren&quot;geöffnet.

   ![Inhaltsfragmente-Editor](assets/quick-setup/aem-sdk/aem-sdk__content-fragment-editor.png)

   Beachten Sie, dass verschiedene Felder wie **Titel**, **Beschreibung** und **Aktivität** das Fragment definieren.

   **Inhaltsfragmente** sind eine der Möglichkeiten, Inhalte in AEM zu verwalten. Inhaltsfragmente sind wiederverwendbare, darstellungsunabhängige Inhalte, die aus strukturierten Datenelementen wie Text, Rich-Text, Daten oder Verweisen auf andere Inhaltsfragmente bestehen. Inhaltsfragmente werden später bei der schnellen Einrichtung genauer untersucht.

1. Klicken **Abbrechen** , um das Fragment zu schließen. Sie können auch in andere Ordner navigieren und die anderen Adventure-Inhalte durchsuchen.

>[!NOTE]
>
> Informationen zur Verwendung einer Cloud Service-Umgebung finden Sie in der Dokumentation . [Bereitstellung einer Codebasis wie der WKND-Referenz-Site in einer Cloud Service-Umgebung](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version).

## 3. Herunterladen und Ausführen der WKND React-App {#sample-app}

Eines der Ziele dieses Tutorials besteht darin, zu zeigen, wie AEM Inhalte von einer externen Anwendung mit den GraphQL-APIs genutzt werden können. In diesem Tutorial wird eine React-App-Beispielanwendung verwendet. Die React-App ist absichtlich einfach, um sich auf die Integration mit AEM GraphQL-APIs zu konzentrieren.

1. Öffnen Sie eine neue Eingabeaufforderung und klonen Sie die React-Beispielanwendung von GitHub:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. Öffnen Sie die React-App in `aem-guides-wknd-graphql/react-app` in Ihrer IDE Ihrer Wahl.
1. Öffnen Sie in der IDE die -Datei. `.env.development` at `/.env.development`. Überprüfen Sie die `REACT_APP_AUTHORIZATION` -Zeile ist nicht kommentiert und die Datei deklariert die folgenden Variablen:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   # Use Authorization when connecting to an AEM Author environment
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   Sichern `REACT_APP_HOST_URI` verweist auf Ihr lokales AEM-SDK. Der Einfachheit halber verbindet dieser Schnellstart die React-App mit  **AEM-Autor**. **Autor** Dienste müssen authentifiziert werden, sodass die App die `admin` Benutzer, um seine Verbindung herzustellen. Die Verbindung einer App mit AEM Author ist eine gängige Praxis bei der Entwicklung, da sie die schnelle Iteration von Inhalten erleichtert, ohne Änderungen veröffentlichen zu müssen.

   >[!NOTE]
   >
   > In einem Produktionsszenario stellt die App eine Verbindung zu einem AEM her **Veröffentlichen** Umgebung. Weitere Informationen hierzu finden Sie im Abschnitt _Produktionsbereitstellung_ Abschnitt.


1. Installieren und starten Sie die React-App:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Ein neues Browser-Fenster öffnet automatisch die App auf [http://localhost:3000](http://localhost:3000).

   ![React Starter App](assets/quick-setup/aem-sdk/react-app__home-view.png)

   Eine Liste der Abenteuerinhalte von AEM wird angezeigt.

1. Klicken Sie auf eines der Abenteuerbilder, um die Details des Abenteuers anzuzeigen. Es wird AEM gebeten, das Detail für ein Abenteuer zurückzugeben.

   ![Ansicht &quot;Adventure Details&quot;](assets/quick-setup/aem-sdk/react-app__adventure-view.png)

1. Verwenden Sie die Entwickler-Tools des Browsers, um die **Netzwerk** -Anfragen. Anzeigen der **XHR** Anfragen und Beobachtung mehrerer GET-Anfragen an `/graphql/execute.json/...`. Dieses Pfadpräfix ruft AEM persistenten Abfrageendpunkt auf und wählt die persistente Abfrage aus, die mit dem Namen und den kodierten Parametern nach dem Präfix ausgeführt werden soll.

   ![GraphQL Endpoint XHR-Anfrage](assets/quick-setup/aem-sdk/react-app__graphql-request.png)

## 4. Bearbeiten von Inhalten in AEM

Wenn die React-App ausgeführt wird, aktualisieren Sie den Inhalt in AEM und stellen Sie sicher, dass die Änderung in der App übernommen wird.

1. Navigieren Sie zu AEM [http://localhost:4502](http://localhost:4502).
1. Navigieren Sie zu **Assets** > **Dateien** > **WKND Shared** > **englisch** > **Abenteuer** > **[Bali Surf Camp](http://localhost:4502/assets.html/content/dam/wknd-shared/en/adventures/bali-surf-camp)**.

   ![Ordner des Bali Surf Camp](assets/setup/bali-surf-camp-folder.png)

1. Klicken Sie in die **Bali Surf Camp** Inhaltsfragment , um den Inhaltsfragment-Editor zu öffnen.
1. Ändern Sie die **Titel** und **Beschreibung** des Abenteuers.

   ![Inhaltsfragment ändern](assets/setup/modify-content-fragment-bali.png)

1. Klicken Sie auf **Speichern**, um die Änderungen zu speichern.
1. Aktualisieren Sie die React-App unter [http://localhost:3000](http://localhost:3000) um Ihre Änderungen zu sehen:

   ![Update des Bali Surf Camp Adventure](assets/setup/overnight-bali-surf-camp-changes.png)

## 5. GraphiQL durchsuchen {#graphiql}

1. Öffnen [GraphiQL](http://localhost:4502/aem/graphiql.html) durch Navigation zu **Instrumente** > **Allgemein** > **GraphQL-Abfrageeditor**
1. Wählen Sie auf der linken Seite vorhandene persistente Abfragen aus und führen Sie sie aus, um die Ergebnisse anzuzeigen.

   >[!NOTE]
   >
   > Das GraphQL-Tool und die GraphQL-API lautet [detaillierter im Tutorial](../multi-step/explore-graphql-api.md).

## Herzlichen Glückwunsch!{#congratulations}

Herzlichen Glückwunsch! Sie haben jetzt eine externe Anwendung, die AEM Inhalte mit GraphQL verbraucht. Sie können den Code in der React-App prüfen und mit der Änderung vorhandener Inhaltsfragmente experimentieren.

### Nächste Schritte

* [Starten des AEM Headless-Tutorials](../multi-step/overview.md)

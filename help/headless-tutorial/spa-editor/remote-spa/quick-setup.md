---
title: Schnelle Einrichtung von SPA-Editor und Remote-SPA
description: Erfahren Sie, wie Sie mit Remote-SPA und AEM SPA-Editor in 15 Minuten loslegen können!
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: ht
source-wordcount: '793'
ht-degree: 100%

---

# Schnelleinrichtung

In der Schnelleinrichtung wird anschaulich erklärt, wie die WKND-App installiert und ausgeführt wird, wie sie als Remote-SPA ausgeführt wird und wie sie mit dem AEM SPA-Editor erstellt wird.

Mit der Schnelleinrichtung gelangen Sie direkt zum Ende dieser Anleitung.

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_Eine Videoanleitung für die Schnelleinrichtung_

## Voraussetzungen

Dieses Tutorial erfordert Folgendes:

+ [AEM as a Cloud Service-SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=de)
+ [Node.js v18](https://nodejs.org/de/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ Voraussetzungen nur für macOS
   + [Xcode](https://developer.apple.com/xcode/) oder [Xcode-Befehlszeilen-Tools](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip oder höher](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql-Quellcode (Verzweigung: feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


Dieses Tutorial setzt Folgendes voraus:

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) als IDE
+ Ein Arbeitsverzeichnis von `~/Code/wknd-app`
+ Ausführen des AEM SDK als Autoren-Service in `http://localhost:4502`
+ Ausführen des AEM SDK mit dem lokalen `admin`-Konto mit Kennwort `admin`
+ Ausführen der SPA auf `http://localhost:3000`

## Starten des AEM SDK-Schnellstarts

Herunterladen und Installieren des AEM SDK-Schnellstarts auf Port 4502 mit den standardmäßigen `admin/admin`-Anmeldeinformationen.

1. [Laden Sie das aktuelle AEM SDK herunter.](https://experience.adobe.com/#/downloads/content/software-distribution/de/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. Entpacken Sie das AEM SDK in `~/aem-sdk`.
1. Führen Sie das AEM SDK-Schnellstart-JAR aus.

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

Das AEM SDK wird gestartet und automatisch auf [http://localhost:4502](http://localhost:4502) ausgeführt. Melden Sie sich mit den folgenden Anmeldeinformationen an:

+ Benutzername: `admin`
+ Passwort: `admin`

## Laden Sie das WKND-Site-Paket herunter und installieren Sie es.

Dieses Tutorial weist eine Abhängigkeit vom Projekt __WKND 2.1.0+__ auf (für den Inhalt).

1. [Laden Sie die neueste Version von `aem-guides-wknd.all.x.x.x.zip` herunter.](https://github.com/adobe/aem-guides-wknd/releases)
1. Melden Sie sich beim AEM SDK Package Manager unter [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) mit den `admin`-Anmeldeinformationen an.
1. __Laden__ Sie die in Schritt 1 heruntergeladene Datei `aem-guides-wknd.all.x.x.x.zip` hoch.
1. Tippen Sie auf die Schaltfläche __Installieren__ für den Eintrag `aem-guides-wknd.all-x.x.x.zip`.

## Herunterladen und Installieren von WKND-App-SPA-Paketen

Um eine Schnelleinrichtung zu ermöglichen, werden hier AEM-Pakete mit der endgültigen AEM-Konfiguration und den fertiggestellten AEM-Inhalten für das Tutorial bereitgestellt.

1. [Herunterladen ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Herunterladen ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. Melden Sie sich beim Package Manager des AEM SDK unter [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) mit den `admin`-Anmeldeinformationen an.
1. __Laden__ Sie die in Schritt 1 heruntergeladene Datei `wknd-app.all.x.x.x.zip` hoch.
1. Tippen Sie auf die Schaltfläche __Installieren__ für den Eintrag `wknd-app.all.x.x.x.zip`
1. __Laden__ Sie die in Schritt 2 heruntergeladene Datei `wknd-app.ui.content.sample.x.x.x.zip` hoch
1. Tippen Sie auf die Schaltfläche __Installieren__ für den Eintrag `wknd-app.ui.content.sample.x.x.x.zip`

## Herunterladen der WKND-App-Quelle

Laden Sie den Quell-Code der WKND-App von Github.com herunter und wechseln Sie zu der Verzweigung mit den Änderungen an der in diesem Tutorial ausgeführten SPA.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## Starten der SPA-Anwendung

Installieren Sie im Stammverzeichnis des Projekts die npm-Abhängigkeiten der SPA-Projekte und führen Sie die Anwendung aus.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

Wenn bei der Ausführung von `npm install` Fehler auftreten, führen Sie die folgenden Schritte aus:

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

Stellen Sie sicher, dass die SPA unter [http://localhost:3000](http://localhost:3000) ausgeführt wird.

## Authoring von Inhalten im AEM-SPA-Editor

Ordnen Sie vor dem Erstellen von Inhalten Ihre Browser-Fenster so an, dass AEM Author (`http://localhost:4502`) auf der linken Seite und die Remote-SPA (`http://localhost:3000`) auf der rechten Seite ausgeführt wird. Auf diese Weise können Sie sehen, wie Änderungen an AEM-bezogenen Inhalten sofort in der SPA widergespiegelt werden.

1. Melden Sie sich beim [AEM SDK Autoren-Service](http://localhost:4502) als `admin` an
1. Navigieren Sie zu __Sites > WKND App > us > en__
1. Bearbeiten Sie die __WKND-App-Startseite__
1. Wechseln Sie In den Modus __Bearbeiten__

### Erstellen der feststehenden Komponente der Startansicht

1. Tippen Sie auf den Text für __WKND-Adventures__, um die feststehende Titelkomponente zu aktivieren (in der SPA-Startansicht hartcodiert)
1. Tippen Sie in der Aktionsleiste der Titelkomponente auf das __Schraubenschlüsselsymbol__
1. Ändern Sie den Inhalt der Titelkomponente und speichern Sie ihn
1. Aktualisieren Sie die unter `http://localhost:3000` ausgeführte SPA und beobachten Sie, wie die Änderungen widergespiegelt werden

### Erstellen der Container-Komponente der Startansicht

1. Gehen Sie während der Bearbeitung der __WKND-App-Startseite__ wie folgt vor:
1. Erweitern Sie die __Seitenleiste des SPA-Editors__ (links)
1. Tippen Sie auf die Symbole __Komponenten__
1. Fügen Sie Komponenten zur Container-Komponente, die sich unter dem WKND-Logo und über der feststehenden Titelkomponente befindet, hinzu, ändern oder entfernen Sie diese
1. Aktualisieren Sie die unter `http://localhost:3000` ausgeführte SPA und beobachten Sie, wie die Änderungen widergespiegelt werden

### Erstellen einer Container-Komponente auf einer dynamischen Route

1. Wechseln Sie im SPA-Editor in den Modus __Vorschau__
1. Tippen Sie auf die Karte __Bali Surf Camp__ und navigieren Sie zur zugehörigen dynamischen Route
1. Fügen Sie Komponenten zur Container-Komponente, die sich über der Überschrift zur __Reiseroute__ befindet, hinzu, ändern oder entfernen Sie diese
1. Aktualisieren Sie die unter `http://localhost:3000` ausgeführte SPA und beobachten Sie, wie die Änderungen widergespiegelt werden

Neue AEM-Seiten unter __WKND-App-Startseite > Adventure__ _müssen_ einen AEM-Seitennamen haben, der dem Namen des Inhaltsfragments des entsprechenden Adventures entspricht. Dies liegt daran, dass die SPA-Route zur AEM-Seitenzuordnung auf dem letzten Segment der Route basiert, also dem Namen des Inhaltsfragments.

## Herzlichen Glückwunsch!

Sie haben soeben einen schnellen Überblick darüber erhalten, wie Sie mit dem AEM-SPA-Editor Ihre SPA mit kontrollierten, bearbeitbaren Bereichen verbessern können. Wir haben Ihr Interesse geweckt? Dann sehen Sie sich doch den Rest des Tutorials an. Achten Sie aber darauf, wieder von vorne zu beginnen, da Ihre lokale Entwicklungsumgebung in dieser Schnelleinrichtung jetzt den Endstatus des Tutorials widerspiegelt.

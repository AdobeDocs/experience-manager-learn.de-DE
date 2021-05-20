---
title: Schnelles Setup SPA Editor und Remote SPA
description: Erfahren Sie, wie Sie mit einem Remote-SPA und AEM SPA-Editor in 15 Minuten loslegen können!
topic: Headless, SPA, Entwicklung
feature: SPA Editor, Kernkomponenten, APIs, Entwicklung
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: 333181.jpg
source-git-commit: 73c75f8dac85615f4ed2dfdcc2ee4d0e9e5d161a
workflow-type: tm+mt
source-wordcount: '801'
ht-degree: 5%

---


# Schnelleinstellungen

Die schnelle Einrichtung ist eine schnelle Anleitung, die veranschaulicht, wie die WKND-App und als Remote-SPA installiert und ausgeführt und mit AEM SPA Editor erstellt wird.

Die schnelle Einrichtung bringt Sie direkt zum Endstatus dieses Tutorials.

>[!VIDEO](https://video.tv.adobe.com/v/333181/?quality=12&learn=on)

_Ein Video mit einer Anleitung zur schnellen Einrichtung_

## Voraussetzungen

Dieses Tutorial erfordert Folgendes:

+ [AEM as a Cloud Service-SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ Nur macOS-Voraussetzungen
   + [](https://developer.apple.com/xcode/) Befehlszeilen-Tools für Xcode oder  [Xcode](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all.0.3.0.zip oder höher](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql-Quellcode](https://github.com/adobe/aem-guides-wknd-graphql)


Dieses Tutorial setzt voraus, dass:

+ [Microsoft® Visual Studio-](https://visualstudio.microsoft.com/) Codeas als IDE
+ Ein Arbeitsverzeichnis von `~/Code/wknd-app`
+ Ausführen des AEM SDK als Autorendienst auf `http://localhost:4502`
+ Ausführen des AEM SDK mit dem lokalen `admin`-Konto mit dem Kennwort `admin`
+ Ausführen des SPA auf `http://localhost:3000`

## Starten des AEM SDK-Schnellstarts

Laden Sie das AEM SDK Quickstart auf Port 4502 herunter und installieren Sie es mit den Standardanmeldeinformationen `admin/admin`.

1. [Laden Sie das neueste AEM SDK herunter](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. Entpacken Sie das AEM SDK in `~/aem-sdk`
1. Ausführen der AEM SDK-Schnellstart-JAR

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK startet und startet automatisch auf [http://localhost:4502](http://localhost:4502). Melden Sie sich mit den folgenden Anmeldedaten an:

+ Benutzername: `admin`
+ Passwort: `admin`

## Herunterladen und Installieren des WKND Site-Pakets

Dieses Tutorial weist eine Abhängigkeit vom __WKND 0.3.0+-Projekt__ (für Inhalte) auf.

1. [Laden Sie die neueste Version von herunter.  `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Melden Sie sich unter [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) mit den Anmeldeinformationen `admin` im Package Manager AEM SDK an.
1. ____ Laden Sie die in Schritt 1  `aem-guides-wknd.all.x.x.x.zip` heruntergeladenen Dateien hoch.
1. Tippen Sie auf die Schaltfläche __Installieren__ für den Eintrag `aem-guides-wknd.all-x.x.x.zip`

## Herunterladen und Installieren von WKND App SPA Packages

Für eine schnelle Einrichtung werden AEM Pakete bereitgestellt, die die endgültige AEM und den Inhalt des Tutorials enthalten.

1. [Download ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Download ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.0.zip)
1. Melden Sie sich unter [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) mit den Anmeldeinformationen `admin` im Package Manager AEM SDK an.
1. ____ Laden Sie die in Schritt 1  `wknd-app.all.x.x.x.zip` heruntergeladenen Dateien hoch.
1. Tippen Sie auf die Schaltfläche __Installieren__ für den Eintrag `wknd-app.all.x.x.x.zip`
1. ____ Laden Sie die in Schritt 2  `wknd-app.ui.content.sample.x.x.x.zip` heruntergeladenen Dateien hoch.
1. Tippen Sie auf die Schaltfläche __Installieren__ für den Eintrag `wknd-app.ui.content.sample.x.x.x.zip`

## WKND-App-Quelle herunterladen

Laden Sie den Quellcode der WKND-App von Github.com herunter und wechseln Sie die Verzweigung, die die Änderungen an der in diesem Tutorial durchgeführten SPA enthält.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
$ git checkout -b feature/spa-editor
$ git pull origin feature/spa-editor
```

## Starten Sie die SPA.

Installieren Sie im Stammverzeichnis des Projekts die npm-Abhängigkeiten der SPA Projekte und führen Sie die Anwendung aus.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

Wenn beim Ausführen von `npm install` Fehler auftreten, führen Sie die folgenden Schritte aus:

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

Stellen Sie sicher, dass die SPA unter [http://localhost:3000](http://localhost:3000) ausgeführt wird.

## Inhaltserstellung in AEM SPA Editor

Bevor Sie Inhalte erstellen, ordnen Sie die Browser-Fenster so an, dass sich die AEM-Autoreninstanz (`http://localhost:4502`) auf der linken Seite befindet und die Remote-SPA (`http://localhost:3000`) auf der rechten Seite ausgeführt wird. Auf diese Weise können Sie sehen, wie Änderungen an AEM-bezogenen Inhalten sofort in der SPA widergespiegelt werden.

1. Melden Sie sich bei [AEM SDK-Autorendienst](http://localhost:4502) als `admin` an.
1. Navigieren Sie zu __Sites > WKND-App > us > en__
1. Bearbeiten Sie __WKND-App-Startseite__
1. Zum Modus __Bearbeiten__ wechseln

### Erstellen der festen Komponente der Startansicht

1. Tippen Sie auf den Text __WKND Adventures__ , um die feste Titelkomponente zu aktivieren (fest in der SPA-Startseite codiert).
1. Tippen Sie auf das Symbol __Schraubenschlüssel__ in der Aktionsleiste der Titelkomponente
1. Ändert den Inhalt der Titelkomponente und speichert ihn
1. Aktualisieren Sie den SPA, der auf `http://localhost:3000` ausgeführt wird, und sehen Sie, dass die Änderungen übernommen wurden.

### Container-Komponente der Home-Ansicht erstellen

1. Beim Bearbeiten der __WKND-App-Startseite__...
1. Erweitern Sie die Seitenleiste __SPA Editor__ (links).
1. Tippen Sie auf die Symbole __Komponenten__
1. Hinzufügen, Ändern oder Entfernen von Komponenten aus der Container-Komponente, die sich unter dem WKND-Logo und über der fixierten Titelkomponente befindet
1. Aktualisieren Sie den SPA, der auf `http://localhost:3000` ausgeführt wird, und sehen Sie, dass die Änderungen übernommen wurden.

### Container-Komponente auf einer dynamischen Route erstellen

1. Wechseln Sie im SPA Editor zum Modus __Vorschau__ .
1. Tippen Sie auf die Karte __Bali Surf Camp__ und navigieren Sie zur dynamischen Route
1. Fügen Sie Komponenten der Container-Komponente hinzu, ändern Sie sie oder entfernen Sie sie aus der Überschrift __Itinerary__ .
1. Aktualisieren Sie den SPA, der auf `http://localhost:3000` ausgeführt wird, und sehen Sie, dass die Änderungen übernommen wurden.

Neue AEM unter der __WKND-App-Startseite > Adventure__ _must_ haben einen AEM Seitennamen, der dem entsprechenden Inhaltsfragment des Abenteuers entspricht. Dies liegt daran, dass die SPA Route zu AEM Seitenzuordnung auf dem letzten Segment der Route basiert, nämlich dem Namen des Inhaltsfragments.

## Herzlichen Glückwunsch!

Sie haben einfach einen schnellen Überblick darüber erhalten, wie AEM SPA Editor Ihre SPA mit kontrollierten, bearbeitbaren Bereichen verbessern kann! Wenn Sie daran interessiert sind - sehen Sie sich den Rest des Tutorials an, aber stellen Sie sicher, dass Sie neu beginnen, da Ihre lokale Entwicklungsumgebung bei dieser schnellen Einrichtung jetzt den Endstatus des Tutorials aufweist!

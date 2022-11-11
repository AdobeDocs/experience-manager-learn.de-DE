---
title: Schnelles Setup SPA Editor und Remote SPA
description: Erfahren Sie, wie Sie mit einem Remote-SPA und AEM SPA-Editor in 15 Minuten loslegen können!
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '797'
ht-degree: 6%

---

# Schnelleinstellungen

Die schnelle Einrichtung ist eine schnelle Anleitung, die veranschaulicht, wie die WKND-App und als Remote-SPA installiert und ausgeführt und mit AEM SPA Editor erstellt wird.

Die schnelle Einrichtung bringt Sie direkt zum Endstatus dieses Tutorials.

>[!VIDEO](https://video.tv.adobe.com/v/333181/?quality=12&learn=on)

_Ein Video mit einer Anleitung zur schnellen Einrichtung_

## Voraussetzungen

Dieses Tutorial erfordert Folgendes:

+ [AEM as a Cloud Service-SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=de)
+ [Node.js v16+](https://nodejs.org/en/)
+ [npm v8+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ Nur macOS-Voraussetzungen
   + [Xcode](https://developer.apple.com/xcode/) oder [Xcode-Befehlszeilen-Tools](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip oder höher](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql-Quellcode (Verzweigung: feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


Dieses Tutorial setzt voraus, dass:

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) als IDE
+ Ein Arbeitsverzeichnis von `~/Code/wknd-app`
+ Ausführen des AEM SDK als Autorendienst in `http://localhost:4502`
+ Ausführen des AEM SDK mit dem lokalen `admin` Konto mit Kennwort `admin`
+ Ausführen des SPA auf `http://localhost:3000`

## Starten des AEM SDK-Schnellstarts

Laden Sie den AEM SDK-Schnellstart auf Port 4502 herunter und installieren Sie ihn, standardmäßig `admin/admin` Anmeldedaten.

1. [Laden Sie das neueste AEM SDK herunter](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. Entpacken Sie das AEM SDK in `~/aem-sdk`
1. Ausführen der AEM SDK-Schnellstart-JAR

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK startet und startet automatisch am [http://localhost:4502](http://localhost:4502). Melden Sie sich mit den folgenden Anmeldedaten an:

+ Benutzername: `admin`
+ Kennwort: `admin`

## Herunterladen und Installieren des WKND Site-Pakets

Dieses Tutorial hängt von __WKND 2.1.0+__ Projekt (für Inhalt).

1. [Laden Sie die neueste Version von herunter. `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Melden Sie sich beim Package Manager AEM SDK an unter [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) mit dem `admin` Anmeldedaten.
1. __Hochladen__ die `aem-guides-wknd.all.x.x.x.zip` heruntergeladen in Schritt 1
1. Tippen Sie auf __Installieren__ Schaltfläche für den Eintrag `aem-guides-wknd.all-x.x.x.zip`

## Herunterladen und Installieren von WKND App SPA Packages

Um eine schnelle Einrichtung durchzuführen, werden hier AEM Pakete bereitgestellt, die die endgültige AEM und den Inhalt des Tutorials enthalten.

1. [Download ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Download ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. Melden Sie sich beim Package Manager AEM SDK an unter [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) mit dem `admin` Anmeldedaten.
1. __Hochladen__ die `wknd-app.all.x.x.x.zip` heruntergeladen in Schritt 1
1. Tippen Sie auf __Installieren__ Schaltfläche für den Eintrag `wknd-app.all.x.x.x.zip`
1. __Hochladen__ die `wknd-app.ui.content.sample.x.x.x.zip` heruntergeladen in Schritt 2
1. Tippen Sie auf __Installieren__ Schaltfläche für den Eintrag `wknd-app.ui.content.sample.x.x.x.zip`

## WKND-App-Quelle herunterladen

Laden Sie den Quellcode der WKND-App von Github.com herunter und wechseln Sie die Verzweigung, die die Änderungen an der in diesem Tutorial durchgeführten SPA enthält.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## Starten Sie die SPA.

Installieren Sie im Stammverzeichnis des Projekts die npm-Abhängigkeiten der SPA Projekte und führen Sie die Anwendung aus.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

Wenn bei der Ausführung Fehler auftreten `npm install` Führen Sie die folgenden Schritte aus:

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

Stellen Sie sicher, dass der SPA ausgeführt wird unter [http://localhost:3000](http://localhost:3000).

## Inhaltserstellung in AEM SPA Editor

Bevor Sie Inhalte erstellen, ordnen Sie Ihre Browserfenster wie AEM Author (`http://localhost:4502`) auf der linken Seite und die Remote-SPA (`http://localhost:3000`) läuft auf der rechten Seite. Auf diese Weise können Sie sehen, wie Änderungen an AEM-bezogenen Inhalten sofort in der SPA widergespiegelt werden.

1. Anmelden bei [AEM SDK-Autorendienst](http://localhost:4502) as `admin`
1. Navigieren Sie zu __Sites > WKND App > us > en__
1. Bearbeiten __WKND-App-Startseite__
1. Wechseln zu __Bearbeiten__ mode

### Erstellen der festen Komponente der Startansicht

1. Tippen Sie auf den Text __WKND Adventures__ zur Aktivierung der fixen Titelkomponente (fest in der SPA-Startansicht codiert)
1. Tippen Sie auf __Schraubenschlüssel__ Symbol in der Aktionsleiste der Titel-Komponente
1. Ändert den Inhalt der Titelkomponente und speichert ihn
1. SPA aktualisieren, auf dem ausgeführt wird `http://localhost:3000` und sehen, dass die Änderungen

### Container-Komponente der Home-Ansicht erstellen

1. Während Sie die __WKND-App-Startseite__...
1. Erweitern Sie die __Seitenleiste SPA Editors__ (links)
1. Tippen Sie auf __Komponenten__ icons
1. Hinzufügen, Ändern oder Entfernen von Komponenten aus der Container-Komponente, die sich unter dem WKND-Logo und über der fixierten Titelkomponente befindet
1. SPA aktualisieren, auf dem ausgeführt wird `http://localhost:3000` und sehen, dass die Änderungen

### Container-Komponente auf einer dynamischen Route erstellen

1. Wechseln zu __Vorschau__ Modus im SPA Editor
1. Tippen Sie auf die __Bali Surf Camp__ Karte und navigieren Sie zur dynamischen Route
1. Hinzufügen, Ändern oder Entfernen von Komponenten aus der Container-Komponente, die sich über dem __Route__ heading
1. SPA aktualisieren, auf dem ausgeführt wird `http://localhost:3000` und sehen, dass die Änderungen

Neue AEM unter __WKND-App-Startseite > Abenteuer__ _must_ haben einen AEM Seitennamen, der dem Namen des entsprechenden Abenteuers Inhaltsfragment entspricht. Dies liegt daran, dass die SPA Route zu AEM Seitenzuordnung auf dem letzten Segment der Route basiert, nämlich dem Namen des Inhaltsfragments.

## Herzlichen Glückwunsch!

Sie haben einfach einen schnellen Überblick darüber erhalten, wie AEM SPA Editor Ihre SPA mit kontrollierten, bearbeitbaren Bereichen verbessern kann! Wenn Sie daran interessiert sind - sehen Sie sich den Rest des Tutorials an, aber stellen Sie sicher, dass Sie neu beginnen, da Ihre lokale Entwicklungsumgebung bei dieser schnellen Einrichtung jetzt den Endstatus des Tutorials aufweist!

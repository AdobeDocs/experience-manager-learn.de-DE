---
title: Quick Setup SPA Editor und Remote SPA
description: Erfahren Sie, wie Sie in 15 Minuten mit einem Remote-SPA und AEM SPA Editor arbeiten können!
topic: Headless, SPA, Entwicklung
feature: SPA Editor, Hauptkomponenten, APIs, Entwicklung
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: kt-7629.jpeg
translation-type: tm+mt
source-git-commit: ba45a52b9dac44f0c08c819cf4778dba83f325c9
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 5%

---


# Schnelleinrichtung

Quick Setup ist ein beschleunigter Schritt zur Installation und Ausführung der WKND-App und als Remote-SPA und zur Erstellung der Anwendung mit AEM SPA Editor.

Mit der Schnelleinrichtung gelangen Sie direkt zum Endstatus dieses Lernprogramms.

## Voraussetzungen

Dieses Lernprogramm erfordert Folgendes:

+ [AEM as a Cloud Service-SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0.zip oder höher](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql-Quellcode](https://github.com/adobe/aem-guides-wknd-graphql)

Dieses Lernprogramm setzt voraus,

+ [Microsoft® Visual Studio-](https://visualstudio.microsoft.com/) Codeas als IDE
+ Ein Arbeitsverzeichnis von `~/Code/wknd-app`
+ Ausführen des AEM SDK als Autorendienst unter `http://localhost:4502`
+ Ausführen des AEM SDK mit dem lokalen `admin`-Konto mit Kennwort `admin`
+ Ausführen des SPA auf `http://localhost:3000`

## Beginn des AEM SDK QuickStart

Laden Sie das AEM SDK QuickStart auf Port 4502 mit den Standardberechtigungen `admin/admin` herunter und installieren Sie es.

1. [Neueste AEM SDK herunterladen](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. Dekomprimieren Sie das AEM SDK nach `~/aem-sdk`
1. Ausführen der AEM SDK QuickStart-JAR

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK wird auf [http://localhost:4502](http://localhost:4502) Beginn und automatisch gestartet. Melden Sie sich mit den folgenden Anmeldedaten an:

+ Benutzername: `admin`
+ Passwort: `admin`

## Herunterladen und Installieren des WKND-Site-Pakets

Dieses Tutorial hat eine Abhängigkeit vom __WKND 0.3.0+&#39;s__ Projekt (für Inhalte).

1. [Laden Sie die neueste Version von  `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Melden Sie sich bei AEM Package Manager unter [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) mit den `admin`-Anmeldeinformationen an.
1. __Laden Sie die in Schritt 1__ heruntergeladenen Dateien  `aem-guides-wknd.all.x.x.x.zip` hoch.
1. Tippen Sie auf die Schaltfläche __Install__ für den Eintrag `aem-guides-wknd.all-x.x.x.zip`

## WKND-SPA herunterladen und installieren

Zur schnellen Einrichtung werden AEM Pakete bereitgestellt, die die endgültige AEM und den Inhalt des Tutorials enthalten.

1. [Download `wknd-app.all.x.x.x.zip`](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Herunterladen  `wknd-app.ui.content.sample.x.x.x.zip`](./assets/quick-setup/wknd-app.ui.content.sample-1.0.0.zip)
1. Melden Sie sich bei AEM Package Manager unter [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) mit den `admin`-Anmeldeinformationen an.
1. __Laden Sie die in Schritt 1__ heruntergeladenen Dateien  `wknd-app.all.x.x.x.zip` hoch.
1. Tippen Sie auf die Schaltfläche __Install__ für den Eintrag `wknd-app.all.x.x.x.zip`
1. __Laden Sie die in Schritt 2__ heruntergeladenen Dateien  `wknd-app.ui.content.sample.x.x.x.zip` hoch.
1. Tippen Sie auf die Schaltfläche __Install__ für den Eintrag `wknd-app.ui.content.sample.x.x.x.zip`

## WKND-App-Quelle herunterladen

Laden Sie den Quellcode der WKND-App von Github.com herunter und wechseln Sie in die Verzweigung, die die Änderungen an der in dieser Übung ausgeführten SPA enthält.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
$ git checkout -b feature/spa-editor
$ git pull origin feature/spa-editor
```

## Beginn des SPA

Installieren Sie aus dem Projektstamm die Abhängigkeiten der SPA Projekte npm und führen Sie die Anwendung aus.

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

Vergewissern Sie sich, dass die SPA unter [http://localhost:3000](http://localhost:3000) ausgeführt wird.

## Autoreninhalte in AEM SPA Editor

Bevor Sie Inhalte erstellen, ordnen Sie die Browser-Fenster so an, dass sich AEM Author (`http://localhost:4502`) auf der linken Seite befindet und die Remote-SPA (`http://localhost:3000`) auf der rechten Seite ausgeführt wird. Auf diese Weise können Sie sehen, wie Änderungen an AEM-bezogenen Inhalten sofort in der SPA widergespiegelt werden.

1. Melden Sie sich bei [AEM SDK-Autorendienst](http://localhost:4502) als `admin` an
1. Navigieren Sie zu __Sites > WKND-App > us > en__
1. Bearbeiten Sie __WKND-App-Startseite__
1. Zum Modus __Bearbeiten__ wechseln

### Erstellen der festen Komponente der Home-Ansicht

1. Tippen Sie auf den Text __WKND Adventures__, um die fixierte Titelkomponente zu aktivieren (fest in die SPA Home-Ansicht codiert)
1. Tippen Sie in der Aktionsleiste der Komponente &quot;Titel&quot;auf das Symbol __wSchraubenschlüssel__
1. Ändert den Inhalt der Titelkomponente und speichert
1. Aktualisieren Sie die SPA, die auf `http://localhost:3000` ausgeführt wird, und sehen Sie, dass die Änderungen übernommen wurden.

### Container der Home-Ansicht erstellen

1. Beim Bearbeiten der __WKND App-Startseite__...
1. Erweitern Sie die Seitenleiste __SPA Editor__ (links)
1. Tippen Sie auf die Symbole __Komponenten__
1. hinzufügen, Ändern oder Entfernen von Komponenten aus der Container-Komponente, die sich unter dem WKND-Logo und über der festen Titelkomponente befindet
1. Aktualisieren Sie die SPA, die auf `http://localhost:3000` ausgeführt wird, und sehen Sie, dass die Änderungen übernommen wurden.

### Erstellen einer Container-Komponente auf einer dynamischen Route

1. Im SPA Editor zu __Vorschau__ wechseln
1. Tippen Sie auf die Karte __Bali Surf Camp__ und navigieren Sie zu ihrer dynamischen Route
1. hinzufügen, Ändern oder Entfernen von Komponenten aus der Container-Komponente, die sich über der Überschrift __Itinerary__ befindet
1. Aktualisieren Sie die SPA, die auf `http://localhost:3000` ausgeführt wird, und sehen Sie, dass die Änderungen übernommen wurden.

## Herzlichen Glückwunsch!

Sie haben einfach einen schnellen Überblick darüber, wie AEM SPA Editor Ihre SPA mit kontrollierten, editierbaren Bereichen verbessern kann! Wenn Sie daran interessiert sind - schauen Sie sich die restlichen Tutorials an, aber stellen Sie sicher, dass Sie Beginn frisch, da in diesem schnellen Setup Ihre lokale Umgebung ist jetzt im Endzustand des Tutorials!

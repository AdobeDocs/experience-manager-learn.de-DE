---
title: AEM Headless-Schnelleinrichtung für AEM as a Cloud Service
description: Mit der AEM Headless-Schnelleinrichtung erhalten Sie praktische Informationen zu AEM Headless mithilfe von Inhalten aus dem WKND Site-Beispielprojekt und einer React-App, die den Inhalt über AEM Headless-GraphQL-APIs nutzt.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
source-git-commit: 94a57490edb00da072446ee8ca07c12c413ce1ac
workflow-type: tm+mt
source-wordcount: '1072'
ht-degree: 2%

---

# AEM Headless-Schnelleinrichtung für AEM as a Cloud Service

Mit der AEM Headless-Schnelleinrichtung erhalten Sie praktische Informationen zu AEM Headless mithilfe von Inhalten aus dem WKND Site-Beispielprojekt und einer React App (einem SPA), die den Inhalt über AEM Headless GraphQL-APIs nutzt.

## Voraussetzungen

Die folgenden Schritte sind erforderlich, um dieser schnellen Einrichtung zu folgen:

+ AEM as a Cloud Service Sandbox-Umgebung (vorzugsweise Entwicklung)
+ Zugriff auf AEM as a Cloud Service und Cloud Manager
   + __AEM Administrator__ Zugang zu AEM as a Cloud Service
   + __Cloud Manager - Deployment Manager__ Zugriff auf Cloud Manager
+ Die folgenden Tools müssen lokal installiert sein:
   + [Node.js v10+](https://nodejs.org/en/)
   + [npm 6+](https://www.npmjs.com/)
   + [Git](https://git-scm.com/)
   + Eine IDE (z. B. [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

## 1. Cloud Manager-Git-Repository erstellen

Erstellen Sie zunächst ein Cloud Manager-Git-Repository, das zur Bereitstellung der WKND-Site verwendet wird. Die WKND-Site ist ein AEM Website-Beispielprojekt, das Inhalte (Inhaltsfragmente) und einen GraphQL-AEM-Endpunkt enthält, der von der React-App der Schnelleinrichtung verwendet wird.

_Screencast von Schritten_
>[!VIDEO](https://video.tv.adobe.com/v/339073/?quality=12&learn=on)

1. Navigieren Sie zu [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. Cloud Manager auswählen __Programm__ , die die AEM as a Cloud Service Umgebung für diese schnelle Einrichtung enthält
1. Erstellen eines Git-Repositorys für das WKND Site-Projekt
   1. Auswählen __Repositorys__ in der oberen Navigation
   1. Auswählen __Repository hinzufügen__ in der oberen Aktionsleiste
   1. Benennen Sie das neue Git-Repository: `aem-headless-quick-setup`
   1. Auswählen __Speichern__ und warten, bis das Git-Repository initialisiert wird

## 2. Push-Beispiel für ein WKND-Site-Projekt in das Cloud Manager-Git-Repository

Nachdem das Cloud Manager-Git-Repository erstellt wurde, klonen Sie den Quellcode des WKND-Site-Projekts von GitHub und pushen Sie ihn an das Cloud Manager-Git-Repository. Jetzt kann Cloud Manager auf das WKND Site-Projekt in der AEM as a Cloud Service Umgebung zugreifen und es bereitstellen.

_Screencast von Schritten_
>[!VIDEO](https://video.tv.adobe.com/v/339074/?quality=12&learn=on)

1. Klonen Sie in der Befehlszeile den Quellcode des WKND Site-Beispielprojekts von GitHub

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Cloud Manager-Git-Repository als Remote-Domäne hinzufügen
   1. Auswählen __Repositorys__ in der oberen Navigation
   1. Auswählen __Zugriff auf Repo Info__ in der oberen Aktionsleiste
   1. Befehl &quot;Ausführen&quot;, gefunden in __Remote zu Ihrem Git-Repository hinzufügen__ über die Befehlszeile

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup/
      ```

1. Pushen Sie den Quellcode des Beispielprojekts aus Ihrem lokalen Git-Repository in das Cloud Manager-Git-Repository.

   ```shell
   $ git push adobe master:main
   ```

   Wenn Sie nach Anmeldeinformationen gefragt werden, geben Sie die __Benutzername__ und __Passwort__ von Cloud Manager __Repository-Informationen__ modal.

## 3. Stellen Sie die WKND-Site für AEM as a Cloud Service bereit.

Wenn das WKND Site-Projekt in das Cloud Manager-Git-Repository gepusht wird, kann es nicht mithilfe von Cloud Manager-Pipelines AEM as a Cloud Service bereitgestellt werden.

Beachten Sie, dass das WKND Site-Projekt Beispielinhalte bereitstellt, die die React-App über AEM Headless GraphQL-APIs nutzt.

_Screencast von Schritten_
>[!VIDEO](https://video.tv.adobe.com/v/339075/?quality=12&learn=on)

1. Anfügen eines __Bereitstellungs-Pipeline für Nicht-Produktion__ zum neuen Git-Repository
   1. Auswählen __Pipelines__ in der oberen Navigation
   1. Auswählen __Pipeline hinzufügen__ in der oberen Aktionsleiste
   1. Im __Konfiguration__ tab
      1. Auswählen __Bereitstellungs-Pipeline__ option
      1. Legen Sie die __Name der produktionsfremden Pipeline__ nach `Dev Deployment pipeline`
      1. Auswählen __Bereitstellungs-Trigger > Bei Git-Änderungen__
      1. Auswählen __Verhalten bei wichtigen Metrikfehlern > Sofort fortfahren__
      1. Klicken Sie auf __Weiter__
   1. Im __Quellcode__ tab
      1. Auswählen __Vollständiger Stack-Code__ option
      1. Wählen Sie die __AEM as a Cloud Service Entwicklungsumgebung__ von __Förderfähige Bereitstellungsumgebungen__ Auswahlfeld
      1. Auswählen `aem-headless-quick-setup` im __Repository__ Auswahlfeld
      1. Auswählen `main` von __Git-Verzweigung__ Auswahlfeld
      1. Wählen Sie __Speichern__ aus
1. Führen Sie die __Dev-Bereitstellungs-Pipeline__
   1. Auswählen __Übersicht__ in der oberen Navigation
   1. Suchen Sie die neu erstellte __Pipeline zur Dev-Implementierung__ im __Pipelines__ Abschnitt
   1. Wählen Sie die __...__ rechts vom Pipeline-Eintrag
   1. Auswählen __Ausführen__ und bestätigen Sie im Modal
   1. Wählen Sie die __...__ rechts von der derzeit ausgeführten Pipeline
   1. Auswählen __Details anzeigen__
1. Überwachen Sie in den Details der Pipeline-Ausführung den Fortschritt, bis er erfolgreich abgeschlossen wurde. Die Pipelineausführung sollte zwischen 45 und 60 Minuten dauern.

## 4. Laden Sie die WKND React-App herunter und führen Sie sie aus.

Laden Sie AEM as a Cloud Service Bootstrapping für den Inhalt aus dem WKND Site-Projekt herunter und starten Sie die WKND React App-Beispielanwendung, die den Inhalt der WKND-Site über AEM Headless GraphQL-APIs nutzt.

_Screencast von Schritten_
>[!VIDEO](https://video.tv.adobe.com/v/339076/?quality=12&learn=on)

1. Klonen Sie in der Befehlszeile den Quell-Code der React-App von GitHub aus.

   ```shell
   $ cd ~/Code
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Öffnen Sie den Ordner . `~/Code/aem-guides-wknd-graphql` in Ihrer IDE.
1. Öffnen Sie in der IDE die -Datei. `react-app/.env.development`.
1. Auf das AEM as a Cloud Service verweisen __Veröffentlichen__ Host-URI des Dienstes aus der  `REACT_APP_HOST_URI` -Eigenschaft.

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com/
   ...
   ```

   So suchen Sie den Host-URI Ihres AEM as a Cloud Service Publish-Dienstes:

   1. Wählen Sie in Cloud Manager __Umgebungen__ in der oberen Navigation
   1. Auswählen __Entwicklung__ Umgebung
   1. Suchen Sie die __Veröffentlichungsdienst (AEM und Dispatcher)__ link __Umgebungssegmente__ table
   1. Kopieren Sie die Adresse des Links und verwenden Sie sie als URI des AEM as a Cloud Service Publish-Dienstes

1. Speichern Sie in der IDE die Änderungen in `.env.development`
1. Führen Sie in der Befehlszeile die React-App aus.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Die React-App, die lokal ausgeführt wird, beginnt auf [http://localhost:3000](http://localhost:3000) und zeigt eine Liste von Abenteuern an, die aus AEM as a Cloud Service mithilfe AEM Headless-GraphQL-APIs bezogen werden.

## 5. Bearbeiten von Inhalten in AEM

Mit der WKND-React-App, die eine Verbindung zu und Nutzung von Inhalten aus den AEM Headless-GraphQL-APIs herstellt, erstellen Sie Inhalte im AEM-Autorendienst und sehen Sie, wie das Erlebnis der React-App gemeinsam aktualisiert wird.

_Screencast von Schritten_
>[!VIDEO](https://video.tv.adobe.com/v/339077/?quality=12&learn=on)

1. Bei AEM as a Cloud Service Authoring-Dienst anmelden
1. Navigieren Sie zu __Assets > Dateien > WKND > Englisch > Abenteuer__
1. Öffnen Sie die __Radfahren in Süd-Utah__ Ordner
1. Wählen Sie die __Radfahren in Süd-Utah__ Inhaltsfragment und wählen Sie __Bearbeiten__ in der oberen Aktionsleiste
1. Aktualisieren Sie einige Felder des Inhaltsfragments, z. B.:
   + Titel: `Cycling Utah's National Parks`
   + Reisedauer: `6 Days`
   + Schwierigkeit: `Intermediate`
   + Preis: `$3500`
   + Primäres Bild: `/content/dam/wknd/en/activities/cycling/mountain-biking.jpg`
1. Auswählen __Speichern__ in der oberen Aktionsleiste
1. Auswählen __Quick Publish__ in der oberen Aktionsleiste __...__
1. React-App aktualisieren, die ausgeführt wird [http://localhost:3000](http://localhost:3000).
1. Wählen Sie in der React-App die jetzt aktualisierte aus und überprüfen Sie die Inhaltsänderungen, die am Inhaltsfragment vorgenommen wurden.

1. Verwenden Sie denselben Ansatz im AEM-Autorendienst:
   1. Rückgängigmachen der Veröffentlichung eines vorhandenen Adventure-Inhaltsfragments und Überprüfen, ob es aus dem React-App-Erlebnis entfernt wurde
   1. Erstellen und veröffentlichen Sie ein neues Adventure Content Fragment und überprüfen Sie, ob es im Erlebnis &quot;React App&quot;angezeigt wird.

   >[!TIP]
   >
   > Wenn Sie nicht mit dem Erstellen und Veröffentlichen neuer oder dem Rückgängigmachen der Veröffentlichung vorhandener Inhaltsfragmente vertraut sind, sehen Sie sich den obigen Screencast an.

## Herzlichen Glückwunsch!

Herzlichen Glückwunsch! Sie haben erfolgreich AEM Headless verwendet, um eine React App zu betreiben!

Um genau zu verstehen, wie die React-App Inhalte von AEM as a Cloud Service nutzt, besuchen Sie das [AEM Headless-Tutorial](../multi-step/overview.md). In diesem Tutorial wird untersucht, wie Inhaltsfragmente in AEM erstellt wurden und wie diese React-App ihren Inhalt als JSON nutzt.

### Nächste Schritte

+ [Starten des AEM Headless-Tutorials](../multi-step/overview.md)

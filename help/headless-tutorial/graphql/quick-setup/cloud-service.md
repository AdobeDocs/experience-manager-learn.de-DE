---
title: AEM Headless-Schnelleinrichtung für AEM as a Cloud Service
description: Mit der AEM Headless-Schnelleinrichtung erhalten Sie praktische Informationen zu AEM Headless. Sie verwenden dabei Inhalte aus dem WKND-Site-Beispielprojekt und eine React-App, die Inhalte über AEM Headless-GraphQL-APIs bezieht.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
duration: 850
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '1078'
ht-degree: 100%

---

# AEM Headless-Schnelleinrichtung für AEM as a Cloud Service

Mit der AEM Headless-Schnelleinrichtung erhalten Sie praktische Informationen zu AEM Headless. Sie verwenden dabei Inhalte aus dem WKND-Site-Beispielprojekt und eine Beispiel-React-App (eine SPA), die Inhalte über AEM Headless-GraphQL-APIs bezieht.

## Voraussetzungen

Die folgenden Schritte sind erforderlich, um dieser Schnelleinrichtung zu folgen:

+ AEM as a Cloud Service-Sandbox-Umgebung (vorzugsweise Entwicklung)
+ Zugreifen auf AEM as a Cloud Service und Cloud Manager
   + __AEM-Administratorzugriff__ auf AEM as a Cloud Service
   + Zugriff auf Cloud Manager als __Cloud Manager – Bereitstellungs-Manager__
+ Die folgenden Tools müssen lokal installiert sein:
   + [Node.js v18](https://nodejs.org/de/)
   + [Git](https://git-scm.com/)
   + Eine IDE (z. B. [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

## 1. Erstellen eines Cloud Manager-Git-Repositorys

Erstellen Sie zunächst ein Cloud Manager-Git-Repository, das zur Bereitstellung der WKND-Site verwendet wird. Die WKND-Site ist ein Beispielprojekt für eine AEM-Website, das Inhalte (Inhaltsfragmente) und einen GraphQL-AEM-Endpunkt enthält, der von der React-App für die Schnelleinrichtung verwendet wird.

_Screencast von Schritten_
>[!VIDEO](https://video.tv.adobe.com/v/339073?quality=12&learn=on)

1. Navigieren Sie zu [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com).
1. Wählen Sie das Cloud Manager-__Programm__ aus, das die AEM as a Cloud Service-Umgebung für diese Schnelleinrichtung enthält
1. Erstellen Sie ein Git-Repository für das WKND-Site-Projekt.
   1. Wählen Sie __Repositorys__ im oberen Navigationsbereich aus.
   1. Wählen Sie __Repository hinzufügen__ in der oberen Aktionsleiste aus.
   1. Benennen Sie das neue Git-Repository: `aem-headless-quick-setup-wknd`
      + Git-Repository-Namen müssen pro Adobe-Organisation eindeutig sein.
   1. Wählen Sie __Speichern__ aus und warten Sie, bis das Git-Repository initialisiert wird.

## 2. Übertragen des Beispielprojekts der WKND-Site in das Cloud Manager-Git-Repository

Nachdem das Cloud Manager-Git-Repository erstellt wurde, klonen Sie den Quell-Code des WKND-Site-Projekts von GitHub und übertragen Sie ihn in das Cloud Manager-Git-Repository. Jetzt kann Cloud Manager auf das WKND-Site-Projekt in der AEM as a Cloud Service-Umgebung zugreifen und es bereitstellen.

_Screencast von Schritten_
>[!VIDEO](https://video.tv.adobe.com/v/339074?quality=12&learn=on)

1. Klonen Sie in der Befehlszeile den Quell-Code des WKND-Site-Beispielprojekts von GitHub.

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Fügen Sie das Cloud Manager-Git-Repository als Remote-Repository hinzu.
   1. Wählen Sie __Repositorys__ im oberen Navigationsbereich aus.
   1. Wählen __Zugriff auf Repo Info__ in der oberen Aktionsleiste aus.
   1. Führen Sie den Befehl in __Remote zu Ihrem Git-Repository hinzufügen__ über die Befehlszeile aus.

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. Übertragen Sie den Quell-Code des Beispielprojekts aus Ihrem lokalen Git-Repository in das Cloud Manager-Git-Repository.

   ```shell
   $ git push adobe main:main
   ```

   Wenn Sie nach Anmeldeinformationen gefragt werden, geben Sie den __Benutzernamen__ und das __Kennwort__ aus dem modalen Cloud Manager-Fenster __Repository-Informationen__ ein.

## 3. Bereitstellen der WKND-Site für AEM as a Cloud Service

Wenn das WKND-Site-Projekt in das Cloud Manager-Git-Repository übertragen wurde, kann es nicht mithilfe von Cloud Manager-Pipelines für AEM as a Cloud Service bereitgestellt werden.

Beachten Sie, dass das WKND-Site-Projekt Beispielinhalte bereitstellt, die die React-App über AEM Headless-GraphQL-APIs nutzt.

_Screencast von Schritten_
>[!VIDEO](https://video.tv.adobe.com/v/339075?quality=12&learn=on)

1. Fügen Sie eine __produktionsfremde Bereitstellungs-Pipeline__ an das neue Git-Repository an.
   1. Wählen Sie __Pipelines__ im oberen Navigationsbereich aus.
   1. Wählen Sie __Pipeline hinzufügen__ in der oberen Aktionsleiste aus.
   1. Auf der Registerkarte __Konfiguration__
      1. Wählen Sie die Option __Bereitstellungs-Pipeline__ aus.
      1. Legen Sie den __Namen der produktionsfremden Pipeline__ auf `Dev Deployment pipeline` fest.
      1. Wählen Sie __Bereitstellungsauslöser > Bei Git-Änderungen__
      1. Wählen Sie __Verhalten bei wichtigen Metrikfehlern > Sofort fortfahren__
      1. Klicken Sie auf __Weiter__
   1. Auf der Registerkarte __Quell-Code__
      1. Wählen Sie die Option __Vollständiger Stack-Code__
      1. Wählen Sie die __AEM as a Cloud Service-Entwicklungsumgebung__ aus dem Auswahlfeld __Mögliche Bereitstellungsumgebungen__
      1. Wählen Sie `aem-headless-quick-setup-wknd` im Auswahlfeld __Repository__
      1. Wählen Sie `main` aus dem Auswahlfeld __Git-Verzweigung__
      1. Wählen Sie __Speichern__ aus
1. Führen Sie die __Dev-Bereitstellungs-Pipeline__ aus
   1. Wählen Sie __Übersicht__ in der oberen Navigationsleiste
   1. Suchen Sie die neu erstellte __Dev-Bereitstellungs-Pipeline__ im __Pipelines__-Abschnitt
   1. Wählen Sie __...__ rechts vom Pipeline-Eintrag
   1. Wählen Sie __Ausführen__ und bestätigen Sie es im modalen Fenster
   1. Wählen Sie __...__ rechts von der derzeit ausgeführten Pipeline
   1. Wählen Sie __Details anzeigen__
1. Überwachen Sie ausgehend von den Details der Pipeline-Ausführung den Fortschritt, bis er erfolgreich abgeschlossen ist. Die Pipelineausführung sollte zwischen 30 und 40 Minuten dauern.

## 4. Laden Sie die WKND React-App herunter und führen Sie sie aus.

Nachdem ein Bootstrapping von AEM as a Cloud Service für den Inhalt aus dem WKND Site-Projekt durchgeführt wurde, laden Sie die WKND React App-Beispielanwendung, die den Inhalt der WKND-Site über AEM Headless GraphQL-APIs nutzt, herunter und starten Sie sie.

_Screencast von Schritten_
>[!VIDEO](https://video.tv.adobe.com/v/339076?quality=12&learn=on)

1. Klonen Sie den Quell-Code der React-App von GitHub aus der Befehlszeile.

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Öffnen Sie den Ordner `~/Code/aem-guides-wknd-graphql/react-app` in Ihrer IDE.
1. Öffnen Sie in der IDE die Datei `.env.development`.
1. Zeigen Sie auf den Host-URI des __Veröffentlichungsinstanz__-Service von AEM as a Cloud Service aus der Eigenschaft `REACT_APP_HOST_URI`.

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
   ...
   ```

   So suchen Sie Ihren Host-URI des Veröffentlichungsinstanz-Service von AEM as a Cloud Service:

   1. Wählen Sie in Cloud Manager __Umgebungen__ in der oberen Navigationsleiste
   1. Wählen Sie __Implementierungsumgebung__
   1. Suchen Sie den Link zum __Veröffentlichungsinstanz-Service (AEM und Dispatcher)__ in der Tabelle __Umgebungssegmente__
   1. Kopieren Sie die Adresse des Links und verwenden Sie sie als URI des Veröffentlichungs-Service von AEM as a Cloud Service 

1. Speichern Sie in der IDE die Änderungen in `.env.development`
1. Führen Sie in der Befehlszeile die React-App aus.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Die React-App, die lokal ausgeführt wird, beginnt auf [http://localhost:3000](http://localhost:3000) und zeigt eine Liste von Adventures an, die über GraphQL-APIs von AEM Headless aus AEM as a Cloud Service aufgerufen werden.

## 5. Bearbeiten von Inhalten in AEM

Mit der WKND-React-Beispiel-App, die eine Verbindung zu Inhalten aus den AEM Headless-GraphQL-APIs herstellt und diese verwendet, erstellen Sie Inhalte im AEM-Autoren-Service und sehen Sie, wie das Erlebnis der React-App übereinstimmend aktualisiert wird.

_Screencast von Schritten_
>[!VIDEO](https://video.tv.adobe.com/v/339077?quality=12&learn=on)

1. Melden Sie sich beim Authoring-Service von AEM as a Cloud Service an
1. Navigieren Sie zu __Assets > Dateien > WKND Shared > Englisch > Adventures__
1. Öffnen Sie den Ordner __Cycling Southern Utah__
1. Wählen Sie das Inhaltsfragment __Cycling Southern Utah__ und wählen Sie __Bearbeiten__ in der oberen Aktionsleiste
1. Aktualisieren Sie einige Felder des Inhaltsfragments, z. B.:
   + Titel: `Cycling Utah's National Parks`
   + Trip Length: `6 Days`
   + Difficulty: `Intermediate`
   + Price: `3500`
   + Primäres Bild: `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. Wählen Sie __Speichern__ in der oberen Aktionsleiste
1. Wählen Sie __Quick Publish__ aus den __...__ in der oberen Aktionsleiste
1. Aktualisieren Sie die React-App, die auf [http://localhost:3000](http://localhost:3000) ausgeführt wird.
1. Wählen Sie in der React-App das jetzt aktualisierte Cycling-Adventure aus und überprüfen Sie die Inhaltsänderungen, die am Inhaltsfragment vorgenommen wurden.

1. Verwenden Sie denselben Ansatz im AEM-Autoren-Service:
   1. Machen Sie die Veröffentlichung eines vorhandenen Adventure-Inhaltsfragments rückgängig und überprüfen Sie, ob es aus dem React-App-Erlebnis entfernt wird
   1. Erstellen und veröffentlichen Sie ein neues Adventure-Inhaltsfragment und überprüfen Sie, ob es im React-App-Erlebnis angezeigt wird

   >[!TIP]
   >
   > Wenn Sie nicht mit dem Erstellen und Veröffentlichen neuer oder dem Rückgängigmachen der Veröffentlichung vorhandener Inhaltsfragmente vertraut sind, sehen Sie sich den obigen Screencast an.

## Herzlichen Glückwunsch!

Herzlichen Glückwunsch! Sie haben erfolgreich AEM Headless verwendet, um eine React-App zu betreiben!

Um genau zu verstehen, wie die React-App Inhalte von AEM as a Cloud Service nutzt, besuchen Sie das [AEM Headless-Tutorial](../multi-step/overview.md). In diesem Tutorial wird untersucht, wie Inhaltsfragmente in AEM erstellt wurden und wie diese React-App ihren Inhalt als JSON nutzt.

### Nächste Schritte

+ [Starten des AEM Headless-Tutorials](../multi-step/overview.md)

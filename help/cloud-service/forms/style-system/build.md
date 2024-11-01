---
title: Verwenden des Stilsystems in AEM Forms
description: Erstellen des Design-Projekts
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
source-git-commit: 1ed08d7784833b6c49139da525341af5ee587345
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 81%

---


# Testen der Änderungen

Erstelle Sie ein adaptives Formular basierend auf der Vorlage **„Leer mit Kernkomponenten“**. Ziehen Sie drei Schaltflächen per Drag-and-Drop in das Formular und beschriften Sie sie mit „Unternehmen“, „Marketing“ und „Standard“.
Weisen Sie den Schaltflächen „Unternehmen“ und „Marketing“ die entsprechenden Stilvarianten zu, indem Sie den Pinsel wie dargestellt auswählen.

![styles](assets/marketing-variation.png)

Auf die dritte Schaltfläche wird der Standardstil angewendet.

## Erstellen des Design-Projekts

Der nächste Schritt besteht darin, das Design-Projekt zu erstellen. Navigieren Sie zum Stammordner Ihres Designprojekts und führen Sie den Befehl _**npm run build**_ aus, wie im Screenshot unten dargestellt.

![build-theme](assets/build-theme.png)

Sobald das Design-Projekt erfolgreich erstellt wurde, können Sie die Änderungen testen.

## Eine schnelle und einfache Möglichkeit zum Testen Ihrer CSS-Datei

* Öffnen Sie die Datei „theme.css“ im Ordner „dist“ Ihres Design-Projekts. Wählen Sie den gesamten Dateiinhalt aus und kopieren Sie ihn.
* Zeigen Sie das im vorherigen Schritt erstellte Formular in einer Vorschau an.
* Klicken Sie mit der rechten Maustaste auf eine der Schaltflächen und wählen Sie „Inspizieren“ aus, um die Developer Console zu öffnen.
* Klicken Sie in der Developer Console auf die Datei „theme.css“ , um die Datei „theme.css“ zu öffnen.
* Wählen Sie den gesamten Inhalt von „theme.css“ mithilfe von Strg+A und klicken Sie auf die Schaltfläche „Löschen“, um ihn zu löschen.
* Kopieren Sie den Inhalt von „theme.css“, den Sie im vorherigen Schritt erstellt haben, und fügen Sie ihn ein.
* Die Schaltflächen sollten wie unten gezeigt mit den entsprechenden Stilen aktualisiert werden.

![final-buttons](assets/final-state-buttons.png)

## Änderungen übernehmen

Wenn Sie mit den Änderungen zufrieden sind, können Sie die Änderungen mithilfe der [Frontend-Pipeline](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/enable-frontend-pipeline-devops/create-frontend-pipeline) an Ihre Cloud-Instanz übertragen


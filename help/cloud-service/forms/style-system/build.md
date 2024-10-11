---
title: Verwenden des Stilsystems in AEM Forms
description: Erstellen des Designprojekts
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
source-git-commit: 86d282b426402c9ad6be84e9db92598d0dc54f85
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 2%

---


# Änderungen testen

Erstellen Sie ein adaptives Formular basierend auf der Vorlage **&quot;Leer mit Kernkomponenten&quot;** . Ziehen Sie drei Schaltflächen in das Formular und beschriften Sie sie mit &quot;Unternehmen&quot;, &quot;Marketing&quot;und &quot;Standard&quot;.
Weisen Sie den Schaltflächen Unternehmen und Marketing die entsprechenden Stilvarianten zu, indem Sie den Pinsel auswählen, wie in der Abbildung dargestellt

![styles](assets/marketing-variation.png)

## Erstellen des Designprojekts

Der nächste Schritt besteht darin, das Designprojekt zu erstellen. Navigieren Sie zum Stammordner Ihres Designprojekts und führen Sie den Befehl _**npm run build**_ aus, wie im Screenshot unten gezeigt.

![build-theme](assets/build-theme.png)

Sobald das Designprojekt erfolgreich erstellt wurde, können Sie die Änderungen testen.

## Schnelle und einfache Möglichkeit, Ihre CSS zu testen

* Öffnen Sie die Datei &quot;theme.css&quot;im Ordner &quot;dist&quot;Ihres Designprojekts. Wählen Sie den gesamten Dateiinhalt aus und kopieren Sie ihn.
* Vorschau des im vorherigen Schritt erstellten Formulars
* Klicken Sie mit der rechten Maustaste auf eine der Schaltflächen und wählen Sie Inspect aus, um die Entwicklerkonsole zu öffnen.
* Klicken Sie in der Entwicklerkonsole auf die Datei theme.css , um die Datei theme.css zu öffnen.
* Wählen und löschen Sie den gesamten Inhalt von theme.css mithilfe von CTR-A und klicken Sie auf die Schaltfläche Löschen .
* Kopieren Sie den Inhalt von theme.css, den Sie im vorherigen Schritt erstellt haben, und fügen Sie ihn ein.
* Die Schaltflächen sollten wie unten gezeigt mit den entsprechenden Stilen aktualisiert werden.

![final-button](assets/final-state-buttons.png)


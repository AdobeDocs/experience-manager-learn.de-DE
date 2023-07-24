---
title: Tutorial zur Integration von Analytics mit Commerce
description: Erfahren Sie, wie Sie Analytics mit Commerce integrieren.
solution: Analytics, Commerce
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
index: true
hidefromtoc: true
kt: null
thumbnail: null
last-substantial-update: 2023-04-11T00:00:00Z
badgeIntegration: label="Integration" type="positive"
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 1%

---


# Integration von Analytics in Commerce

* Stellen Sie sicher, dass das Konto Zugriff auf Adobe Analytics hat.

* Erstellen Sie ein Projekt in Adobe Analytics.

* Erstellen Sie ein Schema.
   * Sie müssen dies aus den Optionen in späteren Schritten auswählen. Um ein Schema zu erstellen, suchen Sie in der linken Spalte unter &quot;Data Management&quot;nach Schemas. Klicken Sie nun oben links auf &quot;Schema erstellen&quot;. Wählen Sie XDM ExperienceEvent aus.
   * Klicken Sie auf der linken Seite &quot;Feldergruppen suchen&quot;auf Hinzufügen
      * Bei der Suche können Sie durch Eingabe von `ExperienceEvent Commerce`
      * Suchen nach `Adobe Analytics ExperienceEvent Commerce` und aktivieren Sie das Kontrollkästchen
      * Klicken Sie unbedingt auf die `Add field groups` oben rechts zum Speichern und Fortsetzen
* Erstellen Sie einen Datensatz. Sie benötigen diesen bei der nächsten Einrichtung des &quot;DataStream&quot;.
   * Der Datensatz befindet sich in der linken Spalte &quot;Data Management&quot;und sucht nach &quot;Datensätzen&quot;.
   * Klicken Sie dann oben rechts auf &quot;Datensatz erstellen&quot;. Sie erstellen den Datensatz aus dem Schema.
   * Suchen und Verwenden des zuvor erstellten Schemas
* Datenstrom erstellen. Sie können dazu über die &quot;Datenerfassung in der linken Spalte&quot;und die Suche nach &quot;Datastreams&quot;gelangen.
* Erstellen Sie Tabellen mit Bedienfeldern und Segmenten. Dies ist für dieses Tutorial kompliziert. Sie benötigen einen erfahrenen Analytics-Mitarbeiter, der Ihnen hilft.


Um Ihren Bericht schließlich anzuzeigen, navigieren Sie zu experience.adobe.com , suchen Sie Ihr Workspace-Projekt, klicken Sie auf den Link des Projekts, das Sie anzeigen möchten, und Sie sollten etwas wie dieses Bild sehen

![Analytics-Screenshot einiger Commerce-Daten](./assets/analytics-commerce/analytics-screenshot-commerce-items.png)
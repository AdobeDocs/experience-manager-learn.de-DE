---
title: AEM-Posteingang
description: Posteingang anpassen durch Hinzufügen neuer Spalten basierend auf Workflow-Daten
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
kt: 5830
topic: Development
role: Developer
level: Experienced
exl-id: 3e1d86ab-e0c4-45d4-b998-75a44a7e4a3f
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 5%

---

# AEM-Posteingang

AEM Posteingang konsolidiert Benachrichtigungen und Aufgaben aus verschiedenen AEM Komponenten, einschließlich Forms-Workflows. Wenn ein Arbeitsablauf für Formulare ausgelöst wird, der einen Schritt &quot;Aufgabe zuweisen&quot;enthält, wird die zugehörige Anwendung als Aufgabe im Posteingang des Empfängers aufgeführt.

Die Benutzeroberfläche des Posteingangs bietet Listen- und Kalenderansichten zum Anzeigen von Aufgaben. Sie können auch die Anzeigeeinstellungen konfigurieren. Sie können Aufgaben anhand verschiedener Parameter filtern.

Sie können den Experience Manager-Posteingang anpassen, um den Standardtitel einer Spalte zu ändern, die Spaltenposition neu anzuordnen und zusätzliche Spalten basierend auf den Daten eines Workflows anzuzeigen.

>[!NOTE]
>
>Sie müssen Administrator oder Workflow-Administratoren sein, um die Spalten des Posteingangs anzupassen.

## Spaltenanpassung

[AEM Posteingang starten](http://localhost:4502/aem/inbox)
Öffnen Sie die Admin Control, indem Sie auf die _Listenansicht_ Symbol und wählen Sie dann _Admin Control_ wie im Screenshot unten gezeigt

![Admin-Kontrolle](assets/open-customization.png)

In der Benutzeroberfläche zur Spaltenanpassung können Sie die folgenden Vorgänge ausführen

* Spalten löschen
* Neuanordnen der Spalten
* Spalten umbenennen

## Branding-Anpassung

Bei der Branding-Anpassung haben Sie folgende Möglichkeiten:

* Hinzufügen des Organisationslogos
* Kopfzeilentext anpassen
* Hilfelink anpassen
* Navigationsoptionen ausblenden

![Inbox-Branding](assets/branding-customization.PNG)

## Nächste Schritte

[Spalte &quot;Verheiratet hinzufügen&quot;](./add-married-column.md)

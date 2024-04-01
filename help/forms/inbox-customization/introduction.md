---
title: AEM-Posteingang
description: Anpassen des Posteingangs durch Hinzufügen neuer Spalten auf Basis von Workflow-Daten
feature: Adaptive Forms
doc-type: article
version: 6.5
jira: KT-5830
topic: Development
role: Developer
level: Experienced
exl-id: 3e1d86ab-e0c4-45d4-b998-75a44a7e4a3f
last-substantial-update: 2020-07-07T00:00:00Z
duration: 54
source-git-commit: adf3fe30474bcfe5fc1a1e2a8a3d49060067726d
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 87%

---

# AEM-Posteingang

Der AEM-Posteingang führt Benachrichtigungen und Aufgaben aus verschiedenen AEM-Komponenten, einschließlich Forms Workflow, zusammen. Wenn ein Forms-Workflow ausgelöst wird, der einen Schritt zur Zuweisung einer Aufgabe enthält, wird die dazugehörige Anwendung als Aufgabe im Posteingang der zugewiesenen Person angezeigt.

Die Benutzeroberfläche des Posteingangs bietet Listen- und Kalenderansichten zum Anzeigen von Aufgaben. Sie können außerdem die Einstellungen für die Anzeige konfigurieren. Sie können Aufgaben nach verschiedenen Parametern filtern.

Sie können den Experience Manager-Posteingang anpassen, indem Sie den Standardtitel einer Spalte ändern, eine Spalte neu anordnen und zusätzliche Spalten basierend auf den Daten eines Workflows anzeigen.

>[!NOTE]
>
>Sie müssen Admin oder Workflow-Admin sein, um die Spalten des Posteingangs anpassen zu können.

## Spaltenanpassung

[AEM Posteingang öffnen](http://localhost:4502/aem/inbox)
Öffnen Sie die Admin Control, indem Sie auf die _Listenansicht_ Symbol und wählen Sie dann _Admin Control_ wie im Screenshot unten gezeigt

![Admin-Kontrolle](assets/open-customization.png)

In der Benutzeroberfläche zur Spaltenanpassung können Sie die folgenden Vorgänge ausführen:

* Spalten löschen
* Spalten neu anordnen
* Spalten umbenennen

## Branding-Anpassung

Im Rahmen der Branding-Anpassung können Sie die folgenden Vorgänge ausführen:

* Logo Ihrer Organisation hinzufügen
* Kopfzeilentext anpassen
* Hilfe-Link anpassen
* Navigationsoptionen ausblenden

![Branding für den Posteingang](assets/branding-customization.PNG)

## Nächste Schritte

[Hinzufügen der Spalte „Verheiratet“](./add-married-column.md)

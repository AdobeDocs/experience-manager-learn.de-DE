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
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 29%

---

# AEM-Posteingang

AEM Posteingang konsolidiert Benachrichtigungen und Aufgaben aus verschiedenen AEM Komponenten, einschließlich Forms-Workflows. Wenn ein Forms-Workflow ausgelöst wird, der einen Schritt zur Zuweisung einer Aufgabe enthält, wird die dazugehörige Anwendung als Aufgabe im Posteingang der zugewiesenen Person angezeigt.

In der Benutzeroberfläche des Posteingangs können die Aufgaben in einer Listen- oder einer Kalenderansicht angezeigt werden. Sie können außerdem die Einstellungen für die Anzeige konfigurieren. Sie können die Aufgaben nach verschiedenen Parametern filtern.

Sie können den Experience Manager-Posteingang anpassen, um den Standardtitel einer Spalte zu ändern, die Spaltenposition neu anzuordnen und zusätzliche Spalten basierend auf den Daten eines Workflows anzuzeigen.

>[!NOTE]
>
>Sie müssen Administrator oder Workflow-Administratoren sein, um die Spalten des Posteingangs anzupassen.

## Spaltenanpassung

[AEM Posteingang starten](http://localhost:4502/aem/inbox)
Öffnen Sie die Admin Control, indem Sie auf die _Listenansicht_ Symbol und wählen Sie dann _Admin Control_ wie im Screenshot unten gezeigt

![admin-control](assets/open-customization.png)

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

---
title: Anpassung des Posteingangs
description: 'Posteingang anpassen durch Hinzufügen neuer Spalten basierend auf Workflow-Daten '
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
kt: 5830
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 25%

---

# AEM-Posteingang

AEM Posteingang konsolidiert Benachrichtigungen und Aufgaben aus verschiedenen AEM Komponenten, einschließlich Forms-Workflows. Wenn ein Forms-Workflow ausgelöst wird, der einen Schritt zur Zuweisung einer Aufgabe enthält, wird die dazugehörige Anwendung als Aufgabe im Posteingang der zugewiesenen Person angezeigt.
In der Benutzeroberfläche des Posteingangs können die Aufgaben in einer Listen- oder einer Kalenderansicht angezeigt werden. Sie können außerdem die Einstellungen für die Anzeige konfigurieren. Sie können Aufgaben anhand verschiedener Parameter filtern
Sie können den Experience Manager-Posteingang anpassen, um den Standardtitel einer Spalte zu ändern, die Spaltenposition neu anzuordnen und zusätzliche Spalten basierend auf den Daten eines Workflows anzuzeigen


>[!NOTE]
>
>Sie müssen Administrator oder Workflow-Administratoren sein, um die Spalten des Posteingangs anzupassen.

## Spaltenanpassung

[Launch AEM ](http://localhost:4502/aem/inbox)
inboxÖffnen Sie das Admin Control, indem Sie auf das  _Listen-_ Viewer-Symbol klicken und dann  _Admin_ Control auswählen, wie im Screenshot unten dargestellt

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

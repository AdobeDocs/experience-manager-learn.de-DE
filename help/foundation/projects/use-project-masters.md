---
title: Verwendung von Projekt-Mastern in AEM
description: Projekt-Master vereinfacht das Benutzer- und Teammanagement mit AEM Projekten erheblich.
version: 6.4, 6.5, cloud-service
topic: Content Management, Zusammenarbeit
feature: Projekte
level: Intermediate
role: Business Practitioner
kt: 256
thumbnail: 17740.jpg
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '372'
ht-degree: 0%

---


# Verwenden von Projekt-Mastern

Projekt-Master vereinfachen die Benutzer- und Teamverwaltung mit [!DNL AEM Projects] erheblich.

>[!VIDEO](https://video.tv.adobe.com/v/17740/?quality=12&learn=on)

Administratoren können jetzt ein **[!DNL Master Project]** erstellen und Benutzer im Rahmen eines Projektteams Rollen/Berechtigungen zuweisen. Projekte können aus einem Übergeordneten Projekt erstellt werden und übernehmen automatisch die Team-Mitgliedschaft. Dies bietet mehrere Vorteile:

* Wiederverwenden vorhandener Teams für mehrere Projekte
* Beschleunigt die Projekterstellung, da Teams nicht von Hand neu erstellt werden müssen
* Team-Mitglieder von einem zentralen Speicherort aus verwalten. Aktualisierungen an Teams werden automatisch von Projekten übernommen
* verhindert die Erstellung doppelter ACLs, die Leistungsprobleme verursachen können

[!DNL Master Projects] kann im Ordner   Mastersordner unter  [!UICONTROL AEM Projekte] erstellt werden. Nachdem ein Übergeordnetes Projekt erstellt wurde, wird es bei der Erstellung neuer Projekte neben den verfügbaren Vorlagen im Assistenten als Option angezeigt.

[!DNL Project Masters] URL (lokale AEM-Autoreninstanz):  [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## Löschen [!DNL Project Masters]

Das Löschen eines Übergeordneten Projekts führt zu unbrauchbaren abgeleiteten Projekten.

Bevor Sie ein Übergeordnetes Projekt löschen, stellen Sie sicher, dass alle abgeleiteten Projekte beendet und aus AEM entfernt wurden. Achten Sie darauf, alle erforderlichen Projektdaten zu speichern, bevor Sie die abgeleiteten Projekte entfernen. Sobald alle abgeleiteten Projekte aus AEM entfernt wurden, kann das Übergeordnete Projekt sicher gelöscht werden.

## Markieren Sie [!DNL Project Masters] als Inaktiv

Indem Sie den Status des Übergeordneten Projekts in den Projekteigenschaften in inaktiv ändern, werden die inaktiven Übergeordneten Projekte aus der Liste der Übergeordneten Projekte ausgeblendet.

Um inaktive Übergeordnete Projekte anzuzeigen, schalten Sie die Schaltfläche &quot;Aktiv anzeigen&quot;in der oberen Leiste (neben dem Umschalter für die Listenanzeige) um. Um das inaktive Projekt erneut zu aktivieren, wählen Sie einfach das inaktive Übergeordnete Projekt aus, bearbeiten Sie die Projekteigenschaften und legen Sie es erneut auf &quot;aktiv&quot;fest.

## [!DNL Project Masters] verstehen

![Technische Ansicht der Projekt-Master](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] arbeiten, indem Sie eine Reihe AEM Benutzergruppen (Eigentümer, Editor und Beobachter) definieren und es abgeleiteten Projekten ermöglichen, auf diese zentral definierten Benutzergruppen zu verweisen und sie wiederzuverwenden.

Dadurch wird die Gesamtzahl der für AEM erforderlichen Benutzergruppen reduziert. Vor [!DNL Project Masters] erstellte jedes Projekt 3 Benutzergruppen mit den zugehörigen ACEs, um die Berechtigungsprüfung durchzusetzen. Daher erreichten 100 Projekte 300 Benutzergruppen. Mit Projekt-Mastern können beliebig viele Projekte dieselben drei Gruppen wiederverwenden, vorausgesetzt, die freigegebene Mitgliedschaft entspricht den geschäftlichen Anforderungen des gesamten Projekts.

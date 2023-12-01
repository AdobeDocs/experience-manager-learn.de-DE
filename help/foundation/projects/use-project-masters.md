---
title: Verwenden von Primärprojekten in AEM
description: Primärprojekte vereinfachen das Benutzer- und Team-Management mit AEM Projekten erheblich.
version: 6.4, 6.5, Cloud Service
topic: Content Management, Collaboration
feature: Projects
level: Intermediate
role: User
jira: KT-256
thumbnail: 17740.jpg
doc-type: Feature Video
exl-id: 78ff62ad-1017-4a02-80e9-81228f9e01eb
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 100%

---

# Verwenden von Primärprojekten

Primärprojekte vereinfachen das Benutzer- und Team-Management mit [!DNL AEM Projects] erheblich.

>[!VIDEO](https://video.tv.adobe.com/v/17740?quality=12&learn=on)

Admins können jetzt ein **[!DNL Master Project]** erstellen und Benutzende zu Rollen/Berechtigungen als Teil eines Projekt-Teams zuweisen. Projekte können aus einem Primärprojekt erstellt werden und automatisch die Team-Mitgliedschaft übernehmen. Das hat mehrere Vorteile, denn es ermöglicht:

* Wiederverwenden vorhandener Teams über mehrere Projekte hinweg
* schnellere Projekterstellung, da Teams nicht manuell neu erstellt werden müssen
* zentrale Verwaltung der Team-Mitgliedschaft und automatische Übernahme von Team-Aktualisierungen bei Projekten
* Vermeidung doppelt erstellter ACLs, die Leistungsprobleme verursachen können

[!DNL Master Projects] können im Ordner [!UICONTROL Masters] unter [!UICONTROL AEM-Projekte] erstellt werden. Sobald ein Primärprojekt erstellt wurde, wird es bei der Erstellung neuer Projekte neben den verfügbaren Vorlagen im Assistenten als Option angezeigt.

[!DNL Project Masters]-URL (lokale AEM-Autoreninstanz): [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## Löschen von [!DNL Project Masters]

Das Löschen eines Primärprojekts führt zu unbrauchbaren abgeleiteten Projekten.

Bevor Sie ein Primärprojekt löschen, stellen Sie sicher, dass alle abgeleiteten Projekte beendet sind und aus AEM entfernt wurden. Achten Sie darauf, alle erforderlichen Projektdaten zu speichern, bevor Sie die abgeleiteten Projekte entfernen. Sobald alle abgeleiteten Projekte aus AEM entfernt wurden, kann das Primärprojekt sicher gelöscht werden.

## Markieren von [!DNL Project Masters] als inaktiv

Indem Sie den Status des Primärprojekts in den Projekteigenschaften zu inaktiv ändern, werden die inaktiven Primärprojekte nicht mehr in der Liste der Primärprojekte angezeigt.

Um inaktive Primärprojekte anzuzeigen, schalten Sie die Filterschaltfläche „Aktive anzeigen“ in der oberen Leiste (neben dem Umschalter für die Listenanzeige) ein. Um das inaktive Projekt erneut zu aktivieren, wählen Sie einfach das inaktive Primärprojekt aus, bearbeiten Sie die Projekteigenschaften und legen Sie es erneut als aktiv fest.

## Grundlegendes zu [!DNL Project Masters]

![Technische Ansicht der Primärprojekte](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] funktionieren wie folgt: Es wird eine Reihe von AEM-Benutzergruppen (Besitzende, Bearbeitende und Beobachtende) definiert und abgeleiteten Projekten ermöglicht, auf diese zentral definierten Benutzergruppen zu verweisen und sie wiederzuverwenden.

Dadurch wird die Gesamtzahl der in AEM erforderlichen Benutzergruppen reduziert. Bevor es [!DNL Project Masters] gab, wurden für jedes Projekt drei Benutzergruppen mit den zugehörigen ACEs erstellt, um die Berechtigungsprüfung zu erzwingen, sodass bei 100 Projekten 300 Benutzergruppen vorhanden waren. Mit Primärprojekten können beliebig viele Projekte dieselben drei Gruppen wiederverwenden, sofern die freigegebene Mitgliedschaft den geschäftlichen Anforderungen des gesamten Projekts entspricht.

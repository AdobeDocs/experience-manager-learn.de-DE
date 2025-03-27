---
title: Erstellen einer Dropdown-Listen-Komponente für Länder
description: Erstellen Sie eine Dropdown-Listen-Komponente für Länder, basierend auf einer AEM Forms-Dropdown-Kernkomponente.
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: aef151bc-daf1-4abd-914a-6299f3fb58e4
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '245'
ht-degree: 100%

---

# Erstellen einer Dropdown-Komponente für Länder basierend auf einer Dropdown-Komponente

Das Erstellen einer neuen Kernkomponente in Adobe Experience Manager (AEM) ist ein aufregender Prozess, der mehrere Schritte umfasst, darunter das Definieren der Komponentenstruktur, das Anpassen des Dialogfelds und das Implementieren eines Sling-Modells für dynamische Funktionen.

Am Ende dieses Tutorials werden Sie Folgendes beherrschen:

* Erstellen und Verwenden eines Sling-Modells zum dynamischen Abrufen von Daten.
* Anpassen des cq-dialog durch Hinzufügen neuer Felder und Ausblenden anderer.
* Definieren einer robusten, für die Wiederverwendung geeigneten Komponentenstruktur.

Die Komponente mit dem Namen „Länder“ ermöglicht es Benutzenden, einen Kontinent auszuwählen und ein Dropdown-Menü mit den Ländern des ausgewählten Kontinents dynamisch zu füllen. Dieses wird auf der Grundlage der vordefinierten Dropdown-Listen-Komponente erstellt, die für diesen speziellen Anwendungsfall erweitert wird.

Lassen Sie uns eintauchen und diese dynamische und leistungsstarke Komponente erstellen!

## Voraussetzungen

Um eine neue Kernkomponente in Adobe Experience Manager (AEM) zu erstellen, müssen mehrere Voraussetzungen erfüllt sein, damit ein reibungsloser Entwicklungsprozess gewährleistet ist. Bevor Sie beginnen, benötigen Sie Folgendes:

* Eine AEM-Entwicklungsumgebung: eine funktionsfähige, Cloud-fähige Installation, die lokal ausgeführt wird
* Zugriff auf AEM Entwicklungs-Tools wie Visual Studio Code oder IntelliJ
* Maven-Einrichtung und ein AEM-Projekt mit neuestem Archetyp
* Grundlegende Kenntnisse der AEM-Konzepte
* Grundlegende Tools und Einrichtung wie Git-Repository, richtige JDK-Version


## Nächste Schritte

[Erstellen einer Komponentenstruktur](./component.md)

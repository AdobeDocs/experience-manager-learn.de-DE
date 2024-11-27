---
title: Dropdown-Listenkomponente "Länder"erstellen
description: Erstellen Sie eine Dropdown-Listenkomponente für Länder basierend auf einer AEM Forms-Core-Dropdown-Komponente.
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
source-git-commit: f9a1fb40aabb6fdc1157e1f2576f9c0d9cf1b099
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 3%

---

# Erstellen einer Dropdown-Komponente für Länder basierend auf einer Dropdown-Komponente

Das Erstellen einer neuen Kernkomponente in Adobe Experience Manager (AEM) ist ein aufregender Prozess, der mehrere Schritte umfasst, darunter das Definieren der Komponentenstruktur, das Anpassen des Dialogfelds und das Implementieren eines Sling-Modells für dynamische Funktionen.

Am Ende dieses Tutorials haben Sie folgende Möglichkeiten beherrscht:

* Erstellen und verwenden Sie ein Sling-Modell, um Daten dynamisch abzurufen.
* Passen Sie das cq-dialog an, indem Sie neue Felder hinzufügen und andere ausblenden.
* Definieren Sie eine robuste Komponentenstruktur, die auf die Wiederverwendung zugeschnitten ist.

Die Komponente mit dem Namen &quot;Länder&quot;ermöglicht es Benutzern, einen Kontinent auszuwählen und dynamisch ein Dropdown-Menü mit Ländern auszufüllen, die dem gewählten Kontinent entsprechen. Diese wird auf der Grundlage der vordefinierten Dropdown-Listenkomponente erstellt, die für diesen speziellen Anwendungsfall erweitert wird.

Lassen Sie uns eintauchen und diese dynamische und leistungsstarke Komponente erstellen!

## Voraussetzungen

Um eine neue Kernkomponente in Adobe Experience Manager (AEM) zu erstellen, müssen mehrere Voraussetzungen erfüllt sein, damit ein reibungsloser Entwicklungsprozess gewährleistet ist. Folgendes müssen Sie vor dem Einstieg benötigen:

* AEM Entwicklungsumgebung: Eine funktionsfähige Cloud-fähige Installation, die lokal ausgeführt wird
* Zugriff auf AEM Entwicklungs-Tools wie Visual Studio Code oder IntelliJ
* MAven-Einrichtung und AEM Projekt mit neuestem Archetyp
* Grundlegendes zu AEM Konzepten
* Grundlegende Tools und Einrichtung wie Git-Repository, richtige JDK-Version


## Nächste Schritte

[Komponentenstruktur erstellen](./component.md)

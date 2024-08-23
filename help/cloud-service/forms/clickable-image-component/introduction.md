---
title: Erstellen einer klickbaren Bildkomponente
description: Erstellen einer klickbaren Bildkomponente im AEM Forms-Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: 426020f59c7103829b7b7b74acb0ddb7159b39fa
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 5%

---

# Einführung

Die Verwendung anklickbarer Bilder in Forms kann zu einem ansprechenderen, intuitiveren und visuell ansprechenderen Benutzererlebnis führen. Für diesen Artikel haben wir SVG für anklickbare Bilder verwendet, da es mehrere Vorteile bietet, insbesondere hinsichtlich Designflexibilität, Leistung und Benutzerfreundlichkeit.
SVG kann mit Adobe Illustrator oder einem der kostenlosen Online-Tools erstellt werden. Ich habe die [USA-Karte von](https://simplemaps.com/resources/svg-us)simplemaps verwendet, um den Anwendungsfall zu demonstrieren.

## Anwendungsbeispiel für die Verwendung einer klickbaren US-Karte

Die anklickbare Karte der USA ermöglicht es Benutzern, staatenspezifische Formularübermittlungen zu untersuchen. Wenn ein Benutzer auf einen Status klickt, werden die Übermittlungen dieses Status mit der Option zum Öffnen einer bestimmten Übermittlung aufgelistet.

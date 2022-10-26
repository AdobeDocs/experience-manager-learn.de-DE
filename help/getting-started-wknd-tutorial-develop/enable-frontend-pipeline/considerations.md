---
title: Entwicklungsaspekte
description: Berücksichtigen Sie die Auswirkungen auf den Front-End- und Back-End-Entwicklungsprozess, sobald Sie die Front-End-Pipeline aktivieren.
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
source-git-commit: de2fa2e4c29ce6db31233ddb1abc66a48d2397a6
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---


# Entwicklungsaspekte

Nachdem Sie die Frontend-Pipeline so aktiviert haben, dass sie nur die Frontend-Ressourcen in AEM as a Cloud Service Umgebung bereitstellt, wirkt sich dies auf die lokale AEM-Entwicklung aus und Sie müssen das Git-Verzweigungsmodell anpassen.

## Ziel

* Einen reibungslosen Front-End- und Back-End-Entwicklungsablauf
* Überprüfen Sie die Abhängigkeiten zwischen der Vollstapel- und der Frontend-Pipeline.


## Überlegungen zur lokalen Entwicklung

>[!VIDEO](https://video.tv.adobe.com/v/3409421/)


## Angepasster Entwicklungsansatz

* Für die lokale Entwicklung mit AEM SDK benötigt das Back-End-Entwicklungsteam weiterhin die clientlib-Generierung über `ui.frontend` -Modul, müssen Sie es jedoch während der Cloud Manager-Bereitstellung in AEM as a Cloud Service Umgebung überspringen. Dies stellt eine Herausforderung dar, wie die in der [Projekt aktualisieren](update-project.md) Kapitel.

A __Lösung__ kann sein, Ihr Git-Verzweigungsmodell anzupassen und sicherzustellen, dass die Konfigurationsänderungen des AEM nie wieder in die __lokale Entwicklung__ Verzweigung der AEM Back-End-Entwickler.


* Wenn Sie im Rahmen einer kontinuierlichen Verbesserung Ihres AEM-Projekts neue Komponenten einführen oder eine vorhandene Komponente aktualisieren, die Änderungen an beiden aufweist `ui.app` und `ui.frontend` -Modul, müssen Sie sowohl Vollstapel- als auch Front-End-Pipelines ausführen.




---
title: Entwicklungsaspekte
description: Berücksichtigen Sie die Auswirkungen auf den Frontend- und Backend-Entwicklungsprozess, sobald Sie die Frontend-Pipeline aktivieren.
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: a3b27d5b-b167-4c60-af49-8f2e8d814c86
duration: 99
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 100%

---

# Entwicklungsaspekte

Nachdem Sie die Frontend-Pipeline so aktiviert haben, dass sie nur die Frontend-Ressourcen in der AEM as a Cloud Service-Umgebung bereitstellt, wirkt sich dies auf die lokale AEM-Entwicklung aus, und Sie müssen das Git-Verzweigungsmodell anpassen.

## Ziel

* Wie Sie einen reibungslosen Frontend- und Backend-Entwicklungsablauf erhalten
* Überprüfen Sie die Abhängigkeiten zwischen der Full-Stack- und der Frontend-Pipeline.


## Überlegungen zur lokalen Entwicklung

>[!VIDEO](https://video.tv.adobe.com/v/3409421?quality=12&learn=on)


## Angepasster Entwicklungsansatz

* Für die lokale Entwicklung mit dem AEM SDK benötigt das Backend-Entwicklungs-Team weiterhin die Clientlib-Generierung über das `ui.frontend`-Modul, Sie müssen dies jedoch während der Bereitstellung von Cloud Manager in einer AEM as a Cloud Service-Umgebung überspringen. Dies wirft die Frage auf, wie die im Kapitel [Aktualisieren des Projekts](update-project.md) beschriebenen Änderungen der Projektkonfiguration isoliert werden können.

Eine __Lösung__ könnte darin bestehen, Ihr Git-Verzweigungsmodell anzupassen und sicherzustellen, dass die Änderungen an der AEM-Projektkonfiguration niemals in den __lokalen Entwicklungszweig__ zurückfließen, den die AEM-Backend-Entwicklerinnen und -Entwickler verwenden.


* Wenn Sie im Rahmen einer fortlaufenden Verbesserung Ihres AEM-Projekts neue Komponenten einführen oder eine vorhandene Komponente aktualisieren, die sowohl im `ui.app`- als auch im `ui.frontend`-Modul Änderungen aufweist, müssen Sie sowohl die Full-Stack- als auch die Frontend-Pipelines ausführen.

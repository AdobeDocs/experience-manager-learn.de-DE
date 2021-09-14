---
title: Debugging von AEM als Cloud Service
description: auf der Self-Service-, skalierbaren, Cloud-Infrastruktur, die es AEM Entwicklern erforderlich macht, zu verstehen, wie sie verschiedene Facetten von AEM als Cloud Service verstehen und debuggen können, von der Erstellung und Bereitstellung bis hin zum Abrufen von Details zu laufenden AEM-Anwendungen.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 1%

---

# Debugging von AEM als Cloud Service

AEM as a Cloud Service ist die Cloud-native Methode zur Nutzung der AEM. AEM ein Cloud Service auf einer Self-Service-, skalierbaren, Cloud-Infrastruktur ausgeführt wird. Dazu müssen AEM Entwickler verstehen, wie sie verschiedene Facetten von AEM als Cloud Service verstehen und debuggen können, von der Erstellung und Bereitstellung bis hin zum Abrufen von Details AEM ausgeführten Anwendungen.

## Protokolle

Protokolle enthalten Details zur Funktionsweise Ihrer Anwendung in AEM as a Cloud Service sowie Einblicke in Probleme mit Bereitstellungen.

[Debugging von AEM als Cloud Service mithilfe von Protokollen](./logs.md)

## Erstellen und Bereitstellen

Adobe Cloud Manager-Pipelines stellen AEM Anwendung in einer Reihe von Schritten bereit, um die Codequalität und -fähigkeit bei der Bereitstellung in AEM as a Cloud Service zu ermitteln. Jeder der Schritte kann zu Fehlern führen. Daher ist es wichtig, zu verstehen, wie Builds debuggt werden, um die eigentliche Ursache für Fehler zu ermitteln und wie etwaige Fehler zu beheben sind.

[Debugging von AEM als Cloud Service-Build und -Bereitstellung](./build-and-deployment.md)

## Entwicklerkonsole

Die Entwicklerkonsole bietet eine Vielzahl von Informationen und Einleitungen in AEM as a Cloud Service-Umgebungen, die nützlich sind, um zu verstehen, wie Ihre Anwendung von erkannt wird und in AEM als Cloud Service funktioniert.

[Debugging von AEM als Cloud Service mit der Developer Console](./developer-console.md)

## CRXDE Lite

CRXDE Lite ist ein klassisches, aber leistungsstarkes Tool zum Debugging von AEM als Cloud Service-Entwicklungsumgebungen. CRXDE Lite bietet eine Reihe von Funktionen, die das Debugging von der Überprüfung aller Ressourcen und Eigenschaften, der Bearbeitung der veränderlichen Teile des JCR, der Berechtigungsprüfung und der Auswertung von Abfragen unterstützen.

[Debugging von AEM als Cloud Service mit CRXDE Lite](./crxde-lite.md)

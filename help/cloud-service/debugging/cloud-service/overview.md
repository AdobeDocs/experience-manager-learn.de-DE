---
title: Debuggen von AEM als Cloud Service
description: in der Self-Service-, Skalierbaren, Cloud-Infrastruktur, was es erfordert, dass AEM Entwickler verstehen, wie verschiedene Facetten von AEM als Cloud Service zu verstehen und zu debuggen sind, von der Erstellung und Bereitstellung bis zum Abrufen von Details über die Ausführung AEM Anwendungen.
feature: Entwicklertools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
topic: Entwicklung
role: Entwickler
level: Anfänger, Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 2%

---


# Debuggen von AEM als Cloud Service

AEM als Cloud Service ist die Cloud-native Möglichkeit, die AEM Anwendungen zu nutzen. AEM ein Cloud Service auf einer selbstständigen, skalierbaren Cloud-Infrastruktur ausgeführt wird, was es erfordert, dass AEM Entwickler verstehen, wie sie verschiedene Facetten von AEM als Cloud Service verstehen und debuggen können, von der Erstellung und Bereitstellung bis zum Abrufen von Details zum Ausführen AEM von Anwendungen.

## Protokolle

Protokolle enthalten Details zur Funktionsweise Ihrer Anwendung in AEM als Cloud Service sowie Einblicke in Probleme mit Bereitstellungen.

[Debuggen von AEM als Cloud Service mithilfe von Protokollen](./logs.md)

## Erstellen und Bereitstellen

Adobe Cloud Manager-Pipeline stellt AEM Anwendung in einer Reihe von Schritten bereit, um die Codequalität und -fähigkeit bei der Bereitstellung auf AEM als Cloud Service zu bestimmen. Jeder dieser Schritte kann zu Fehlern führen. Es ist daher wichtig, zu verstehen, wie Builds debuggt werden, um die Ursache für Fehler zu ermitteln und wie Fehler zu beheben sind.

[Debuggen von AEM als Cloud Service-Build und Bereitstellung](./build-and-deployment.md)

## Developer Console

Die Developer Console bietet eine Reihe von Informationen und Anleitungen zu AEM als Cloud Service-Umgebung, die nützlich sind, um zu verstehen, wie Ihre Anwendung erkannt wird und wie sie in AEM als Cloud Service funktioniert.

[Debuggen von AEM als Cloud Service mit der Developer Console](./developer-console.md)

## CRXDE Lite

CRXDE Lite ist ein klassisches, aber leistungsfähiges Tool zum Debugging von AEM als Umgebung zur Cloud Service-Entwicklung. CRXDE Lite bietet eine Reihe von Funktionen, die das Debugging von der Überprüfung aller Ressourcen und Eigenschaften, der Manipulation der veränderlichen Teile der JCR, der Überprüfung von Berechtigungen und der Auswertung von Abfragen unterstützen.

[Debuggen von AEM als Cloud Service mit CRXDE Lite](./crxde-lite.md)

---
title: Debugging von AEM as a Cloud Service
description: auf einer selbstbedienungsfähigen und skalierbaren Cloud-Infrastruktur. Dies setzt voraus, dass AEM-Entwicklerinnen und -Entwickler wissen, wie sie die verschiedenen Facetten von AEM as a Cloud Service verstehen und debuggen können, von der Erstellung und Bereitstellung bis hin zum Erhalten von Details der laufenden AEM-Anwendungen.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
duration: 64
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 100%

---

# Debugging von AEM as a Cloud Service

AEM as a Cloud Service ist die Cloud-native Methode zur Nutzung der AEM-Anwendungen. AEM as a Cloud Service wird auf einer selbstbedienungsfähigen und skalierbaren Cloud-Infrastruktur ausgeführt. Dies setzt voraus, dass AEM-Entwicklerinnen und Entwickler wissen, wie sie die verschiedenen Facetten von AEM as a Cloud Service verstehen und debuggen können, von der Erstellung und Bereitstellung bis hin zum Erhalten von Details der laufenden AEM-Anwendungen.

## Protokolle

Die Protokolle geben Details dazu, wie Ihre Anwendung in AEM als Cloud Service funktioniert, und geben Aufschluss über Probleme bei der Bereitstellung.

[Debugging von AEM as a Cloud Service mithilfe von Protokollen](./logs.md)

## Erstellung und Bereitstellung

Die Adobe Cloud Manager-Pipelines stellen die AEM-Anwendung in einer Reihe von Schritten bereit, um die Code-Qualität und die Lebensfähigkeit bei der Bereitstellung in AEM as a Cloud Service zu bestimmen. Jeder dieser Schritte kann zu Fehlern führen. Daher ist es wichtig zu wissen, wie man Builds debuggt, um die Ursache von Fehlern zu ermitteln und diese zu beheben.

[Debugging bei der Erstellung und Bereitstellung von AEM as a Cloud Service](./build-and-deployment.md)

## Developer Console

Die Developer Console bietet eine Vielzahl von Informationen und Einblicken in AEM as a Cloud Service-Umgebungen, die nützlich sind, um zu verstehen, wie Ihre Anwendung von AEM as a Cloud Service erkannt wird und darin funktioniert.

[Debugging von AEM as a Cloud Service mit der Developer Console](./developer-console.md)

## Repository-Browser

Der Repository-Browser ist ein leistungsfähiges Tool, das Einblick in den zugrunde liegenden Datenspeicher von AEM bietet und die Fehlersuche in einer AEM as a Cloud Service-Umgebung erleichtert. Der Repository-Browser unterstützt eine schreibgeschützte Ansicht der Ressourcen und Eigenschaften von AEM in den Bereichen Produktion, Staging und Entwicklung sowie Autoren-, Veröffentlichungs- und Vorschau-Service.

[Debugging von AEM as a Cloud Service mit dem Repository-Browser](./repository-browser.md)

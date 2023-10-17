---
title: Einführung in das Speichern und Abrufen von Formulardaten aus der MySQL-Datenbank
description: Mehrteiliges Tutorial, das Sie durch die Schritte zum Speichern und Abrufen von Formulardaten führt
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 95795102-4278-4556-8e0f-1b8a359ab093
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: ht
source-wordcount: '165'
ht-degree: 100%

---

# Speichern und Abrufen von adaptiven Formulardaten aus der MySQL-Datenbank

In diesem Tutorial werden Sie durch die Schritte geführt, die zum Speichern und Abrufen von adaptiven Formulardaten aus der Datenbank erforderlich sind. In diesem Tutorial wurde die MySQL-Datenbank zum Speichern von adaptiven Formulardaten verwendet. Eine Datenbank Ihrer Wahl kann zum Speichern der Daten verwendet werden, solange Sie die datenbankspezifischen Treiber in AEM bereitgestellt haben. Auf hoher Ebene sind die folgenden Schritte erforderlich, um den Anwendungsfall zu erzielen:

* Verwendung der GuideBridge-API für den Zugriff auf die Daten des adaptiven Formulars

* Durchführung eines POST-Aufrufs an ein Servlet.  Dieses Servlet speichert die Daten in der Datenbank. Die gespeicherten Daten sind einer GUID zugeordnet

* Wenn Sie das adaptive Formular mit den gespeicherten Daten füllen wollen, rufen Sie die mit der GUID verbundenen Daten ab und füllen das adaptive Formular mit der Methode **request.setAttribute**.

## Demonstration des Anwendungsfalls

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=12&learn=on)



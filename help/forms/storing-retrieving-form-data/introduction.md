---
title: Einführung in das Speichern und Abrufen von Formulardaten aus der MySQL-Datenbank
description: Mehrteiliges Tutorial, das Sie durch die Schritte führt, die zum Speichern und Abrufen von Formulardaten erforderlich sind
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 95795102-4278-4556-8e0f-1b8a359ab093
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 0%

---

# Speichern und Abrufen von adaptiven Formulardaten aus der MySQL-Datenbank

In diesem Tutorial werden Sie durch die Schritte geführt, die zum Speichern und Abrufen von adaptiven Formulardaten aus der Datenbank erforderlich sind. In diesem Tutorial wurde die MySQL-Datenbank zum Speichern von adaptiven Formulardaten verwendet. Datenbank Ihrer Wahl kann zum Speichern der Daten verwendet werden, solange Sie die datenbankspezifischen Treiber in AEM bereitgestellt haben. Auf hoher Ebene sind die folgenden Schritte erforderlich, um den Anwendungsfall zu erreichen:

* Zugriff auf die Daten des adaptiven Formulars über die GuideBridge-API erhalten

* Führen Sie einen POST-Aufruf an ein Servlet durch. Dieses Servlet speichert die Daten in der Datenbank. Die gespeicherten Daten sind mit einer GUID verknüpft.

* Wenn Sie das adaptive Formular mit den gespeicherten Daten füllen möchten, rufen Sie die mit der GUID verknüpften Daten ab und füllen das adaptive Formular mit der **request.setAttribute** -Methode.

## Nachweis des Anwendungsfalls

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=12&learn=on)

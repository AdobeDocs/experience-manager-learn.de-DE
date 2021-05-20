---
title: Speichern und Abrufen von Formulardaten aus der MySQL-Datenbank
description: Mehrteiliges Tutorial, das Sie durch die Schritte führt, die zum Speichern und Abrufen von Formulardaten erforderlich sind
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 1%

---


# Speichern und Abrufen von adaptiven Formulardaten aus der MySQL-Datenbank

In diesem Tutorial werden Sie durch die Schritte geführt, die zum Speichern und Abrufen von adaptiven Formulardaten aus der Datenbank erforderlich sind. In diesem Tutorial wurde die MySQL-Datenbank zum Speichern von adaptiven Formulardaten verwendet. Datenbank Ihrer Wahl kann zum Speichern der Daten verwendet werden, solange Sie die datenbankspezifischen Treiber in AEM bereitgestellt haben. Auf hoher Ebene sind die folgenden Schritte erforderlich, um den Anwendungsfall zu erreichen:

* Zugriff auf die Daten des adaptiven Formulars über die GuideBridge-API erhalten

* Führen Sie einen POST-Aufruf an ein Servlet durch. Dieses Servlet speichert die Daten in der Datenbank. Die gespeicherten Daten sind mit einer GUID verknüpft.

* Wenn Sie das adaptive Formular mit den gespeicherten Daten füllen möchten, rufen Sie die mit der GUID verknüpften Daten ab und füllen das adaptive Formular mit der Methode **request.setAttribute** aus.

## Nachweis des Anwendungsfalls

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=9&learn=on)

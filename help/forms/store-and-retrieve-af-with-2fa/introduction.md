---
title: Speichern und Abrufen von Formulardaten mit Anhängen aus der MySQL-Datenbank
description: Mehrteilige Anleitung, um Sie durch die Schritte zu führen, die zum Speichern und Abrufen von Formulardaten mit Anhängen erforderlich sind
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 4%

---


# Speichern und Abrufen von adaptiven Formulardaten mit 2FA

In diesem Tutorial werden Sie durch die Schritte geführt, die zum Speichern und Abrufen von adaptiven Formulardaten mit Anhängen mit 2FA erforderlich sind. In diesem Tutorial wurde die MySQL-Datenbank zum Speichern von adaptiven Formulardaten verwendet. Datenbank Ihrer Wahl kann zum Speichern der Daten verwendet werden, solange Sie die datenbankspezifischen Treiber in AEM bereitgestellt haben. Auf hoher Ebene sind die folgenden Schritte erforderlich, um den Anwendungsfall zu erreichen:

* Zugriff auf die Daten des adaptiven Formulars über die GuideBridge-API erhalten

* Führen Sie einen POST-Aufruf an ein Servlet durch. Dieses Servlet speichert die Daten in der Datenbank und die Formularanlagen im CRX-Repository. Die in der Datenbank gespeicherten Daten sind einer GUID zugeordnet.

* Wenn Sie das adaptive Formular mit den gespeicherten Daten füllen möchten, rufen Sie die mit der GUID verknüpften Daten ab und füllen das adaptive Formular mit der Methode **request.setAttribute** aus.

## Nachweis des Anwendungsfalls

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)

## Voraussetzungen

Die Zielgruppe dieses Inhalts wird voraussichtlich über Erlebnisse in den folgenden Bereichen verfügen:

* Adaptives Formular
* Formulardatenmodell
* OSGi-Dienste/-Komponenten
* AEM Client-Bibliotheken

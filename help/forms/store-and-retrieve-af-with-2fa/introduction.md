---
title: Speichern und Abrufen von Formulardaten mit Anhängen aus der MySQL-Datenbank
description: Mehrteilige Anleitung, um Sie durch die Schritte zu führen, die zum Speichern und Abrufen von Formulardaten mit Anhängen erforderlich sind
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
topic: Development
role: Developer
level: Experienced
exl-id: b278652f-6c09-4abc-b92e-20bfaf2e791a
last-substantial-update: 2020-11-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 2%

---

# Speichern und Abrufen von adaptiven Formulardaten mit 2FA

In diesem Tutorial werden Sie durch die Schritte geführt, die zum Speichern und Abrufen von adaptiven Formulardaten mit Anhängen mit 2FA erforderlich sind. In diesem Tutorial wurde die MySQL-Datenbank zum Speichern von adaptiven Formulardaten verwendet. Datenbank Ihrer Wahl kann zum Speichern der Daten verwendet werden, solange Sie die datenbankspezifischen Treiber in AEM bereitgestellt haben. Auf hoher Ebene sind die folgenden Schritte erforderlich, um den Anwendungsfall zu erreichen:

* Zugriff auf die Daten des adaptiven Formulars über die GuideBridge-API erhalten

* Führen Sie einen POST-Aufruf an ein Servlet durch. Dieses Servlet speichert die Daten in der Datenbank und die Formularanlagen im CRX-Repository. Die in der Datenbank gespeicherten Daten sind einer GUID zugeordnet.

* Wenn Sie das adaptive Formular mit den gespeicherten Daten füllen möchten, rufen Sie die mit der GUID verknüpften Daten ab und füllen das adaptive Formular mit der **request.setAttribute** -Methode.

## Nachweis des Anwendungsfalls

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)

## Voraussetzungen

Die Zielgruppe dieses Inhalts wird voraussichtlich über Erlebnisse in den folgenden Bereichen verfügen:

* Adaptives Formular
* Formulardatenmodell
* OSGi-Dienste/-Komponenten
* AEM Client-Bibliotheken

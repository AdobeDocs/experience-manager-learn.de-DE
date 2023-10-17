---
title: Speichern und Abrufen von Formulardaten mit Anhängen aus der MySQL-Datenbank
description: Mehrteiliges Tutorial, das Sie durch die Schritte zum Speichern und Abrufen von Formulardaten mit Anhängen führt
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
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: ht
source-wordcount: '212'
ht-degree: 100%

---

# Speichern und Abrufen von adaptiven Formulardaten mit 2FA

In diesem Tutorial werden Sie durch die Schritte geführt, die zum Speichern und Abrufen von adaptiven Formulardaten mit Anhängen mit 2FA erforderlich sind. In diesem Tutorial wurde die MySQL-Datenbank zum Speichern von adaptiven Formulardaten verwendet. Eine Datenbank Ihrer Wahl kann zum Speichern der Daten verwendet werden, solange Sie die datenbankspezifischen Treiber in AEM bereitgestellt haben. Auf hoher Ebene sind die folgenden Schritte erforderlich, um den Anwendungsfall zu erzielen:

* Verwendung der GuideBridge-API für den Zugriff auf die Daten des adaptiven Formulars

* Durchführung eines POST-Aufrufs an ein Servlet.  Dieses Servlet speichert die Daten in der Datenbank und die Formularanlagen im CRX-Repository. Die in der Datenbank gespeicherten Daten sind einer GUID zugeordnet.

* Wenn Sie das adaptive Formular mit den gespeicherten Daten füllen wollen, rufen Sie die mit der GUID verbundenen Daten ab und füllen das adaptive Formular mit der Methode **request.setAttribute**.

## Demonstration des Anwendungsfalls

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)

## Voraussetzungen

Die Zielgruppe dieses Inhalts wird voraussichtlich über Erfahrungen in den folgenden Bereichen verfügen:

* Adaptives Formular
* Formulardatenmodell
* OSGi-Dienste/Komponenten
* AEM Client-Bibliotheken


## Nächste Schritte

[Konfigurieren der Datenquelle](./configure-data-source.md)
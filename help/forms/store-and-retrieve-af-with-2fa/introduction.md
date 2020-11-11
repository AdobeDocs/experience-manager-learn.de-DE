---
title: Speichern und Abrufen von Formulardaten mit Anlagen aus der MySQL-Datenbank
description: Mehrteilige Übung, um Sie durch die Schritte zum Speichern und Abrufen von Formulardaten mit Anlagen zu führen
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 2%

---


# Speichern und Abrufen adaptiver Formulardaten mit 2FA

Dieses Lernprogramm führt Sie durch die Schritte zum Speichern und Abrufen adaptiver Formulardaten mit Anlagen mit 2FA. In diesem Lernprogramm wurde die MySQL-Datenbank verwendet, um Daten des adaptiven Formulars zu speichern. Datenbank Ihrer Wahl kann verwendet werden, um die Daten zu speichern, solange Sie die datenbankspezifischen Treiber in AEM bereitgestellt haben. Auf hoher Ebene sind die folgenden Schritte erforderlich, um den Verwendungsfall zu erreichen:

* Verwenden Sie die GuideBridge-API, um Zugriff auf die Daten des adaptiven Formulars zu erhalten

* Führen Sie einen POST-Aufruf an ein Servlet aus. Dieses Servlet speichert die Daten in der Datenbank und die Formularanlagen im CRX-Repository. Die in der Datenbank gespeicherten Daten sind mit einer GUID verknüpft.

* Wenn Sie das adaptive Formular mit den gespeicherten Daten füllen möchten, rufen Sie die mit der GUID verknüpften Daten ab und füllen das adaptive Formular mit der **Methode request.setAttribute** .

## Nachweis des Anwendungsfalls

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)

## Voraussetzungen

Die Audience dieses Inhalts wird voraussichtlich einige Erfahrungen in den folgenden Bereichen aufweisen:

* Adaptives Formular
* Formulardatenmodell
* OSGi-Dienste/Komponenten
* AEM Client-Bibliotheken

---
title: Speichern und Abrufen von Formulardaten aus der MySQL-Datenbank
description: Lernprogramm mit mehreren Teilen, um Sie durch die Schritte zum Speichern und Abrufen von Formulardaten zu führen
feature: adaptive Formulare
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 0%

---


# Speichern und Abrufen adaptiver Formulardaten aus der MySQL-Datenbank

Dieses Lernprogramm führt Sie durch die Schritte zum Speichern und Abrufen adaptiver Formulardaten aus der Datenbank. In diesem Lernprogramm wurde die MySQL-Datenbank verwendet, um Daten des adaptiven Formulars zu speichern. Datenbank Ihrer Wahl kann verwendet werden, um die Daten zu speichern, solange Sie die datenbankspezifischen Treiber in AEM bereitgestellt haben. Auf hoher Ebene sind die folgenden Schritte erforderlich, um den Verwendungsfall zu erreichen:

* Verwenden Sie die GuideBridge-API, um Zugriff auf die Daten des adaptiven Formulars zu erhalten

* Führen Sie einen POST-Aufruf an ein Servlet aus. Dieses Servlet speichert die Daten in der Datenbank. Die gespeicherten Daten sind mit einer GUID verknüpft

* Wenn Sie das adaptive Formular mit den gespeicherten Daten füllen möchten, rufen Sie die mit der GUID verknüpften Daten ab und füllen das adaptive Formular mit der Methode **request.setAttribute**.

## Nachweis des Anwendungsfalls

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=9&learn=on)

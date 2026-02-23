---
title: Erstellen eines Formulardatenmodells für ein IC-Dokument
description: Erfahren Sie, wie Sie ein Formulardatenmodell in AEM Forms erstellen, um Daten für interaktive Kommunikationsdokumente dynamisch abzurufen.
version: Experience Manager as a Cloud Service
feature: Interactive Communication
role: Developer
level: Intermediate
doc-type: Feature Video
duration: 170
last-substantial-update: 2026-02-20T00:00:00Z
jira: KT-20353
source-git-commit: c2dde214df0dabe8d856751a9d16afb1423e7450
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 2%

---


# Erstellen eines Formulardatenmodells für ein IC-Dokument

Erstellen Sie ein Forms-Datenmodell, um externe Datenquellen mit der interaktiven Kommunikation in Adobe AEM zu integrieren. Dieser Prozess umfasst das Einrichten eines RESTful-Services, das Hochladen einer Swagger-Datei und das Konfigurieren von Service-Endpunkten zum dynamischen Abrufen und Binden von Daten. Erfahren Sie, wie Sie eine sichere Verbindung zu externen Services herstellen und das Modell testen können, um einen erfolgreichen Datenabruf sicherzustellen.

Es wurde ein Pseudo-API-Server implementiert, der den Auftrags-Service zu Entwicklungs- und Testzwecken simuliert. Sie stellt einen Endpunkt bereit, um Bestellungen für einen bestimmten Benutzer abzurufen (z. B. nach Benutzer-ID) und dabei vordefinierte oder dynamisch generierte Bestellungsdaten im selben Schema wie die Produktions-API zurückzugeben.

Die Swagger-Datei, die bei der Erstellung des Formulardatenmodells verwendet wurde, kann [hier heruntergeladen werden](assets/UsersAndOrders.json)

>[!VIDEO](https://video.tv.adobe.com/v/3480026/?captions=ger&learn=on&enablevpops)

## Nächste Schritte

[Erstellen einer Vorlage](./create-template.md)

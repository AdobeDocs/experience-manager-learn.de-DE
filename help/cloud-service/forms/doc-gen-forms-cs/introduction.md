---
title: Microservices zur Dokumenterstellung in AEM Forms CS
description: Verwenden Sie die Document Generation-Microservices von einer externen Anwendung.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
feature: Document Services
topic: Entwicklung
kt: 7432
thumbnail: 332439.jpg
source-git-commit: 33cb3d18b744d9a8e54a87152079b42ed09212f2
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 4%

---

# Einführung

In diesem Kurs werden wir die Dokumenterstellungsmikrodienste verwenden, um ein PDF zu generieren, indem wir Daten mit einer XDP-Vorlage zusammenführen. Die Verwendung dieser Microservices über eine externe Anwendung umfasst die folgenden Schritte:

1. Dienstanmeldeinformationen für ein AEM technisches Konto generieren
1. Erstellen Sie ein JSON-Web-Token (JWT) aus den Dienstanmeldeinformationen und tauschen Sie dasselbe für ein Zugriffstoken aus.
1. Zugriff für das technische Konto konfigurieren in AEM
1. HTTP-Aufrufe mithilfe des Zugriffstokens durchführen

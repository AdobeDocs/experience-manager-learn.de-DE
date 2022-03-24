---
title: PDF-Manipulation in Forms CS mithilfe des Vorgangs "invoke DDX"
description: Zusammenführen von PDF-Dateien mithilfe von invoke DDX.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9980
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 1%

---

# Einführung

In diesem Kurs werden wir die PDF-Manipulation und Archivierung von PDF-Dokumenten mit Forms CS verwenden. Die Verwendung dieser Microservices über eine externe Anwendung umfasst die folgenden Schritte:

1. Dienstanmeldeinformationen für ein AEM technisches Konto generieren
1. Erstellen Sie ein JSON-Web-Token (JWT) aus den Dienstanmeldeinformationen und tauschen Sie dasselbe für ein Zugriffstoken aus.
1. Zugriff für das technische Konto konfigurieren in AEM
1. Führen Sie HTTP-Aufrufe mithilfe des Zugriffstokens aus, um PDF-Dateien zu bearbeiten/PDF/A-Dateien zu generieren und zu validieren

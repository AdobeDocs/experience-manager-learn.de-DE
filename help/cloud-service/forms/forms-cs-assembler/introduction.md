---
title: Veränderung von PDF-Dateien in Forms CS mithilfe des Vorgangs „DDX aufrufen“
description: Stellen Sie PDF-Dateien mithilfe des Vorgangs „DDX aufrufen“ zusammen.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9980
exl-id: 713c4e9e-95ac-48e1-a7fc-2b3ec0b145e5
source-git-commit: e925b9fa02dc8d4695b85377c5f7f43fbd45ebc8
workflow-type: ht
source-wordcount: '96'
ht-degree: 100%

---

# Einführung

Dieser Kurs behandelt das Verändern von PDF-Dateien und Archivieren von PDF-Dokumenten mit Forms CS. Die Verwendung dieser Microservices über eine externe Anwendung umfasst die folgenden Schritte:

1. Erstellen von Service-Anmeldeinformationen für ein technisches AEM-Konto
1. Erstellen eines JSON-Web-Tokens (JWT) anhand der Service-Anmeldeinformationen und Ersetzen dieses Tokens durch ein Zugriffs-Token
1. Konfigurieren des Zugriffs für das technische Konto in AEM
1. Ausführen von HTTP-Aufrufen mithilfe des Zugriffs-Tokens, um PDF-Dateien zu ändern bzw. PDF/A-Dateien zu generieren und zu validieren

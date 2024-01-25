---
title: Veränderung von PDF-Dateien in Forms CS mithilfe des Vorgangs „DDX aufrufen“
description: Stellen Sie PDF-Dateien mithilfe des Vorgangs „DDX aufrufen“ zusammen.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9980
exl-id: 713c4e9e-95ac-48e1-a7fc-2b3ec0b145e5
duration: 19
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 100%

---

# Einführung

Dieser Kurs behandelt das Verändern von PDF-Dateien und Archivieren von PDF-Dokumenten mit Forms CS. Die Verwendung dieser Microservices über eine externe Anwendung umfasst die folgenden Schritte:

1. Erstellen von Service-Anmeldeinformationen für ein technisches AEM-Konto
1. Erstellen eines JSON-Web-Tokens (JWT) anhand der Service-Anmeldeinformationen und Ersetzen dieses Tokens durch ein Zugriffs-Token
1. Konfigurieren des Zugriffs für das technische Konto in AEM
1. Ausführen von HTTP-Aufrufen mithilfe des Zugriffs-Tokens, um PDF-Dateien zu ändern bzw. PDF/A-Dateien zu generieren und zu validieren

---
title: React-App mit AEM Forms und Acrobat Sign
description: Acrobat Sign und AEM Forms ermöglichen die Automatisierung komplexer Transaktionen und die Einbeziehung legaler E-Signaturen als Teil einer nahtlosen digitalen Erfahrung.
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 64172af3-2905-4bc8-8311-68c2a70fb39e
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 1%

---

# AEM Forms mit Acrobat Sign-Webformular


Dieses Tutorial führt Sie durch den Anwendungsfall des Generierens eines interaktiven Kommunikationsdokuments mit den Daten, die von der [React](https://react.dev/) App erstellen und das generierte Dokument zum Signieren mit dem Acrobat Sign-Webformular präsentieren.

Im Folgenden wird der Ablauf des Anwendungsfalls beschrieben:

* Benutzer füllt ein Formular in der React-App aus.
* Die Formulardaten werden an einen AEM Forms-Endpunkt gesendet, um ein interaktives Kommunikationsdokument zu generieren.
* Erstellen Sie die Acrobat Sign-Widget-URL mit dem generierten Dokument.
* Präsentieren Sie die Widget-URL der aufrufenden Anwendung, damit der Benutzer das Dokument signieren kann.

## Voraussetzungen

Sie benötigen Folgendes, damit der Anwendungsfall funktioniert:

* Ein AEM-Server mit Forms-Add-on im Paket
* Ein [Integrationsschlüssel für eine Acrobat Sign-Anwendung](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html)

## Nächste Schritte

Schreiben Sie eine [benutzerdefinierter OSGi-Dienst zum Generieren des Dokuments für interaktive Kommunikation](./create-ic-document.md) Verwendung der dokumentierten API

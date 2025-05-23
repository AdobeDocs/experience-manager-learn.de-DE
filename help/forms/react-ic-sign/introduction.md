---
title: React-App mit AEM Forms und Acrobat Sign
description: Acrobat Sign und AEM Forms ermöglichen die Automatisierung komplexer Transaktionen und die Einbeziehung legaler E-Signaturen als Teil einer nahtlosen digitalen Erfahrung.
feature: Adaptive Forms,Acrobat Sign
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 64172af3-2905-4bc8-8311-68c2a70fb39e
duration: 31
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '169'
ht-degree: 100%

---

# AEM Forms mit Acrobat Sign-Web-Formular


Dieses Tutorial führt Sie durch den Anwendungsfall des Generierens eines interaktiven Kommunikationsdokuments mit den Daten, die von der [React](https://react.dev/)-App erstellt wurden, und der Präsentation des generierten Dokuments zum Signieren mit dem Acrobat Sign-Web-Formular.

Im Folgenden wird der Ablauf des Anwendungsfalls beschrieben

* Benutzende füllen ein Formular in der React-App aus.
* Die Formulardaten werden an einen AEM Forms-Endpunkt gesendet, um ein interaktives Kommunikationsdokument zu generieren.
* Erstellen Sie eine Acrobat Sign-Widget-URL mit dem generierten Dokument.
* Präsentieren Sie die Widget-URL der aufrufenden Anwendung, damit die Benutzenden das Dokument signieren können.

## Voraussetzungen

Sie benötigen Folgendes, damit der Anwendungsfall funktioniert:

* Einen AEM-Server mit Forms-Add-on-Paket
* Einen [Integrationsschlüssel für eine Acrobat Sign-Anwendung](https://helpx.adobe.com/de/sign/kb/how-to-create-an-integration-key.html)

## Nächste Schritte

Schreiben Sie einen [benutzerdefinierten OSGi-Dienst zum Generieren des Dokuments für interaktive Kommunikation](./create-ic-document.md) mithilfe der dokumentierten API

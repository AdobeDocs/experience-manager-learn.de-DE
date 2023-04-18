---
title: React-App mit AEM Forms und Acrobat Sign
description: Acrobat Sign und AEM Forms ermöglichen die Automatisierung komplexer Transaktionen und die Einbeziehung legaler E-Signaturen als Teil eines nahtlosen digitalen Erlebnisses.
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
source-git-commit: 155e6e42d4251b731d00e2b456004016152f81fe
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

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


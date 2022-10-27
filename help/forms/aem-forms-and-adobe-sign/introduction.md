---
title: Verwenden von AEM Forms mit Adobe Sign
description: Adobe Sign und AEM Forms ermöglichen die Automatisierung komplexer Transaktionen und die Einbeziehung legaler E-Signaturen als Teil eines nahtlosen digitalen Erlebnisses.
feature: Adaptive Forms,Adobe Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 0be61f04-c3ed-4ecb-bab7-73fd308c14e0
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 41%

---

# Verwenden von AEM Forms mit Adobe Sign

Adobe Sign aktiviert Arbeitsabläufe für E-Signaturen für adaptive Formulare. E-Signaturen verbessern die Workflows bei der Verarbeitung von Dokumenten in den Bereichen Recht, Vertrieb, Gehaltsabrechnung, Personalverwaltung u. v. a..
Die Integration zwischen AEM Forms und Adobe Sign ermöglicht Ihnen Folgendes:

* Verwenden Sie Adaptive Forms, um Daten zu erfassen und automatisch generiertes Datensatzdokument (DoR) für Signaturen anzuzeigen.
* Erstellen Sie Adaptive Forms basierend auf Ihrer PDF-Vorlage. Zusammenführen der Daten mit der PDF-Vorlage und Anzeigen derselben für Unterschriften
* Dokumente zum Signieren mit der Workflow-Komponente &quot;Dokument unterschreiben&quot;senden

## Voraussetzungen

Um Adobe Sign mit AEM Forms zu integrieren, benötigen Sie Folgendes:

* Einen SSL aktivierten AEM Forms-Server
* Ein gültiges Adobe Sign-Entwicklerkonto.
* Eine Adobe Sign API-Anwendung
* Anmeldeinformationen (Client-ID und Client Secret) der Adobe Sign API-Anwendung.

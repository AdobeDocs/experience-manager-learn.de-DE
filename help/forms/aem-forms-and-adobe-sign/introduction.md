---
title: Verwenden von AEM Forms mit Adobe Sign
description: Adobe Sign und AEM Forms ermöglichen die Automatisierung komplexer Transaktionen und die Einbeziehung legaler E-Signaturen als Teil eines nahtlosen digitalen Erlebnisses.
feature: Adaptive Forms,Adobe Sign
version: 6.4,6.5
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '163'
ht-degree: 40%

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


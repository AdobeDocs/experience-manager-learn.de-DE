---
title: Verwenden von AEM Forms mit Acrobat Sign
description: Acrobat Sign und AEM Forms ermöglichen die Automatisierung komplexer Transaktionen und die Einbeziehung legaler E-Signaturen als Teil eines nahtlosen digitalen Erlebnisses.
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 0be61f04-c3ed-4ecb-bab7-73fd308c14e0
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 15%

---

# Verwenden von AEM Forms mit Acrobat Sign

Acrobat Sign ermöglicht Workflows für die elektronische Signatur für adaptive Formulare. E-Signaturen verbessern die Workflows bei der Verarbeitung von Dokumenten in den Bereichen Recht, Vertrieb, Gehaltsabrechnung, Personalverwaltung u. v. a..
Die Integration zwischen AEM Forms und Acrobat Sign ermöglicht Ihnen Folgendes:

* Verwenden Sie Adaptive Forms, um Daten zu erfassen und automatisch generiertes Datensatzdokument (DoR) für Signaturen anzuzeigen.
* Erstellen Sie Adaptive Forms basierend auf Ihrer PDF-Vorlage. Zusammenführen der Daten mit der PDF-Vorlage und Anzeigen derselben für Unterschriften
* Dokumente zum Signieren mit der Workflow-Komponente &quot;Dokument unterschreiben&quot;senden

## Voraussetzungen

Sie benötigen Folgendes, um Acrobat Sign in AEM Forms zu integrieren:

* Einen SSL aktivierten AEM Forms-Server
* Ein aktives Acrobat Sign-Entwicklerkonto.
* Eine Acrobat Sign API-Anwendung
* Anmeldeinformationen (Client-ID und Client Secret) der Acrobat Sign-API-Anwendung.

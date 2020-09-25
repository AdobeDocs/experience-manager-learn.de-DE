---
title: Verwenden von AEM Forms mit Adobe Sign
description: Adobe Sign und AEM Forms ermöglichen die Automatisierung komplexer Transaktionen und die Einbeziehung legaler E-Signaturen als Teil einer nahtlosen digitalen Erfahrung.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: c60a46027cc8d71fddd41aa31dbb569e4df94823
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 41%

---

# Verwenden von AEM Forms mit Adobe Sign

Adobe Sign aktiviert Arbeitsabläufe für E-Signaturen für adaptive Formulare. E-Signaturen verbessern die Workflows bei der Verarbeitung von Dokumenten in den Bereichen Recht, Vertrieb, Gehaltsabrechnung, Personalverwaltung u. a.
Die Integration zwischen AEM Forms und Adobe Sign ermöglicht Ihnen Folgendes:

* Verwenden Sie Adaptives Forms, um Daten zu erfassen und automatisch generiertes Dokument aus Datensatz(DoR) für Unterschriften vorzustellen
* Erstellen Sie adaptives Forms basierend auf Ihrer PDF-Vorlage. Daten mit der PDF-Vorlage zusammenführen und für Unterschriften gleich darstellen
* Senden von Dokumenten zum Signieren mit der Workflow-Komponente &quot;Dokument signieren&quot;

## Voraussetzungen

Um Adobe Sign mit AEM Forms zu integrieren, benötigen Sie Folgendes:

* Einen SSL aktivierten AEM Forms-Server
* Ein gültiges Adobe Sign-Entwicklerkonto. 
* Eine Adobe Sign API-Anwendung
* Anmeldeinformationen (Client-ID und Client Secret) der Adobe Sign API-Anwendung.


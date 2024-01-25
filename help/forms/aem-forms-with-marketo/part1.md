---
title: Integrieren von AEM Forms und Marketo
description: Erfahren Sie, wie Sie AEM Forms und Marketo mithilfe des AEM Forms-Formulardatenmodells integrieren.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
exl-id: 45047852-4fdb-4702-8a99-faaad7213b61
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
last-substantial-update: 2020-03-20T00:00:00Z
duration: 83
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 100%

---

# Integrieren von AEM Forms und Marketo

Marketo, Teil von Adobe, bietet Software für die Marketing-Automatisierung an, die sich auf kontobasiertes Marketing konzentriert, einschließlich E-Mail, mobile und soziale Medien, digitale Anzeigen, Webmanagement und Analysen.

Mit dem Formulardatenmodell von AEM Forms können wir jetzt AEM-Formular nahtlos in Marketo integrieren.

[Weitere Informationen zum Formulardatenmodell](https://helpx.adobe.com/de/experience-manager/6-5/forms/using/data-integration.html)

Marketo stellt eine REST-API bereit, die die Remote-Ausführung vieler Systemfunktionen ermöglicht. Von der Erstellung von Programmen bis hin zum Massenimport von Leads gibt es viele Optionen, die eine detaillierte Steuerung einer Marketo-Instanz ermöglichen. Mithilfe des Formulardatenmodells ist es ganz einfach, AEM Forms in Marketo zu integrieren.

In diesem Tutorial werden Sie durch die Schritte geführt, die zur Integration von AEM Forms mit Marketo mithilfe des Formulardatenmodells erforderlich sind. Nach Abschluss des Tutorials verfügen Sie über ein OSGi-Bundle, das die benutzerdefinierte Authentifizierung für Marketo durchführt. Außerdem haben Sie die Datenquelle mithilfe der bereitgestellten Swagger-Datei konfiguriert.

Für den Einstieg wird dringend empfohlen, dass Sie mit den folgenden Themen im Abschnitt „Voraussetzung“ vertraut sind.

## Voraussetzung

1. [AEM-Server mit installiertem AEM Forms-Add-on-Paket](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. Lokale AEM-Entwicklungsumgebung
1. Kenntnisse zum Formulardatenmodell
1. Grundlegendes Wissen zu Swagger-Dateien
1. Erstellen von adaptiven Formularen

**Client Secret ID und Client Secret Key**

Der erste Schritt bei der Integration von Marketo mit AEM Forms besteht darin, die API-Anmeldeinformationen zu erhalten, die für REST-Aufrufe über die API erforderlich sind. Sie benötigen Folgendes:

1. client_id
1. client_secret
1. identity_endpoint
1. Authentifizierungs-URL

[Folgen Sie der offiziellen Marketo-Dokumentation, um die oben genannten Eigenschaften zu erhalten.](https://developers.marketo.com/rest-api/) Alternativ können Sie sich auch an den Admin Ihrer Marketo-Instanz wenden.

**Bevor Sie beginnen**

[Laden Sie die mit diesem Artikel verknüpften Assets herunter und entpacken Sie sie.](assets/aemformsandmarketo.zip) Die Zip-Datei enthält Folgendes:

1. BlankTemplatePackage.zip: Dies ist die Vorlage für adaptive Formulare. Importieren Sie dies mit dem Package Manager.
1. marketo.json: Dies ist die Swagger-Datei, die zum Konfigurieren der Datenquelle verwendet wird.
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar: Dies ist das Paket, das die benutzerdefinierte Authentifizierung durchführt. Sie können dies nutzen, wenn Sie die Anleitung nicht abschließen können oder Ihr Paket nicht wie erwartet funktioniert.

## Nächste Schritte

[Erstellen einer benutzerdefinierten Authentifizierung](./part2.md)

---
title: AEM Forms mit Marketo (Teil 1)
seo-title: AEM Forms mit Marketo (Teil 1)
description: Tutorial zur Integration von AEM Forms mit Marketo mithilfe des AEM Forms-Formulardatenmodells.
seo-description: Tutorial zur Integration von AEM Forms mit Marketo mithilfe des AEM Forms-Formulardatenmodells.
feature: Adaptives Forms, Formulardatenmodell
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 1%

---


# AEM Forms mit Marketo

Marketo, ein Teil der Adobe, bietet Marketingautomatisierungssoftware, die sich auf kontobasiertes Marketing konzentriert, darunter E-Mail, mobile, soziale, digitale Anzeigen, Webmanagement und Analysen.

Mit dem Formulardatenmodell von AEM Forms können wir jetzt AEM Formular nahtlos in Marketo integrieren.

[Weitere Informationen zum Formulardatenmodell](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo stellt eine REST-API bereit, die die Remote-Ausführung vieler Systemfunktionen ermöglicht. Von der Erstellung von Programmen bis hin zum Massenimport von Leads gibt es viele Optionen, die eine detaillierte Steuerung einer Marketo-Instanz ermöglichen. Mithilfe des Formulardatenmodells ist es ganz einfach, AEM Forms in Marketo zu integrieren.

In diesem Tutorial werden Sie durch die Schritte geführt, die zur Integration von AEM Forms mit Marketo mithilfe des Formulardatenmodells erforderlich sind. Nach Abschluss des Tutorials verfügen Sie über ein OSGi-Bundle, das die benutzerdefinierte Authentifizierung für Marketo durchführt. Außerdem haben Sie die Datenquelle mithilfe der bereitgestellten Swagger-Datei konfiguriert.

Für den Einstieg wird dringend empfohlen, dass Sie mit den folgenden Themen im Abschnitt Voraussetzung vertraut sind.

## Voraussetzung

1. [AEM Server mit installiertem AEM Forms Add on Package](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. Lokale AEM Entwicklungsumgebung
1. Mit dem Formulardatenmodell vertraut
1. Grundlegendes zu Swagger-Dateien
1. Erstellen des adaptiven Forms

**Client Secret ID und Client Secret Key**

Der erste Schritt bei der Integration von Marketo mit AEM Forms besteht darin, die API-Anmeldeinformationen abzurufen, die zum Ausführen der REST-Aufrufe mithilfe der API erforderlich sind. Sie benötigen Folgendes:

1. client_id
1. client_secret
1. identity_endpoint
1. Authentifizierungs-URL.

[Folgen Sie der offiziellen Marketo-Dokumentation, um die oben genannten Eigenschaften zu erhalten.](https://developers.marketo.com/rest-api/) Alternativ können Sie sich auch an den Administrator Ihrer Marketo-Instanz wenden.

**Voraussetzungen**

[Laden Sie die mit diesem Artikel verknüpften Assets herunter und entpacken Sie sie.](assets/aemformsandmarketo.zip) Die ZIP-Datei enthält Folgendes:

1. BlankTemplatePackage.zip - Dies ist die Vorlage für adaptive Formulare. Importieren Sie dies mit dem Package Manager.
1. marketo.json - Dies ist die Swagger-Datei, die zum Konfigurieren der Datenquelle verwendet wird.
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar - Dies ist das Bundle, das die benutzerdefinierte Authentifizierung durchführt. Sie können dies gerne verwenden, wenn Sie das Tutorial nicht abschließen können oder Ihr Bundle nicht wie erwartet funktioniert.

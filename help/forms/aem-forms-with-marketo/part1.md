---
title: AEM Forms mit Marketo (Teil 1)
seo-title: AEM Forms mit Marketo (Teil 1)
description: Lernprogramm zur Integration von AEM Forms in Marketing mit dem AEM Forms-Formulardatenmodell.
seo-description: Lernprogramm zur Integration von AEM Forms in Marketing mit dem AEM Forms-Formulardatenmodell.
feature: adaptive Formulare, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 1%

---


# AEM Forms mit Marketo

Marketo, ein Teil der Adobe, bietet Marketingautomatisierungssoftware, die sich auf kontenbasiertes Marketing konzentriert, einschließlich E-Mail, Mobil-, Social-, Digital-Anzeigen, Web-Management und Analysen.

Mithilfe des Formulardatenmodells von AEM Forms können wir AEM Formular jetzt nahtlos in Marketing integrieren.

[Weitere Informationen zum Formulardatenmodell](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo stellt eine REST-API zur Verfügung, die die entfernte Ausführung vieler Systemfunktionen ermöglicht. Von der Erstellung von Programmen bis zum Massenimport von Interessenten gibt es viele Optionen, die eine feinkörnige Steuerung einer Marketing-Instanz ermöglichen. Mit dem Formulardatenmodell ist es ganz einfach, AEM Forms in Marketo zu integrieren.

Dieses Lernprogramm führt Sie durch die Schritte, die bei der Integration von AEM Forms mit Marketo mit dem Formulardatenmodell erforderlich sind. Nach Abschluss des Lernprogramms haben Sie ein OSGi-Bundle, das die benutzerdefinierte Authentifizierung für Marketo durchführt. Außerdem haben Sie die Datenquelle mithilfe der bereitgestellten Swagger-Datei konfiguriert.

Es wird dringend empfohlen, sich mit den folgenden Themen im Abschnitt Voraussetzung vertraut zu machen.

## Voraussetzung

1. [AEM Server mit AEM Forms Hinzufügen auf installiertem Paket](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. Umgebung zur lokalen AEM
1. Mit Formulardatenmodell vertraut
1. Grundkenntnisse in Swagger-Dateien
1. Adaptives Forms erstellen

**geheime Client-ID und geheimer Clientschlüssel**

Der erste Schritt bei der Integration von Marketo mit AEM Forms besteht darin, die API-Anmeldeinformationen abzurufen, die für die REST-Aufrufe mit der API erforderlich sind. Sie benötigen Folgendes:

1. client_id
1. client_secret
1. identity_endpoint
1. Authentifizierungs-URL.

[Bitte befolgen Sie die offizielle Marketingdokumentation, um die oben genannten Eigenschaften zu erhalten.](https://developers.marketo.com/rest-api/) Alternativ können Sie sich auch an den Administrator Ihrer Marketo-Instanz wenden.

**Voraussetzungen**

[Laden Sie die Elemente für diesen Artikel herunter und dekomprimieren Sie sie.](assets/aemformsandmarketo.zip) Die ZIP-Datei enthält Folgendes:

1. BlankTemplatePackage.zip - Dies ist die Vorlage für ein adaptives Formular. Importieren Sie dies mit dem Package Manager.
1. marketo.json - Dies ist die Swagger-Datei, die zum Konfigurieren der Datenquelle verwendet wird.
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar - Dies ist das Bundle, das die benutzerdefinierte Authentifizierung ausführt. Verwenden Sie diese Option, wenn Sie das Tutorial nicht abschließen können oder Ihr Bundle nicht wie erwartet funktioniert.

---
title: AEM Forms mit Marketo (Teil 3)
seo-title: AEM Forms mit Marketo (Teil 3)
description: Lernprogramm zur Integration von AEM Forms in Marketing mit dem AEM Forms-Formulardatenmodell.
seo-description: Lernprogramm zur Integration von AEM Forms in Marketing mit dem AEM Forms-Formulardatenmodell.
feature: '"Adaptives Forms, Formulardatenmodell"'
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: Entwicklung
role: Entwickler
level: Erfahren
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 13%

---


# Datenquelle konfigurieren

Mit der AEM Forms-Datenintegration können Sie unterschiedliche Datenquellen konfigurieren und Verbindungen zu ihnen herzustellen. Die folgenden Datenquellen werden standardmäßig unterstützt. Mit einer kleinen Anpassung können Sie jedoch auch andere Datenquellen integrieren.

1. Relationale Datenbanken: MySQL, Microsoft SQL Server, IBM DB2 und Oracle RDBMS
1. AEM-Benutzerprofil 
1. RESTful Webservices 
1. SOAP-basierte Webservices
1. OData-Dienstleistungen

Für die Integration von AEM Forms mit Marketo werden wir RESTful Web Services verwenden. Der erste Schritt bei der Integration ist die Konfiguration einer [Datenquelle.](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) Bitte verwenden Sie die Swagger-Datei, die im Rahmen dieses Lernprogramms zur Verfügung gestellt wird. Der folgende Screenshot zeigt die wichtigen Eigenschaften, die beim Konfigurieren der Datenquelle angegeben werden müssen.
![datasource](assets/datasource.jfif)

Die Datei &quot;marketo.json&quot;ist die Swagger-Datei und wird Ihnen als Teil der Assets dieses Tutorials zur Verfügung gestellt.
Eigenschafts-Host ist spezifisch für Ihre Marketing-Instanz.
Der Authentifizierungstyp ist benutzerdefiniert und die Authentifizierungsimplementierung muss mit &quot;AEMForms with Marketing&quot;übereinstimmen. (Sofern Sie dies nicht in Ihrem Code geändert haben).

## Formulardatenmodell erstellen

Nach der Konfiguration der Datenquelle wird im nächsten Schritt ein Formulardatenmodell erstellt, das auf der im vorherigen Schritt konfigurierten Datenquelle basiert. Gehen Sie wie folgt vor, um ein Formulardatenmodell zu erstellen:

Verweisen Sie Ihren Browser auf die Seite [Datenintegrationen.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) Dadurch werden alle auf Ihrer AEM erstellten Datenintegrationen Liste.

1. Klicken Sie auf Erstellen | Formulardatenmodell
1. Geben Sie einen aussagekräftigen Titel wie FormsAndMarketing ein und klicken Sie auf Weiter
1. Wählen Sie die Datenquelle aus, die im vorherigen Schritt konfiguriert wurde, und klicken Sie auf Erstellen und bearbeiten, um das Formulardatenmodell im Bearbeitungsmodus zu öffnen
1. Erweitern Sie den Knoten &quot;FormsAndMarketo&quot;. Erweitern des Knotens Dienste
1. Wählen Sie die erste &quot;Get&quot;-Operation
1. Klicken Sie auf Hinzufügen Auswahl
1. Klicken Sie im Dialogfeld &quot;Hinzufügen verknüpfte Modellobjekte&quot;auf &quot;Alle auswählen&quot;und klicken Sie dann auf Hinzufügen
1. Speichern Sie das Formulardatenmodell, indem Sie auf &quot;Speichern&quot;klicken
1. Registerkarte zur Registerkarte &quot;Dienste&quot;
1. Wählen Sie den einzigen aufgelisteten Dienst aus und klicken Sie auf Testdienst
1. Geben Sie eine gültige leadId ein und klicken Sie auf Test. Wenn alles gut läuft, sollten Sie die Interessentendetails wie im Screenshot unten gezeigt zurückerhalten
   ![testresults](assets/testresults.jfif)

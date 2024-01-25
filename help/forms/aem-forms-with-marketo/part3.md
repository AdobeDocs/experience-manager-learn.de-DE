---
title: AEM Forms in Marketo (Teil 3)
description: Tutorial zur Integration von AEM Forms in Marketo mithilfe des AEM Forms-Formulardatenmodells.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 7096340b-8ccf-4f5e-b264-9157232e96ba
duration: 86
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 100%

---

# Konfigurieren einer Datenquelle

Mit der AEM Forms-Datenintegration können Sie unterschiedliche Datenquellen konfigurieren und Verbindungen zu ihnen herzustellen. Die folgenden Datenquellen werden standardmäßig unterstützt. Es ist jedoch möglich, mit nur wenigen Anpassungen auch andere Datenquellen zu integrieren.

1. Relationale Datenbanken: MySQL, Microsoft SQL Server, IBM DB2 und Oracle RDBMS
1. AEM-Benutzerprofil 
1. RESTful-Webservices
1. SOAP-basierte Webservices
1. OData-Services  

Für die Integration von AEM Forms in Marketo verwenden wir RESTful-Web-Dienste. Der erste Schritt bei der Integration besteht in der Konfiguration einer [Datenquelle.](https://helpx.adobe.com/de/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) Verwenden Sie die im Rahmen dieses Tutorials bereitgestellte Swagger-Datei. Der folgende Screenshot zeigt wichtige Eigenschaften, die beim Konfigurieren der Datenquelle angegeben werden müssen.
![Datenquelle](assets/datasource.jfif)

„marketo.json“ ist die Swagger-Datei und wird Ihnen mit den Assets für dieses Tutorial bereitgestellt.
Der Eigenschaften-Host ist spezifisch für Ihre Marketo-Instanz.
Der Authentifizierungstyp ist benutzerdefiniert und die Authentifizierungsimplementierung muss „AemForms With Marketo“ entsprechen. (Es sei denn, Sie haben dies in Ihrem Code geändert.)

## Erstellen eines Formulardatenmodells

Nach dem Konfigurieren der Datenquelle besteht der nächste Schritt darin, ein Formulardatenmodell zu erstellen, das auf der im vorherigen Schritt konfigurierten Datenquelle basiert. Gehen Sie wie folgt vor, um ein Formulardatenmodell zu erstellen:

Verweisen Sie den Browser auf die [Datenintegrationsseite.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) Hier werden alle in Ihrer AEM-Instanz erstellten Datenintegrationen aufgelistet.

1. Klicken Sie auf „Erstellen“ > „Formulardatenmodell erstellen“.
1. Geben Sie einen aussagekräftigen Titel wie „FormulareUndMarketo“ ein und klicken Sie auf „Weiter“.
1. Wählen Sie die im vorherigen Schritt konfigurierte Datenquelle aus und klicken Sie auf „Erstellen und Bearbeiten“, um das Formulardatenmodell im Bearbeitungsmodus zu öffnen.
1. Erweitern Sie den Knoten „FormulareUndMarketo“. Erweitern Sie den Knoten „Dienste“.
1. Wählen Sie den ersten GET-Vorgang aus
1. Klicken Sie auf „Ausgewählte hinzufügen“.
1. Klicken Sie im Dialogfeld „Zugeordnete Modellobjekte hinzufügen“ auf „Alle auswählen“ und dann auf „Hinzufügen“.
1. Speichern Sie das Formulardatenmodell, indem Sie auf die Schaltfläche „Speichern“ klicken.
1. Wechseln Sie zur Registerkarte „Dienste“.
1. Wählen Sie den einzigen aufgelisteten Dienst aus und klicken Sie auf „Dienst testen“.
1. Geben Sie eine gültige Lead-ID ein und klicken Sie auf „Testen“. Wenn alles reibungslos funktioniert, sollten die Lead-Details zurückgegeben werden, wie im Screenshot unten dargestellt
   ![Testergebnisse](assets/testresults.jfif)

## Nächste Schritte

[Anwenden für Tests](./part4.md)

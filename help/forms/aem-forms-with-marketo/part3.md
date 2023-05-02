---
title: AEM Forms mit Marketo (Teil 3)
description: Tutorial zur Integration von AEM Forms mit Marketo mithilfe des AEM Forms-Formulardatenmodells.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 7096340b-8ccf-4f5e-b264-9157232e96ba
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Konfigurieren einer Datenquelle

Mit der AEM Forms-Datenintegration können Sie unterschiedliche Datenquellen konfigurieren und Verbindungen zu ihnen herzustellen. Die folgenden Datenquellen werden standardmäßig unterstützt. Mit einer kleinen Anpassung können Sie jedoch auch mit anderen Datenquellen integrieren.

1. Relationale Datenbanken: MySQL, Microsoft SQL Server, IBM DB2 und Oracle RDBMS
1. AEM-Benutzerprofil 
1. RESTful-Webservices
1. SOAP-basierte Webservices
1. OData-Services  

Für die Integration von AEM Forms mit Marketo verwenden wir RESTful-Webdienste. Der erste Schritt bei der Integration besteht darin, eine [Datenquelle.](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) Verwenden Sie die Swagger-Datei, die im Rahmen dieses Tutorials bereitgestellt wird. Der folgende Screenshot zeigt die wichtigen Eigenschaften, die beim Konfigurieren der Datenquelle angegeben werden müssen.
![datasource](assets/datasource.jfif)

&quot;marketo.json&quot;ist die Swagger-Datei und wird Ihnen als Teil der Assets dieses Tutorials bereitgestellt.
Der Eigenschaftenhost ist spezifisch für Ihre Marketo-Instanz.
Der Authentifizierungstyp ist benutzerdefiniert und die Authentifizierungsimplementierung muss mit &quot;AEM Forms mit Marketo&quot;übereinstimmen. (Sofern Sie dies nicht in Ihrem Code geändert haben).

## Erstellen von Formulardatenmodellen

Nach dem Konfigurieren der Datenquelle besteht der nächste Schritt darin, ein Formulardatenmodell zu erstellen, das auf der zuvor konfigurierten Datenquelle basiert. Gehen Sie wie folgt vor, um ein Formulardatenmodell zu erstellen:

Verweisen Sie den Browser auf [Datenintegrationsseite.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) Hier werden alle Datenintegrationen aufgelistet, die in Ihrer AEM-Instanz erstellt wurden.

1. Klicken Sie auf Erstellen | Formulardatenmodell
1. Geben Sie einen aussagekräftigen Titel wie FormsAndMarketo ein und klicken Sie auf Weiter
1. Wählen Sie die Datenquelle aus, die im vorherigen Schritt konfiguriert wurde, und klicken Sie auf Erstellen und Bearbeiten , um das Formulardatenmodell im Bearbeitungsmodus zu öffnen.
1. Erweitern Sie den Knoten &quot;FormsAndMarketo&quot;. Erweitern Sie den Knoten Dienste .
1. Wählen Sie den ersten Vorgang &quot;Get&quot;aus
1. Klicken Sie auf Ausgewählte hinzufügen
1. Klicken Sie im Dialogfeld &quot;Zugeordnete Modellobjekte hinzufügen&quot;auf &quot;Alle auswählen&quot;und klicken Sie dann auf &quot;Hinzufügen&quot;
1. Speichern Sie das Formulardatenmodell, indem Sie auf die Schaltfläche Speichern klicken
1. Registerkarte &quot;Dienste&quot;
1. Wählen Sie den einzigen aufgelisteten Dienst aus und klicken Sie auf Test Service .
1. Geben Sie eine gültige leadId ein und klicken Sie auf Test . Wenn alles gut läuft, sollten Sie die Lead-Details zurückerhalten, wie im Screenshot unten dargestellt
   ![Testergebnisse](assets/testresults.jfif)

## Nächste Schritte

[Alles zusammenbringen für Tests](./part4.md)


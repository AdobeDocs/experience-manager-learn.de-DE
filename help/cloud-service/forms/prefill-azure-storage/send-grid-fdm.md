---
title: Senden von E-Mails mit SendGrid
description: Auslösen einer E-Mail mit einem Link zum gespeicherten Formular
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-13717
exl-id: 4b2d1e50-9fa1-4934-820b-7dae984cee00
duration: 37
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '183'
ht-degree: 100%

---

# Integrieren in SendGrid

Die AEM Forms-Funktion zur Datenintegration ermöglicht die Konfiguration und Verbindung verschiedener Datenquellen mit AEM Forms. Es bietet eine intuitive Benutzeroberfläche, um ein einheitliches Datendarstellungsschema für Geschäftsentitäten und Dienste aus allen verbundenen Datenquellen zu erstellen.

Wir haben hier die SendGrid-API zum Senden von E-Mails mithilfe einer dynamischen Vorlage verwendet. Es wird davon ausgegangen, dass Sie mit der SendGrid-API zum Senden von E-Mails mit dynamischen Vorlagen vertraut sind. Im Rahmen dieses Tutorials wurde Ihnen eine Swagger-Datei zur API-Beschreibung bereitgestellt.

## Erstellen der Integration

Gehen Sie wie folgt vor, um die Integration zwischen AEM Forms und SendGrid zu erstellen.

* Erstellen Sie eine RESTful-Datenquelle mithilfe der [Swagger-Datei](./assets/SendGridWithDynamicTemplate.yaml). [Befolgen Sie die in diesem Video enthaltenen ausführlichen Anweisungen](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=de) zum Erstellen von Datenquellen in AEM Forms.
* Erstellen Sie ein Formulardatenmodell basierend auf der Datenquelle, die Sie im vorherigen Schritt erstellt haben.[Befolgen Sie die ausführliche Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate/use-form-data-model/create-form-data-models.html?lang=de) zum Erstellen des Formulardatenmodells.

Das für dieses Tutorial erstellte Formulardatenmodell ist Teil der Artikel-Assets.

### Nächste Schritte

[Erstellen der Azure Storage-Konfiguration](./create-fdm.md)

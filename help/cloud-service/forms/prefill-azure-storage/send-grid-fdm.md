---
title: E-Mail mit SendGrid senden
description: Trigger einer E-Mail mit einem Link zum gespeicherten Formular
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 13717
exl-id: 4b2d1e50-9fa1-4934-820b-7dae984cee00
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 12%

---

# Integration mit SendGrid

Die Datenintegration von AEM Forms ermöglicht die Konfiguration und Verbindung verschiedener Datenquellen mit AEM Forms. In der intuitiven Benutzeroberfläche können Sie eine einheitliche Datendarstellung der Geschäftsbereiche und Services für sämtliche verbundenen Datenquellen erstellen.

Wir haben die SendGrid-API zum Senden von E-Mails mithilfe einer dynamischen Vorlage verwendet. Es wird davon ausgegangen, dass Sie mit der SendGrid-API für das Senden von E-Mails mit dynamischen Vorlagen vertraut sind. Im Rahmen dieses Tutorials wurde Ihnen eine Swagger-Datei zur Beschreibung der API bereitgestellt.

## Integration erstellen

Führen Sie die folgenden Schritte aus, um die Integration zwischen AEM Forms und SendGrid zu erstellen

* Erstellen Sie eine RESTful-Datenquelle mithilfe der [Swagger-Datei](./assets/SendGridWithDynamicTemplate.yaml). [In diesem Video erhalten Sie ausführliche Anweisungen.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) zum Erstellen von Datenquellen in AEM Forms
* Erstellen Sie ein Formulardatenmodell basierend auf der Datenquelle, die Sie im vorherigen Schritt erstellt haben.[Folgen Sie der detaillierten Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate/use-form-data-model/create-form-data-models.html) zum Erstellen des Formulardatenmodells.

Das für dieses Tutorial erstellte Formulardatenmodell ist Teil der Artikel-Assets.

### Nächste Schritte

[Azure Storage-Integration erstellen](./create-fdm.md)

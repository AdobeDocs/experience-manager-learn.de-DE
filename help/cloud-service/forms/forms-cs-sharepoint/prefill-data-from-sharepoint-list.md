---
title: Vorbefüllen eines adaptiven Formulars mit Daten aus einer SharePoint-Liste
description: Erfahren Sie, wie Sie das adaptive Formular mithilfe eines Formulardatenmodells, das von der SharePoint-Liste unterstützt wird, vorab ausfüllen
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14795
duration: 60
exl-id: 9abe9f9d-8fb3-4e01-a830-1dad1c27274d
source-git-commit: 3dc1aea74e2a7cf30da9f6fb96ecc5c7edcf6e34
workflow-type: ht
source-wordcount: '234'
ht-degree: 100%

---

# Vorausfüllen eines adaptiven Formulars mit Daten aus der SharePoint-Liste

In der vorherigen Version von AEM Form (6.5) musste benutzerdefinierter Code geschrieben werden, um ein durch Formulardatenmodell unterstütztes adaptives Formular mithilfe des Anfrage-Attributs im Voraus auszufüllen. In AEM Forms as Cloud Service ist das Schreiben von benutzerdefiniertem Code nicht mehr erforderlich.

In diesem Artikel werden die Schritte erläutert, die zum Vorbefüllen/Vorausfüllen eines adaptiven Formulars mit Daten erforderlich sind, die aus der SharePoint-Liste mit dem Vorbefüllungsdienst für Formulardatenmodelle abgerufen wurden.

In diesem Artikel wird davon ausgegangen, dass Sie [das adaptive Formular erfolgreich konfiguriert haben, um Daten an die SharePoint-Liste zu senden](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=de#connect-af-sharepoint-list).

Im Folgenden sehen Sie Daten der Sharepoint-Liste:
![SharePoint-Liste](assets/list-data.png)

Um ein adaptives Formular mit den Daten auszufüllen, die mit einem bestimmten GUID verknüpft sind, müssen die folgenden Schritte ausgeführt werden:

## Konfigurieren des Get-Service

* Erstellen Sie einen Get-Service für das Objekt der obersten Ebene des Formulardatenmodells mithilfe des GUID-Attributs
  ![get-service](assets/mapping-request-attribute.png)

In diesem Screenshot ist die Spalte „GUID“ über ein Anfrage-Attribut mit dem Namen `submissionid` gebunden.

Der vollständig konfigurierte Get-Service sieht wie folgt aus:

![get-service](assets/fdm-request-attribute.png)

## Konfigurieren des adaptiven Formulars, um den Vorbefüllungsdienst des Formulardatenmodells zu verwenden

* Öffnen Sie ein adaptives Formular, das auf dem Formulardatenmodell der SharePoint-Liste basiert. Verknüpfen des Vorfüllservice für ein Formulardatenmodell
  ![form-prefill-service](assets/form-prefill-service.png)

## Testen des Formulars

Vorschau des Formulars durch Einbeziehung der `submissionid` in der URL wie unten dargestellt

```html
http://localhost:4502/content/dam/formsanddocuments/contactusform/jcr:content?wcmmode=disabled&submissionid=57e12249-751a-4a38-a81f-0a4422b24412
```

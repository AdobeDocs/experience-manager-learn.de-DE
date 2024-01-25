---
title: Vorausfüllen des adaptiven Formulars mit Daten aus der Sharepoint-Liste
description: Erfahren Sie, wie Sie das adaptive Formular mithilfe des Formulardatenmodells, das von der Freigabepunktliste unterstützt wird, vorab ausfüllen
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14795
duration: 53
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 2%

---

# Vorausfüllen des adaptiven Formulars mit SharePoint-Listendaten

In der vorherigen Version von AEM Form (6.5) musste benutzerdefinierter Code geschrieben werden, um ein durch Formulardatenmodell unterstütztes adaptives Formular mithilfe des Anforderungsattributs im Voraus auszufüllen. In AEM Forms as Cloud Service ist das Schreiben von benutzerdefiniertem Code nicht mehr erforderlich.

In diesem Artikel werden die Schritte erläutert, die zum Vorausfüllen/Vorausfüllen des adaptiven Formulars mit Daten erforderlich sind, die aus der Sharepoint-Liste mit dem Vorbefüllungs-Dienst für Formulardatenmodelle abgerufen wurden.

In diesem Artikel wird davon ausgegangen, dass [das adaptive Formular erfolgreich konfiguriert hat, um Daten an die Sharepoint-Liste zu senden.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=en#connect-af-sharepoint-list)

Im Folgenden finden Sie die Daten in der Sharepoint-Liste
![sharepoint-list](assets/list-data.png)

Um ein adaptives Formular mit den Daten auszufüllen, die mit einem bestimmten Handbuch verknüpft sind, müssen die folgenden Schritte ausgeführt werden

## Konfigurieren des get-Dienstes

* Erstellen Sie einen get -Dienst für das Objekt der obersten Ebene des Formulardatenmodells mithilfe des guid -Attributs.
  ![get-service](assets/mapping-request-attribute.png)

In diesem Screenshot ist die Spalte &quot;guid&quot;an ein Anforderungsattribut mit dem Namen `submissionid`.

Der vollständig konfigurierte get-Dienst sieht wie folgt aus

![get-service](assets/fdm-request-attribute.png)

## Konfigurieren des adaptiven Formulars für die Verwendung des Vorfülldienstes für Formulardatenmodelle

* Öffnen Sie ein adaptives Formular, das auf dem Datenmodell des Freigabepunkt-Listenformulars basiert. Verknüpfen des Vorbefüllungs-Dienstes für das Formulardatenmodell
  ![form-prefill-service](assets/form-prefill-service.png)

## Testen des Formulars

Vorschau des Formulars durch Einbeziehung der `submissionid` in der URL wie unten dargestellt

```html
http://localhost:4502/content/dam/formsanddocuments/contactusform/jcr:content?wcmmode=disabled&submissionid=57e12249-751a-4a38-a81f-0a4422b24412
```





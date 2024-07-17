---
title: AEM Forms mit Marketo (Teil 2)
description: Tutorial zur Integration von AEM Forms in Marketo mithilfe des AEM Forms-Formulardatenmodells.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: f8ba3d5c-0b9f-4eb7-8609-3e540341d5c2
duration: 137
source-git-commit: 8bde459ae9a6e261cfc3aff308babe9de6e56059
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 15%

---

# Erstellen einer Datenquelle

Marketos REST-APIs sind mit 2-Legierungen OAuth 2.0 authentifiziert. Wir können einfach eine Datenquelle mithilfe der Swagger-Datei erstellen, die im vorherigen Schritt heruntergeladen wurde.

## Konfigurationscontainer erstellen

* Melden Sie sich bei AEM an.
* Klicken Sie auf das Menü &quot;Tools&quot;und dann auf **Konfigurations-Browser** , wie unten dargestellt

* ![Menü &quot;Tools&quot;](assets/datasource3.png)

* Klicken Sie auf **Erstellen** und geben Sie einen aussagekräftigen Namen ein, wie unten dargestellt. Stellen Sie sicher, dass Sie die Option Cloud-Konfigurationen wie unten gezeigt auswählen

* ![Konfigurations-Container](assets/datasource4.png)

## Erstellen von Cloud-Services

* Navigieren Sie zum Menü &quot;Tools&quot;und klicken Sie dann auf Cloud Services > Data Sources .

* ![cloud-services](assets/datasource5.png)

* Wählen Sie den Konfigurationscontainer aus, der im vorherigen Schritt erstellt wurde, und klicken Sie auf **Erstellen** , um eine neue Datenquelle zu erstellen. Geben Sie einen aussagekräftigen Namen ein, wählen Sie den RESTful-Dienst aus der Dropdown-Liste Diensttyp aus und klicken Sie auf **Weiter**
* ![new-data-source](assets/datasource6.png)

* Laden Sie die Swagger-Datei hoch und geben Sie den Grant-Typ, die Client-ID, das Client-Geheimnis und die Zugriffstoken-URL für Ihre Marketo-Instanz an, wie im Screenshot unten dargestellt.

* Testen Sie die Verbindung. Wenn die Verbindung erfolgreich hergestellt wurde, klicken Sie auf die blaue Schaltfläche **Erstellen** , um die Erstellung der Datenquelle abzuschließen.

* ![data-source-config](assets/datasource1.png)


## Nächste Schritte

[Erstellen einer RESTful-Dienst-basierten Datenquelle](./part3.md)

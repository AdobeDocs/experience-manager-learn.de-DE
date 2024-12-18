---
title: Integrieren von AEM Forms as a Cloud Service und Marketo (Teil 2)
description: Erfahren Sie, wie Sie AEM Forms und Marketo mithilfe des AEM Forms-Formulardatenmodells integrieren.
feature: Form Data Model,Integration
version: Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
exl-id: 75e589fa-f7fc-4d0b-98c8-ce4d603ef2f7
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 100%

---

# Erstellen einer Datenquelle

Die REST-APIs von Marketo sind mit OAuth 2.0 mit zwei Abzweigungen authentifiziert. Wir können ganz einfach eine Datenquelle mithilfe der Swagger-Datei erstellen, die im vorherigen Schritt heruntergeladen wurde.

## Erstellen eines Konfigurations-Containers

* Melden Sie sich bei AEM an.
* Klicken Sie auf das Menü „Tools“ und dann auf **Konfigurations-Browser**, wie unten dargestellt

* ![Menü „Tools“](assets/datasource3.png)

* Klicken Sie auf **Erstellen** und geben Sie einen aussagekräftigen Namen ein, wie unten dargestellt. Stellen Sie sicher, dass Sie die Option „Cloud-Konfigurationen“ wie unten dargestellt auswählen

* ![Konfigurations-Container](assets/datasource4.png)

## Erstellen von Cloud-Services

* Navigieren Sie zum Menü „Tools“ und klicken Sie dann auf „Cloud-Services“ > „Datenquellen“

* ![cloud-services](assets/datasource5.png)

* Wählen Sie den Konfigurations-Container aus, der im vorherigen Schritt erstellt wurde, und klicken Sie auf **Erstellen**, um eine neue Datenquelle zu erstellen. Geben Sie einen aussagekräftigen Namen ein, wählen Sie aus der Dropdown-Liste „Diensttyp“ den RESTful-Dienst aus und klicken Sie auf **Weiter**
* ![new-data-source](assets/datasource6.png)

* Laden Sie die Swagger-Datei hoch und geben Sie den Grant-Typ, die Client-ID, das Client-Geheimnis und die Zugriffs-Token-URL für Ihre Marketo-Instanz an, wie im Screenshot unten dargestellt.

* Testen Sie die Verbindung. Wenn sich die Verbindung erfolgreich herstellen lässt, klicken Sie auf die blaue Schaltfläche **Erstellen**, um die Erstellung der Datenquelle abzuschließen.

* ![data-source-config](assets/datasource1.png)


## Nächste Schritte

[Erstellen eines Formulardatenmodells](./part3.md)

---
title: Integrieren mit [!DNL ServiceNow]
description: Erstellen und zeigen Sie alle Vorfälle mithilfe des Formulardatenmodells an.
feature: Adaptive Forms
version: 6.4,6.5
kt: 9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
source-git-commit: 81a15fb0182760aaac8cb58cccbfe28de7323492
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 3%

---

# Integrieren von AEM Forms mit [!DNL ServiceNow]

Vorfälle erstellen und anzeigen in [!DNL ServiceNow] Verwenden des Formulardatenmodells in AEM Forms.

## Voraussetzungen

* [!DNL ServiceNow] -Konto.
* Mit [Erstellen von Datenquellen](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* Mit [Formulardatenmodell](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)

## Beispiel-Assets

Die in diesem Artikel bereitgestellten Beispiel-Assets umfassen Folgendes
* Cloud Service-Konfiguration
* Swagger-Dateien zum Erstellen eines Vorfalls und Abrufen aller Vorfälle
* Formulardatenmodell basierend auf den Swagger-Dateien
* Adaptives Formular zum Erstellen und Auflisten [!DNL ServiceNow] Vorfälle

## Bereitstellen der Assets auf Ihrem Server

* Laden Sie die [Beispiel-Assets](assets/service-now.zip)
* Importieren Sie die Assets in AEM mit [Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* Bearbeiten Sie die [Konfiguration des CreateIncident-Cloud-Service](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident), um mit Ihrer ServiceNow-Instanz abzugleichen.
* Bearbeiten Sie die [Cloud Service-Konfiguration von GetAllIncidents](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) , um Ihrer ServiceNow-Instanz zu entsprechen

## Testen der Integration

* [Öffnen Sie das adaptive Formular](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* Geben Sie Werte in das Beschreibungsfeld und in das Kommentarfeld ein und klicken Sie auf die Schaltfläche Vorfall erstellen .
* Die Incident-ID des neu erstellten Vorfalls sollte in das Textfeld eingetragen werden und die nachstehende Tabelle sollte alle Vorfälle auflisten.

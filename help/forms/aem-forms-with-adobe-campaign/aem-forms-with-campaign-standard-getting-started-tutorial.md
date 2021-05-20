---
title: Erste Schritte mit AEM Forms und Adobe Campaign Standard
seo-title: Erste Schritte mit AEM Forms und Adobe Campaign Standard
description: Integrieren Sie AEM Forms mit Adobe Campaign Standard mithilfe des AEM Forms-Formulardatenmodells, um ACS-Kampagnenprofilinformationen usw. abzurufen.
seo-description: Integrieren Sie AEM Forms mit Adobe Campaign Standard mithilfe des AEM Forms-Formulardatenmodells, um ACS-Kampagnenprofilinformationen usw. abzurufen.
uuid: 56450c9b-3752-4a64-b1b3-8c78e81f5921
feature: Adaptives Forms, Formulardatenmodell
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 89245554-7b99-4e7e-9810-52191f9ea365
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 3%

---


# Erste Schritte mit AEM Forms und Adobe Campaign Standard {#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formsandcampaign](assets/helpx-cards-forms.png)

In diesem Tutorial werden die verschiedenen Anwendungsfälle für die Integration von AEM Forms in Adobe Campaign Standard (ACS) aufgelistet.

ACS verfügt über einen umfangreichen Satz von APIs, die es ACS ermöglichen, mit der Technologie unserer Wahl zu interagieren. In diesem Tutorial konzentrieren wir uns auf die Verknüpfung von AEM Forms mit ACS.

Gehen Sie wie folgt vor, um AEM Forms in ACS zu integrieren:

* [Richten Sie den API-Zugriff auf Ihre ACS-Instanz ein.](https://docs.campaign.adobe.com/doc/standard/en/api/ACS_API.html#setting-up-api-access)
* JSON-Web-Token erstellen.
* Ersetzen Sie das JSON-Web-Token durch den Adobe Identity Management-Dienst durch ein Zugriffstoken.
* Schließen Sie dieses Zugriffstoken in den Autorisierungs-HTTP-Header zusammen mit dem X-API-Schlüssel in jeder Anfrage an die ACS-Instanz ein.

Befolgen Sie zum Einstieg die folgenden Anweisungen:

* [Laden Sie die mit diesem Tutorial verknüpften Assets herunter und entpacken Sie sie.](assets/aem-forms-and-acs-bundles.zip)
* Stellen Sie die Bundles mithilfe von [Felix Web Console](http://localhost:4502/system/console/bundles) bereit.
* Stellen Sie die entsprechenden Einstellungen für Adobe Campaign in der Felix OSGi-Konfiguration bereit.
* [Erstellen Sie einen Dienstbenutzer wie in diesem Artikel](/help/forms/adaptive-forms/service-user-tutorial-develop.md) beschrieben. Stellen Sie sicher, dass Sie das mit dem Artikel verknüpfte OSGi-Bundle bereitstellen.
* Speichern Sie den privaten ACS-Schlüssel in etc/key/campaign/private.key. Sie müssen einen Ordner mit dem Namen campaign unter etc/key erstellen.
* [Stellen Sie dem Dienstbenutzer &quot;data&quot; Lesezugriff auf den Kampagnenordner bereit.](http://localhost:4502/useradmin)

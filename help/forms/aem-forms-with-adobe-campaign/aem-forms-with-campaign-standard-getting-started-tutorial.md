---
title: Erste Schritte mit AEM Forms und Adobe Campaign Standard
seo-title: Erste Schritte mit AEM Forms und Adobe Campaign Standard
description: Integrieren Sie AEM Forms mit Adobe Campaign Standard mithilfe des AEM Forms-Formulardatenmodells, um Informationen zum ACS-Kampagne-Profil usw. abzurufen.
seo-description: Integrieren Sie AEM Forms mit Adobe Campaign Standard mithilfe des AEM Forms-Formulardatenmodells, um Informationen zum ACS-Kampagne-Profil usw. abzurufen.
uuid: 56450c9b-3752-4a64-b1b3-8c78e81f5921
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 89245554-7b99-4e7e-9810-52191f9ea365
translation-type: tm+mt
source-git-commit: 3b44a9e2341ce23f737e6ef75c67fadd9870d2ac
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 2%

---


# Erste Schritte mit AEM Forms und Adobe Campaign Standard {#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formsandcampaign](assets/helpx-cards-forms.png)

In diesem Lernprogramm werden die verschiedenen Anwendungsfälle für die Integration von AEM Forms in Adobe Campaign Standard (ACS) Liste.

ACS verfügt über einen umfangreichen Satz von APIs offen gelegt, die es ermöglichen, ACS mit der Technologie unserer Wahl zu verbinden. In diesem Tutorial konzentrieren wir uns auf die Verknüpfung von AEM Forms mit ACS.

Um AEM Forms mit ACS zu integrieren, müssen Sie die folgenden Schritte ausführen:

* [Richten Sie den API-Zugriff auf Ihrer ACS-Instanz ein.](https://docs.campaign.adobe.com/doc/standard/en/api/ACS_API.html#setting-up-api-access)
* JSON-WebToken erstellen
* Tauschen Sie das JSON-WebToken mit der Adobe Identity Management Service für ein Zugriffstoken aus.
* Schließen Sie dieses Zugriffstoken in den Autorisierungs-HTTP-Header zusammen mit dem X-API-Schlüssel in jede Anforderung an die ACS-Instanz ein.

Zum Einstieg befolgen Sie bitte die folgenden Anweisungen

* [Laden Sie die Assets für dieses Lernprogramm herunter und dekomprimieren Sie sie.](assets/aem-forms-and-acs-bundles.zip)
* Bereitstellen der Pakete mithilfe der [Felix-Webkonsole](http://localhost:4502/system/console/bundles)
* Geben Sie die entsprechenden Einstellungen für Adobe Campaign in der Felix OSGI-Konfiguration ein.
* [Erstellen Sie einen Dienstbenutzer, wie in diesem Artikel](/help/forms/adaptive-forms/service-user-tutorial-develop.md)erwähnt. Stellen Sie sicher, dass Sie das mit dem Artikel verknüpfte OSGi-Bundle bereitstellen.
* Speichern Sie den privaten ACS-Schlüssel unter etc/key/campaign/private.key. Sie müssen einen Ordner mit dem Namen Kampagne unter etc/key erstellen.
* [Stellen Sie dem Dienstbenutzer &quot;data&quot;Lesezugriff auf den Ordner &quot;Kampagne&quot;bereit.](http://localhost:4502/useradmin)

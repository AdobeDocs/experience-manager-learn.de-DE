---
title: Erste Schritte mit AEM Forms und Adobe Campaign Standard
description: Integrieren Sie AEM Forms mit Adobe Campaign Standard mithilfe des AEM Forms-Formulardatenmodells, um ACS-Kampagnenprofilinformationen usw. abzurufen.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: e028837b-13d8-4058-ac25-ed095f49524c
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Erste Schritte mit AEM Forms und Adobe Campaign Standard {#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formsandcampaign](assets/helpx-cards-forms.png)

In diesem Tutorial werden die verschiedenen Anwendungsfälle für die Integration von AEM Forms in Adobe Campaign Standard (ACS) aufgelistet.

ACS verfügt über einen umfangreichen Satz von APIs, die es ACS ermöglichen, mit der Technologie unserer Wahl zu interagieren. In diesem Tutorial konzentrieren wir uns auf die Verknüpfung von AEM Forms mit ACS.

Gehen Sie wie folgt vor, um AEM Forms in ACS zu integrieren:

* [Richten Sie den API-Zugriff auf Ihre ACS-Instanz ein.](https://experienceleague.adobe.com/docs/campaign-standard/using/working-with-apis/get-started-apis.html?lang=en)
* JSON-Web-Token erstellen.
* Ersetzen Sie das JSON-Web-Token durch den Adobe Identity Management-Dienst durch ein Zugriffstoken.
* Schließen Sie dieses Zugriffstoken in den Autorisierungs-HTTP-Header zusammen mit dem X-API-Schlüssel in jeder Anfrage an die ACS-Instanz ein.

Befolgen Sie zum Einstieg die folgenden Anweisungen:

* [Laden Sie die mit diesem Tutorial verknüpften Assets herunter und entpacken Sie sie.](assets/aem-forms-and-acs-bundles.zip)
* Stellen Sie die Bundles mithilfe von bereit. [Felix-Webkonsole](http://localhost:4502/system/console/bundles)
* Stellen Sie die entsprechenden Einstellungen für Adobe Campaign in der Felix OSGi-Konfiguration bereit.
* [Erstellen Sie einen Dienstbenutzer wie in diesem Artikel beschrieben.](/help/forms/adaptive-forms/service-user-tutorial-develop.md). Stellen Sie sicher, dass Sie das mit dem Artikel verknüpfte OSGi-Bundle bereitstellen.
* Speichern Sie den privaten ACS-Schlüssel in etc/key/campaign/private.key. Sie müssen einen Ordner mit dem Namen campaign unter etc/key erstellen.
* [Stellen Sie dem Dienstbenutzer &quot;data&quot; Lesezugriff auf den Kampagnenordner bereit.](http://localhost:4502/useradmin)

## Nächste Schritte

[JWT und Zugriffstoken generieren](partone.md)

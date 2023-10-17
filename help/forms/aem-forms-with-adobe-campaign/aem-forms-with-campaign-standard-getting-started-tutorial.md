---
title: Integrieren von AEM Forms und Adobe Campaign Standard
description: Integrieren von AEM Forms mit Adobe Campaign Standard mithilfe des AEM Forms-Formulardatenmodells, um ACS-Kampagnenprofilinformationen usw. abzurufen
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
exl-id: e028837b-13d8-4058-ac25-ed095f49524c
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: ht
source-wordcount: '251'
ht-degree: 100%

---

# Integrieren von AEM Forms und Adobe Campaign Standard

![formsandcampaign](assets/helpx-cards-forms.png)

Hier lernen Sie, wie Sie AEM Forms mit Adobe Campaign Standard (ACS) integrieren.

ACS verfügt über einen umfangreichen Satz von APIs, die es ACS ermöglichen, mit der Technologie Ihrer Wahl zu interagieren. In diesem Tutorial konzentrieren wir uns auf die Verknüpfung von AEM Forms mit ACS.

Gehen Sie wie folgt vor, um AEM Forms in ACS integrieren:

* [Richten Sie den API-Zugriff auf Ihre ACS-Instanz ein.](https://experienceleague.adobe.com/docs/campaign-standard/using/working-with-apis/get-started-apis.html?lang=de)
* Erstellen Sie ein JSON-Web-Token.
* Ersetzen Sie das JSON-Web-Token mit dem Adobe Identity Management-Dienst durch ein Zugriffs-Token.
* Schließen Sie dieses Zugriffs-Token zusammen mit dem X-API-Schlüssel in jeder Anfrage an die ACS-Instanz in den Autorisierungs-HTTP-Header ein.

Befolgen Sie zum Einstieg die folgenden Anweisungen:

* [Laden Sie die mit diesem Tutorial verknüpften Assets herunter und entpacken Sie sie.](assets/aem-forms-and-acs-bundles.zip)
* Stellen Sie die Bundles mithilfe der [Felix-Web-Konsole](http://localhost:4502/system/console/bundles) bereit.
* Geben Sie die entsprechenden Einstellungen für Adobe Campaign in der Felix-OSGi-Konfiguration an.
* [Erstellen Sie eine Dienstbenutzerin oder einen Dienstbenutzer wie in diesem Artikel beschrieben](/help/forms/adaptive-forms/service-user-tutorial-develop.md). Stellen Sie sicher, dass Sie das mit dem Artikel verknüpfte OSGi-Bundle bereitstellen.
* Speichern Sie den privaten ACS-Schlüssel in etc/key/campaign/private.key. Sie müssen einen Ordner mit dem Namen „campaign“ unter etc/key erstellen.
* [Stellen Sie der Dienstbenutzerin bzw. dem Dienstbenutzer namens „data“ Lesezugriff auf den Kampagnenordner bereit.](http://localhost:4502/useradmin)

## Nächste Schritte

[Generieren von JWT und Zugriffstoken](partone.md)

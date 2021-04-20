---
title: DataSource mit Salesforce in AEM Forms 6.3 und 6.4 konfigurieren
seo-title: DataSource mit Salesforce in AEM Forms 6.3 und 6.4 konfigurieren
description: Integration von AEM Forms mit Salesforce mithilfe des Formulardatenmodells
seo-description: Integration von AEM Forms mit Salesforce mithilfe des Formulardatenmodells
uuid: 0124526d-f1a3-4f57-b090-a418a595632e
feature: Adaptive Forms, Form Data Model
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 8e314fc3-62d0-4c42-b1ff-49ee34255e83
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '936'
ht-degree: 0%

---


# DataSource mit Salesforce in AEM Forms 6.3 und 6.4 konfigurieren{#configuring-datasource-with-salesforce-in-aem-forms-and}

## Voraussetzungen {#prerequisites}

In diesem Artikel werden wir den Prozess der Erstellung der Datenquelle mit Salesforce durchlaufen

Voraussetzungen für diese Übung:

* Blättern Sie nach unten auf dieser Seite, laden Sie die Swagger-Datei herunter und speichern Sie sie auf Ihrer Festplatte.
* AEM Forms mit aktiviertem SSL

   * [Offizielle Dokumentation zur Aktivierung von SSL unter AEM 6.3](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [Offizielle Dokumentation zur Aktivierung von SSL unter AEM 6.4](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* Sie benötigen ein Salesforce-Konto
* Sie müssen eine verbundene App erstellen. Das offizielle Dokumentationsformular Salesforce zum Erstellen der App ist [hier](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0) aufgelistet.
* Bereitstellung geeigneter OAuth-Scopes für die App (ich habe alle verfügbaren OAuth-Scopes zum Testen ausgewählt)
* Geben Sie die Rückruf-URL ein. Die Rückruf-URL in meinem Fall war

   * Wenn Sie **AEM Forms 6.3** verwenden, lautet die Callback-URL https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html. In dieser URL-Datei ist createlead der Name meines Formulardatenmodells.

   * Wenn Sie** AEM Forms 6.4** verwenden, lautet die Rückruf-URL [https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html](https://gbedekar-w7-1:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html)

In diesem Beispiel ist gbedekar -w7-1:6443 der Name meines Servers und der Anschluss, an dem AEM ausgeführt wird.

Nachdem Sie die Notiz für die verbundene App erstellt haben, klicken Sie auf **Consumer key und auf den geheimen Schlüssel**. Sie benötigen diese beim Erstellen der Datenquelle in AEM Forms.

Nachdem Sie die verbundene App erstellt haben, müssen Sie eine Swagger-Datei für die Vorgänge erstellen, die Sie in salesforce ausführen müssen. Als Teil der herunterladbaren Assets wird eine Swagger-Musterdatei eingefügt. Mit dieser Swagger-Datei können Sie beim Senden des adaptiven Formulars das Objekt &quot;Interessent&quot;erstellen. Bitte entdecken Sie diese Swagger-Datei.

Der nächste Schritt besteht darin, Datenquelle in AEM Forms zu erstellen. Gehen Sie entsprechend Ihrer AEM Forms-Version wie folgt vor:

## AEM Forms 6.3 {#aem-forms}

* Melden Sie sich mit dem HTTPS-Protokoll bei AEM Forms an
* Navigieren Sie zu Cloud-Diensten, indem Sie https://&lt;Servername>: /etc/cloudservices.html eingeben, z. B. https://gbedekar-w7-1:6443/etc/cloudservices.html
* Blättern Sie nach unten zu &quot;Formulardatenmodell&quot;.
* Klicken Sie auf &quot;Konfigurationen anzeigen&quot;.
* Klicken Sie auf &quot;+&quot;, um eine neue Konfiguration hinzuzufügen
* Wählen Sie &quot;Restlicher Full Service&quot;. Geben Sie der Konfiguration aussagekräftigen Titel und Namen. Beispiel:

   * Name: CreateLeadInSalesForce
   * Titel: CreateLeadInSalesForce

* Klicken Sie auf &quot;Erstellen&quot;

**Im nächsten Bildschirm **

* Wählen Sie &quot;Datei&quot;als Option für Ihre Swagger-Quelldatei. Navigieren Sie zu der zuvor heruntergeladenen Datei
* Authentifizierungstyp als OAuth2.0 auswählen
* Geben Sie die Werte für ClientID und geheimer Clientschlüssel an
* OAuth-URL ist - **https://login.salesforce.com/services/oauth2/authorize**
* Token-URL aktualisieren - **https://na5.salesforce.com/services/oauth2/token**
* **URL des Zugriffstools - https://na5.salesforce.com/services/oauth2/token**
* Autorisierungsbereich: ** API   chatter_api full id   openid   refresh_token visualforce web**
* Authentifizierungs-Handler: Autorisierungsanbieter
* Klicken Sie auf &quot;Mit OAUTH verbinden&quot;. Wenn alles gut läuft, sollten Sie keine Fehler sehen

Nachdem Sie Ihr Formulardatenmodell mit Salesforce erstellt haben, können Sie die Formulardatenintegration mit der soeben erstellten Datenquelle erstellen. Die offizielle Dokumentation zum Erstellen der Formulardatenintegration ist [hier](https://helpx.adobe.com/aem-forms/6-3/data-integration.html).

Stellen Sie sicher, dass Sie das Formulardatenmodell so konfigurieren, dass es den POST-Dienst einbezieht, um ein Lead-Objekt in SFDC zu erstellen.

Sie müssen außerdem den Lese- und Schreibdienst für das Lead-Objekt konfigurieren. Die Screenshots finden Sie unten auf dieser Seite.

Nachdem Sie das Formulardatenmodell erstellt haben, können Sie adaptives Forms auf der Grundlage dieses Modells erstellen und die Übermittlungsmethoden des Formulardatenmodells verwenden, um Interessenten in SFDC zu erstellen.

## AEM Forms 6.4 {#aem-forms-1}

* Datenquelle erstellen

   * [Zu Datenquellen navigieren](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * Klicken Sie auf die Schaltfläche &quot;Erstellen&quot;
   * Stellen Sie einige aussagekräftige Werte bereit.

      * Name: CreateLeadInSalesForce
      * Titel: CreateLeadInSalesForce
      * Diensttyp: RESTful-Dienst
   * Klicken Sie auf Weiter
   * Swagger-Quelle: Datei
   * Suchen und wählen Sie die Swagger-Datei aus, die Sie im vorherigen Schritt heruntergeladen haben
   * Authentifizierungstyp: OAuth 2.0. Geben Sie die folgenden Werte an
   * Geben Sie die Werte für ClientID und geheimer Clientschlüssel an
   * OAuth-URL ist - **https://login.salesforce.com/services/oauth2/authorize**
   * Token-URL aktualisieren - **https://na5.salesforce.com/services/oauth2/token**
   * Zugriffstoken Ur **l - https://na5.salesforce.com/services/oauth2/token**
   * Autorisierungsbereich: ** api chatter_api full id openid refresh_token visualforce web**
   * Authentifizierungs-Handler: Autorisierungsanbieter
   * Klicken Sie auf &quot;Mit OAuth verbinden&quot;. Sollten Sie Fehler sehen, überprüfen Sie bitte die vorhergehenden Schritte, um sicherzustellen, dass alle Informationen korrekt eingegeben wurden.


Nachdem Sie Ihre Datenquelle mit SalesForce erstellt haben, können Sie dann die Formulardatenintegration mit der soeben erstellten Datenquelle erstellen. Der Link zur Dokumentation hierfür ist [hier](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)

Stellen Sie sicher, dass Sie das Formulardatenmodell so konfigurieren, dass es den POST-Dienst einbezieht, um ein Lead-Objekt in SFDC zu erstellen.

Sie müssen außerdem den Lese- und Schreibdienst für das Lead-Objekt konfigurieren. Die Screenshots finden Sie unten auf dieser Seite.

Nachdem Sie das Formulardatenmodell erstellt haben, können Sie adaptives Forms auf der Grundlage dieses Modells erstellen und die Übermittlungsmethoden des Formulardatenmodells verwenden, um Interessenten in SFDC zu erstellen.

>[!NOTE]
>
>Stellen Sie sicher, dass die URL in der Swagger-Datei Ihrer Region entspricht. Beispielsweise lautet die URL in der Swagger-Musterdatei &quot;na46.salesforce.com&quot;, da das Konto in Nordamerika erstellt wurde. Die einfachste Möglichkeit ist, sich bei Ihrem Salesforce-Konto anzumelden und die URL zu überprüfen.

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)

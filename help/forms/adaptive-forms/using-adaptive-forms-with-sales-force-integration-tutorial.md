---
title: Konfigurieren von DataSource mit Salesforce in AEM Forms 6.3 und 6.4
description: Integrieren von AEM Forms mit Salesforce mithilfe des Formulardatenmodells
feature: Adaptive Forms, Form Data Model
topics: integrations
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 7a4fd109-514a-41a8-a3fe-53c1de32cb6d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '892'
ht-degree: 0%

---

# Konfigurieren von DataSource mit Salesforce in AEM Forms 6.3 und 6.4{#configuring-datasource-with-salesforce-in-aem-forms-and}

## Voraussetzungen {#prerequisites}

In diesem Artikel gehen wir durch den Prozess der Erstellung einer Datenquelle mit Salesforce

Voraussetzungen für dieses Tutorial:

* Scrollen Sie nach unten auf dieser Seite, laden Sie die Swagger-Datei herunter und speichern Sie sie auf Ihrer Festplatte.
* AEM Forms mit aktiviertem SSL

   * [Offizielle Dokumentation zur Aktivierung von SSL in AEM 6.3](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [Offizielle Dokumentation zur Aktivierung von SSL in AEM 6.4](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* Sie benötigen ein Salesforce-Konto
* Sie müssen eine Connected App erstellen. Die offizielle Dokumentation von Salesforce zum Erstellen der App wird aufgelistet [here](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0).
* Bereitstellung geeigneter OAuth-Bereiche für die App (ich habe alle verfügbaren OAuth-Bereiche zum Testen ausgewählt)
* Geben Sie die Callback-URL an. Die Callback-URL in meinem Fall war

   * Wenn Sie **AEM Forms 6.3**, lautet die Callback-URL https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html. In dieser URL-Erstellung ist der Name meines Formulardatenmodells.

   * Wenn Sie &quot;AEM Forms 6.4**&quot;verwenden, lautet die Callback-URL https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html

In diesem Beispiel ist gbedekar -w7-1:6443 der Name meines Servers und der Port, auf dem AEM ausgeführt wird.

Nachdem Sie die Connected App erstellt haben, notieren Sie den Hinweis **Consumer Key und Secret Key**. Sie benötigen diese beim Erstellen der Datenquelle in AEM Forms.

Nachdem Sie Ihre verbundene App erstellt haben, müssen Sie eine Swagger-Datei für die Vorgänge erstellen, die Sie in Salesforce ausführen müssen. Eine Beispiel-Swagger-Datei ist als Teil der herunterladbaren Assets enthalten. Mit dieser Swagger-Datei können Sie das &quot;Lead&quot;-Objekt bei der Übermittlung des adaptiven Formulars erstellen. Bitte schauen Sie sich diese Swagger-Datei an.

Der nächste Schritt besteht darin, eine Datenquelle in AEM Forms zu erstellen. Führen Sie je nach AEM Forms-Version die folgenden Schritte aus

## AEM Forms 6.3 {#aem-forms}

* Melden Sie sich mit dem HTTPS-Protokoll bei AEM Forms an
* Navigieren Sie zu Cloud-Services, indem Sie https:// eingeben.&lt;servername>:&lt;serverport> /etc/cloudservices.html, z. B. https://gbedekar-w7-1:6443/etc/cloudservices.html
* Scrollen Sie nach unten zu &quot;Formulardatenmodell&quot;.
* Klicken Sie auf &quot;Konfigurationen anzeigen&quot;.
* Klicken Sie auf &quot;+&quot;, um eine neue Konfiguration hinzuzufügen.
* Wählen Sie &quot;Rest Full Service&quot;. Geben Sie der Konfiguration einen aussagekräftigen Titel und Namen. Beispiel:

   * Name: CreateLeadInSalesForce
   * Titel: CreateLeadInSalesForce

* Klicken Sie auf &quot;Erstellen&quot;

**Im nächsten Bildschirm **

* Wählen Sie &quot;Datei&quot; als Option für Ihre Swagger-Quelldatei aus. Navigieren Sie zu der Datei, die Sie zuvor heruntergeladen haben
* Wählen Sie Authentifizierungstyp als OAuth2.0 aus.
* Geben Sie die Werte ClientID und Client Secret an
* OAuth-URL lautet - **https://login.salesforce.com/services/oauth2/authorize**
* Token-URL aktualisieren - **https://na5.salesforce.com/services/oauth2/token**
* **Zugriffstoken-URL - https://na5.salesforce.com/services/oauth2/token**
* Autorisierungsbereich: ** api chatter_api full id openid refresh_token visualforce web**
* Authentifizierungs-Handler: Autorisierungsanbieter
* Klicken Sie auf &quot;Mit OAUTH verbinden&quot;. Wenn alles gut läuft, sollten keine Fehler angezeigt werden.

Nachdem Sie Ihr Formulardatenmodell mit Salesforce erstellt haben, können Sie die Formulardatenintegration mithilfe der soeben erstellten Datenquelle erstellen. Die offizielle Dokumentation zum Erstellen der Formulardatenintegration finden Sie unter [here](https://helpx.adobe.com/aem-forms/6-3/data-integration.html).

Stellen Sie sicher, dass Sie das Formulardatenmodell so konfigurieren, dass es den POST-Dienst einbezieht, um ein Lead-Objekt in SFDC zu erstellen.

Sie müssen außerdem den Lese- und Schreibdienst für das Lead-Objekt konfigurieren. Die Screenshots finden Sie unten auf dieser Seite.

Nachdem Sie das Formulardatenmodell erstellt haben, können Sie auf diesem Modell basierende adaptive Forms erstellen und mithilfe der Übermittlungsmethoden für Formulardatenmodelle Lead in SFDC erstellen.

## AEM Forms 6.4 {#aem-forms-1}

* Datenquelle erstellen

   * [Navigieren Sie zu Data Sources](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * Klicken Sie auf die Schaltfläche &quot;Erstellen&quot;
   * Geben Sie einige aussagekräftige Werte an

      * Name: CreateLeadInSalesForce
      * Titel: CreateLeadInSalesForce
      * Diensttyp: RESTful-Dienst
   * Klicken Sie auf Weiter
   * Swagger-Quelle: Datei
   * Suchen und wählen Sie die Swagger-Datei aus, die Sie im vorherigen Schritt heruntergeladen haben
   * Authentifizierungstyp: OAuth 2.0. Geben Sie die folgenden Werte an
   * Geben Sie die Werte ClientID und Client Secret an
   * OAuth-URL lautet - **https://login.salesforce.com/services/oauth2/authorize**
   * Token-URL aktualisieren - **https://na5.salesforce.com/services/oauth2/token**
   * Zugriffstoken-Benutzer **l - https://na5.salesforce.com/services/oauth2/token**
   * Autorisierungsbereich: ** api chatter_api full id openid refresh_token visualforce web**
   * Authentifizierungs-Handler: Autorisierungsanbieter
   * Klicken Sie auf die Schaltfläche &quot;Mit OAuth verbinden&quot;. Sollten Fehler auftreten, überprüfen Sie bitte die vorhergehenden Schritte, um sicherzustellen, dass alle Informationen korrekt eingegeben wurden.


Nachdem Sie Ihre Datenquelle mit SalesForce erstellt haben, können Sie die Formulardatenintegration mithilfe der soeben erstellten Datenquelle erstellen. Der Dokumentationslink dafür lautet [here](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)

Stellen Sie sicher, dass Sie das Formulardatenmodell so konfigurieren, dass es den POST-Dienst einbezieht, um ein Lead-Objekt in SFDC zu erstellen.

Sie müssen außerdem den Lese- und Schreibdienst für das Lead-Objekt konfigurieren. Die Screenshots finden Sie unten auf dieser Seite.

Nachdem Sie das Formulardatenmodell erstellt haben, können Sie auf diesem Modell basierende adaptive Forms erstellen und mithilfe der Übermittlungsmethoden für Formulardatenmodelle Lead in SFDC erstellen.

>[!NOTE]
>
>Stellen Sie sicher, dass die URL in der Swagger-Datei Ihrer Region entspricht. Beispielsweise lautet die URL in der Beispiel-Swagger-Datei &quot;na46.salesforce.com&quot;, da das Konto in Nordamerika erstellt wurde. Am einfachsten ist es, sich bei Ihrem Salesforce-Konto anzumelden und die URL zu überprüfen.

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)

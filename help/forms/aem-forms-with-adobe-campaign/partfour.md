---
title: Kampagne Profil mithilfe des Formulardatenmodells erstellen
seo-title: Kampagne Profil mithilfe des Formulardatenmodells erstellen
description: Schritte zum Erstellen von Adobe Campaign Standard-Profilen mit dem AEM Forms-Formulardatenmodell
seo-description: Schritte zum Erstellen von Adobe Campaign Standard-Profilen mit dem AEM Forms-Formulardatenmodell
uuid: 3216827e-e1a2-4203-8fe3-4e2a82ad180a
feature: Ausgabe-Service
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 461c532e-7a07-49f5-90b7-ad0dcde40984
topic: Entwicklung
role: Entwickler
level: Erfahren
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 3%

---


# Kampagne-Profil mithilfe des Formulardatenmodells {#create-campaign-profile-using-form-data-model} erstellen

Schritte zum Erstellen von Adobe Campaign Standard-Profilen mit dem AEM Forms-Formulardatenmodell

## Benutzerdefinierte Authentifizierung erstellen {#create-custom-authentication}

Beim Erstellen der Datenquelle mit der Swagger-Datei unterstützt AEM Forms die folgenden Authentifizierungstypen

* Kein
* OAuth 2.0
* Einfache Authentifizierung
* API-Schlüssel
* Benutzerdefinierte Authentifizierung

![campaingfdm](assets/campaignfdm.gif)

Wir müssen eine benutzerdefinierte Authentifizierung verwenden, um REST-Aufrufe an Adobe Campaign Standard zu tätigen.

Zur Verwendung der benutzerdefinierten Authentifizierung müssen wir eine OSGi-Komponente entwickeln, die die IAuthentication-Schnittstelle implementiert

Die Methode getAuthDetails muss implementiert werden. Diese Methode gibt das Objekt AuthenticationDetails zurück. Für dieses AuthenticationDetails-Objekt sind die erforderlichen HTTP-Header festgelegt, die zum Aufrufen der REST-API an Adobe Campaign erforderlich sind.

Im Folgenden finden Sie den Code, der bei der Erstellung der benutzerdefinierten Authentifizierung verwendet wurde. Die Methode getAuthDetails führt die gesamte Arbeit aus. Wir erstellen ein AuthenticationDetails-Objekt. Dann fügen wir dem Objekt die entsprechenden HttpHeaders hinzu und geben dieses Objekt zurück.

```java
package aemfd.campaign.core;

import java.io.IOException;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.dermis.authentication.api.IAuthentication;
import com.adobe.aemfd.dermis.authentication.exception.AuthenticationException;
import com.adobe.aemfd.dermis.authentication.model.AuthenticationDetails;
import com.adobe.aemfd.dermis.authentication.model.Configuration;

import aemforms.campaign.core.CampaignService;
import formsandcampaign.demo.CampaignConfigurationService;
@Component(service=IAuthentication.class,immediate=true)

public class CampaignAuthentication implements IAuthentication {
 @Reference
 CampaignService campaignService;
  @Reference
     CampaignConfigurationService config;
private Logger log = LoggerFactory.getLogger(CampaignAuthentication.class);
 @Override
 public AuthenticationDetails getAuthDetails(Configuration arg0) throws AuthenticationException {
 try {
   AuthenticationDetails auth = new AuthenticationDetails();
   auth.addHttpHeader("Cache-Control", "no-cache");
   auth.addHttpHeader("Content-Type", "application/json");
   auth.addHttpHeader("X-Api-Key",config.getApiKey() );
         auth.addHttpHeader("Authorization", "Bearer "+campaignService.getAccessToken());
         log.debug("Returning auth");
         return auth;
   
  } catch (NoSuchAlgorithmException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (InvalidKeySpecException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;
  
 }

 @Override
 public String getAuthenticationType() {
  // TODO Auto-generated method stub
  return "Campaign Custom Authentication";
 }

}
```

## Datenquelle erstellen {#create-data-source}

Der erste Schritt besteht darin, die Swagger-Datei zu erstellen. Die Swagger-Datei definiert die REST-API, die zum Erstellen eines Profils in Adobe Campaign Standard verwendet wird. Die Swagger-Datei definiert die Eingabeparameter und die Ausgabeparameter der REST-API.

Eine Datenquelle wird mithilfe der Swagger-Datei erstellt. Beim Erstellen der Datenquelle können Sie den Authentifizierungstyp angeben. In diesem Fall verwenden wir die benutzerdefinierte Authentifizierung, um sich gegen Adobe Campaign zu authentifizieren. Der oben aufgeführte Code wurde zur Authentifizierung gegen Adobe Campaign verwendet.

Sie erhalten eine Beispieldatei für Swagger als Teil des Assets, das mit diesem Artikel in Verbindung steht.**Vergewissern Sie sich, dass Sie Host und basePath in der Swagger-Datei so ändern, dass sie mit Ihrer ACS-Instanz übereinstimmen.**

## Testen Sie die Lösung {#test-the-solution}

Gehen Sie wie folgt vor, um die Lösung zu testen:
* [Vergewissern Sie sich, dass Sie die hier beschriebenen Schritte ausgeführt haben](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [Laden Sie diese Datei herunter und entpacken Sie sie, um die Swagger-Datei zu erhalten.](assets/create-acs-profile-swagger-file.zip)
* Datenquelle mithilfe der Swagger-Datei erstellen
Formulardatenmodell erstellen und auf der im vorherigen Schritt erstellten Datenquelle basieren
* Erstellen Sie ein adaptives Formular basierend auf dem Formulardatenmodell, das im vorherigen Schritt erstellt wurde.
* Ziehen Sie die folgenden Elemente aus der Registerkarte &quot;Datenquellen&quot;in das adaptive Formular

   * E-Mail
   * Vorname
   * Nachname
   * Mobiltelefon

* Konfigurieren Sie die Übermittlungsaktion auf &quot;Mit Formulardatenmodell senden&quot;.
* Konfigurieren Sie das Datenmodell für eine entsprechende Übermittlung.
* Vorschau des Formulars. Füllen Sie die Felder aus und senden Sie sie ab.
* Überprüfen Sie, ob Profil in Adobe Campaign Standard erstellt wurde.

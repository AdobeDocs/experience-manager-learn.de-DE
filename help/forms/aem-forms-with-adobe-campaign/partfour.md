---
title: Erstellen eines Campaign-Profils mit dem Formulardatenmodell
description: Schritte zum Erstellen eines Adobe Campaign Standard-Profils mit dem AEM Forms-Formulardatenmodell
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 59d5ba6d-91c1-48c7-8c87-8e0caf4f2d7e
duration: 110
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 100%

---

# Erstellen eines Campaign-Profils mit dem Formulardatenmodell {#create-campaign-profile-using-form-data-model}

Schritte zum Erstellen eines Adobe Campaign Standard-Profils mit dem AEM Forms-Formulardatenmodell

## Erstellen einer benutzerdefinierten Authentifizierung {#create-custom-authentication}

Beim Erstellen einer Datenquelle mit der Swagger-Datei unterstützt AEM Forms die folgenden Authentifizierungstypen

* Ohne
* OAuth 2.0
* Standardauthentifizierung
* API-Schlüssel
* Benutzerdefinierte Authentifizierung

![campaingfdm](assets/campaignfdm.gif)

Wir müssen benutzerdefinierte Authentifizierung verwenden, um REST-Aufrufe an Adobe Campaign Standard durchzuführen.

Um die benutzerdefinierte Authentifizierung zu verwenden, müssen wir eine OSGi-Komponente entwickeln, die die Authentifizierungs-Benutzeroberfläche implementiert

Die Methode getAuthDetails muss implementiert werden. Diese Methode gibt ein AuthenticationDetails-Objekt zurück. Für dieses AuthenticationDetails-Objekt sind die erforderlichen HTTP-Header festgelegt, die zum Aufrufen der REST-API an Adobe Campaign benötigt werden.

Im Folgenden finden Sie den Code, der zum Erstellen der benutzerdefinierten Authentifizierung verwendet wurde. Die Methode getAuthDetails führt alle Aufgaben aus. Wir erstellen ein AuthenticationDetails-Objekt. Anschließend fügen wir diesem Objekt die entsprechenden Http-Header hinzu und geben dieses Objekt zurück.

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

## Erstellen einer Datenquelle {#create-data-source}

Der erste Schritt besteht darin, die Swagger-Datei zu erstellen. Die Swagger-Datei definiert die REST-API, die zum Erstellen eines Profils in Adobe Campaign Standard verwendet wird. Die Swagger-Datei definiert die Eingabeparameter und die Ausgabeparameter der REST-API.

Eine Datenquelle wird mithilfe der Swagger-Datei erstellt. Beim Erstellen der Datenquelle können Sie den Authentifizierungstyp angeben. In diesem Fall verwenden wir eine benutzerdefinierte Authentifizierung, um sich bei Adobe Campaign zu authentifizieren. Der oben aufgeführte Code wurde zur Authentifizierung bei Adobe Campaign verwendet.

Die Beispiel-Swagger-Datei wird Ihnen als Teil des Assets bereitgestellt, das mit diesem Artikel in Verbindung steht.**Stellen Sie sicher, dass Sie Host und basePath in der Swagger-Datei so ändern, dass sie mit Ihrer ACS-Instanz übereinstimmen.**

## Testen der Lösung {#test-the-solution}

Gehen Sie wie folgt vor, um die Lösung zu testen:
* [Vergewissern Sie sich, dass Sie die hier beschriebenen Schritte ausgeführt haben](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [Laden Sie diese Datei herunter und entpacken Sie sie, um die Swagger-Datei zu erhalten.](assets/create-acs-profile-swagger-file.zip)
* Erstellen einer Datenquelle mit der Swagger-Datei
Erstellen Sie ein Formulardatenmodell und basieren Sie es auf der im vorherigen Schritt erstellten Datenquelle.
* Erstellen Sie ein adaptives Formular basierend auf dem Formulardatenmodell, das Sie im vorherigen Schritt erstellt haben.
* Ziehen Sie die folgenden Elemente aus der Registerkarte „Datenquellen“ in das adaptive Formular

   * E-Mail
   * Vorname
   * Nachname
   * Mobiltelefon

* Konfigurieren Sie die Absendeaktion auf „Absenden mit Formulardatenmodell“.
* Konfigurieren Sie das Datenmodell für das entsprechende Absenden.
* Zeigen Sie das Formular in einer Vorschau an. Füllen Sie die Felder aus und senden Sie ab.
* Überprüfen Sie, ob das Profil in Adobe Campaign Standard erstellt wurde.

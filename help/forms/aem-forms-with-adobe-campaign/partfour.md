---
title: Campaign-Profil mit Formulardatenmodell erstellen
seo-title: Campaign-Profil mit Formulardatenmodell erstellen
description: Schritte zum Erstellen eines Adobe Campaign Standard-Profils mit dem AEM Forms-Formulardatenmodell
seo-description: Schritte zum Erstellen eines Adobe Campaign Standard-Profils mit dem AEM Forms-Formulardatenmodell
uuid: 3216827e-e1a2-4203-8fe3-4e2a82ad180a
feature: Ausgabe-Service
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 461c532e-7a07-49f5-90b7-ad0dcde40984
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 3%

---


# Kampagnenprofil mit Formulardatenmodell erstellen {#create-campaign-profile-using-form-data-model}

Schritte zum Erstellen eines Adobe Campaign Standard-Profils mit dem AEM Forms-Formulardatenmodell

## Benutzerdefinierte Authentifizierung erstellen {#create-custom-authentication}

Beim Erstellen der Datenquelle mit der Swagger-Datei unterstützt AEM Forms die folgenden Authentifizierungstypen

* Kein
* OAuth 2.0
* Einfache Authentifizierung
* API-Schlüssel
* Benutzerdefinierte Authentifizierung

![campaignFdm](assets/campaignfdm.gif)

Wir müssen benutzerdefinierte Authentifizierung verwenden, um REST-Aufrufe an Adobe Campaign Standard durchzuführen.

Um die benutzerdefinierte Authentifizierung zu verwenden, müssen wir eine OSGi-Komponente entwickeln, die die IAuthentication-Oberfläche implementiert

Die Methode getAuthDetails muss implementiert werden. Diese Methode gibt das Objekt AuthenticationDetails zurück. Für dieses AuthenticationDetails-Objekt sind die erforderlichen HTTP-Header festgelegt, die zum Aufrufen der REST-API an Adobe Campaign benötigt werden.

Im Folgenden finden Sie den Code, der beim Erstellen der benutzerdefinierten Authentifizierung verwendet wurde. Die Methode getAuthDetails führt alle Aufgaben aus. Wir erstellen ein Objekt &quot;AuthenticationDetails&quot;. Anschließend fügen wir diesem Objekt die entsprechenden HttpHeaders hinzu und geben dieses Objekt zurück.

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

Eine Datenquelle wird mithilfe der Swagger-Datei erstellt. Beim Erstellen der Datenquelle können Sie den Authentifizierungstyp angeben. In diesem Fall verwenden wir eine benutzerdefinierte Authentifizierung, um sich bei Adobe Campaign zu authentifizieren. Der oben aufgeführte Code wurde zur Authentifizierung bei Adobe Campaign verwendet.

Die Beispiel-Swagger-Datei wird Ihnen als Teil des Assets bereitgestellt, das mit diesem Artikel in Verbindung steht.**Stellen Sie sicher, dass Sie Host und basePath in der Swagger-Datei so ändern, dass sie mit Ihrer ACS-Instanz übereinstimmen.**

## Testen Sie die Lösung {#test-the-solution}.

Gehen Sie wie folgt vor, um die Lösung zu testen:
* [Vergewissern Sie sich, dass Sie die hier beschriebenen Schritte ausgeführt haben](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [Laden Sie diese Datei herunter und entpacken Sie sie, um die Swagger-Datei zu erhalten.](assets/create-acs-profile-swagger-file.zip)
* Datenquelle mithilfe der Swagger-Datei erstellen
Formulardatenmodell erstellen und auf der Grundlage der Datenquelle erstellen, die im vorherigen Schritt erstellt wurde
* Erstellen Sie ein adaptives Formular basierend auf dem Formulardatenmodell, das Sie im vorherigen Schritt erstellt haben.
* Ziehen Sie die folgenden Elemente aus der Registerkarte &quot;Datenquellen&quot;in das adaptive Formular

   * E-Mail
   * Vorname
   * Nachname
   * Mobiltelefon

* Konfigurieren Sie die Sendeaktion in &quot;Senden mit Formulardatenmodell&quot;.
* Konfigurieren Sie das Datenmodell für die entsprechende Übermittlung.
* Vorschau des Formulars Füllen Sie die Felder aus und senden Sie sie.
* Überprüfen, ob das Profil in Adobe Campaign Standard erstellt wurde.

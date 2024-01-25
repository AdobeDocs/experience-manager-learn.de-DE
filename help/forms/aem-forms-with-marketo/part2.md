---
title: AEM Forms mit Marketo (Teil 2)
description: Tutorial zur Integration von AEM Forms in Marketo mithilfe des AEM Forms-Formulardatenmodells.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: f8ba3d5c-0b9f-4eb7-8609-3e540341d5c2
duration: 172
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '356'
ht-degree: 100%

---

# Marketo-Authentifizierungsdienst

Die REST-APIs von Marketo sind mit OAuth 2.0 mit zwei Abzweigungen authentifiziert. Zur Authentifizierung bei Marketo muss eine benutzerdefinierte Authentifizierung erstellt werden. Diese benutzerdefinierte Authentifizierung wird normalerweise in ein OSGi-Bundle geschrieben. Der folgende Code zeigt den benutzerdefinierten Authenticator, der im Rahmen dieses Tutorials verwendet wurde.

## Benutzerdefinierter Authentifizierungsdienst

Der folgende Code erstellt das Objekt „AuthenticationDetails“, das über das für die Authentifizierung bei Marketo erforderliche access_token verfügt

```java
package com.marketoandforms.core;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
 
import com.adobe.aemfd.dermis.authentication.api.IAuthentication;
import com.adobe.aemfd.dermis.authentication.exception.AuthenticationException;
import com.adobe.aemfd.dermis.authentication.model.AuthenticationDetails;
import com.adobe.aemfd.dermis.authentication.model.Configuration;
@Component(service={IAuthentication.class}, immediate=true)
public class MarketoAuthenticationService implements IAuthentication {
@Reference
MarketoService marketoService;
    @Override
    public AuthenticationDetails getAuthDetails(Configuration arg0) throws AuthenticationException
    {
        AuthenticationDetails auth = new AuthenticationDetails();
        auth.addHttpHeader("Cache-Control", "no-cache");
        auth.addHttpHeader("Authorization", "Bearer " + marketoService.getAccessToken());
        return auth
    }
 
    @Override
    public String getAuthenticationType() {
        // TODO Auto-generated method stub
        return "AemForms With Marketo";
    }
}
```

Der MarketoAuthenticationService implementiert die IAuthentication-Schnittstelle. Diese Schnittstelle ist Teil des AEM Forms Client SDK. Der Dienst ruft das Zugriffs-Token ab und fügt das Token in die HttpHeader der AuthenticationDetails ein. Sobald die HttpHeader des AuthenticationDetails-Objekts ausgefüllt sind, wird das AuthenticationDetails-Objekt an die Dermis-Ebene des Formulardatenmodells zurückgegeben.

Beachten Sie die von der Methode „getAuthenticationType“ zurückgegebene Zeichenfolge. Diese Zeichenfolge wird verwendet, wenn Sie Ihre Datenquelle konfigurieren.

### Abrufen eines Zugriffs-Tokens

Eine einfache Schnittstelle wird mit einer Methode definiert, die access_token zurückgibt. Der Code für die Klasse, die diese Schnittstelle implementiert, wird weiter unten auf der Seite aufgeführt.

```java
package com.marketoandforms.core;
public interface MarketoService {
    String getAccessToken();
}
```

Der folgende Code stammt aus dem Dienst, der access_token zurückgibt, das beim Ausführen der REST-API-Aufrufe verwendet werden soll. Der Code in diesem Dienst greift auf die Konfigurationsparameter zu, die für den GET-Aufruf erforderlich sind. Wie Sie sehen können, übergeben wir client_id,client_secret in der GET-URL, um access_token zu generieren. access_token wird dann wie generiert an die aufrufende Anwendung zurückgegeben.

```java
package com.marketoandforms.core.impl;
import java.io.IOException;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.ParseException;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.util.EntityUtils;
import org.json.JSONException;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.marketoandforms.core.*; 
@Component(service=MarketoService.class,immediate = true)
public class MarketoServiceImpl implements MarketoService {
    private final Logger log = LoggerFactory.getLogger(getClass());
@Reference
MarketoConfigurationService config;
    @Override
    public String getAccessToken()
    {
        String AUTH_URL = config.getAUTH_URL();
        String CLIENT_ID = config.getCLIENT_ID();
        String CLIENT_SECRET = config.getCLIENT_SECRET();
        String AUTH_PATH = config.getAUTH_PATH();
        HttpClient httpClient = HttpClientBuilder.create().build();
        String getURL = AUTH_URL+AUTH_PATH+"&client_id="+CLIENT_ID+"&client_secret="+CLIENT_SECRET;
        log.debug("The url to get the access token is "+getURL);
        HttpGet httpGet = new HttpGet(getURL);
        httpGet.addHeader("Cache-Control","no-cache");
        try {
            HttpResponse httpResponse = httpClient.execute(httpGet);
            HttpEntity httpEntity = httpResponse.getEntity();
            org.json.JSONObject responseJSON = new org.json.JSONObject(EntityUtils.toString(httpEntity))
            return (String)responseJSON.get("access_token");
        } catch (ClientProtocolException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (ParseException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (JSONException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }
}
```

Der folgende Screenshot zeigt die Konfigurationseigenschaften, die festgelegt werden müssen. Diese Konfigurationseigenschaften werden im oben aufgeführten Code gelesen, um access_token zu erhalten

![config](assets/configuration-settings.png)

### Konfiguration

Der folgende Code wurde zum Erstellen der Konfigurationseigenschaften verwendet. Diese Eigenschaften sind spezifisch für Ihre Marketo-Instanz

```java
package com.marketoandforms.core;
 
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
 
@ObjectClassDefinition(name="Marketo Credentials Service Configuration", description = "Connect Form With Marketo")
public @interface MarketoConfiguration {
     @AttributeDefinition(name="Identity Endpoint", description="URL of Marketo Identity Endpoint")
     String identityEndpoint() default "";
      @AttributeDefinition(name="Authentication path", description="Marketo authentication path")
      String authPath() default "";
      @AttributeDefinition(name="Client ID", description="Client ID")
      String clientID() default "";
      @AttributeDefinition(name="Client Secret", description="Client Secret")
      String clientSecret() default "";
}
```

Der folgende Code liest die Konfigurationseigenschaften und gibt diese über die Getter-Methoden zurück

```java
package com.marketoandforms.core;
 
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
@Component(immediate=true, service={MarketoConfigurationService.class})
@Designate(ocd=MarketoConfiguration.class)
public class MarketoConfigurationService {
    private final Logger log = LoggerFactory.getLogger(getClass());
    private MarketoConfiguration config;
    private String AUTH_URL;
    private String  AUTH_PATH;
    private String CLIENT_ID ;
    private String CLIENT_SECRET;
     @Activate
      protected final void activate(MarketoConfiguration config) {
        System.out.println("####In my marketo activating auth service");
        AUTH_URL = config.identityEndpoint();
        AUTH_PATH = config.authPath();
        CLIENT_ID = config.clientID();
        CLIENT_SECRET = config.clientSecret();
        log.info("clientID:" + CLIENT_ID);
        System.out.println("The client id is "+CLIENT_ID+"AUTH PATH"+AUTH_PATH);
      }
    public String getAUTH_URL() {
        return AUTH_URL;
    }
   public String getAUTH_PATH() {
        return AUTH_PATH;
    }
    public String getCLIENT_ID() {
        return CLIENT_ID;
    }

    public String getCLIENT_SECRET() {
        return CLIENT_SECRET;
    }
}
```

1. Erstellen Sie das Bundle und stellen Sie es auf Ihrem AEM-Server bereit.
1. [Lassen Sie Ihren Browser auf „configMgr“ verweisen](http://localhost:4502/system/console/configMgr) und suchen Sie nach „Marketo Credentials Service Configuration“
1. Geben Sie die entsprechenden Eigenschaften für Ihre Marketo-Instanz an

## Nächste Schritte

[Erstellen einer RESTful-Dienst-basierten Datenquelle](./part3.md)

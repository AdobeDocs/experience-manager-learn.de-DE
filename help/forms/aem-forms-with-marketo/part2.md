---
title: AEM Forms mit Marketo (Teil 2)
seo-title: AEM Forms mit Marketo (Teil 2)
description: Lernprogramm zur Integration von AEM Forms in Marketing mit dem AEM Forms-Formulardatenmodell.
seo-description: Lernprogramm zur Integration von AEM Forms in Marketing mit dem AEM Forms-Formulardatenmodell.
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 1%

---


# Marketing-Authentifizierungsdienst

Die REST-APIs von Marketo werden mit 2-Legierungen OAuth 2.0 authentifiziert. Wir müssen eine benutzerdefinierte Authentifizierung erstellen, um sich gegen Marketo zu authentifizieren. Diese benutzerdefinierte Authentifizierung wird normalerweise in einem OSGI-Bundle geschrieben. Der folgende Code zeigt den benutzerdefinierten Authentifizierer, der im Rahmen dieses Lernprogramms verwendet wurde.

## Benutzerdefinierter Authentifizierungsdienst

Mit dem folgenden Code wird das AuthenticationDetails-Objekt erstellt, dessen access_token für die Authentifizierung gegen Marketo erforderlich ist

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

Der MarketoAuthenticationService implementiert die IAuthentication-Schnittstelle. Diese Schnittstelle ist Teil des AEM Forms Client SDK. Der Dienst ruft das Zugriffstoken ab und fügt das Token in den HttpHeader der AuthenticationDetails ein. Sobald die HttpHeaders des AuthenticationDetails-Objekts gefüllt sind, wird das AuthenticationDetails-Objekt an die Dermis-Ebene des Formulardatenmodells zurückgegeben.

Bitte beachten Sie die von der Methode getAuthenticationType zurückgegebene Zeichenfolge. Diese Zeichenfolge wird beim Konfigurieren der Datenquelle verwendet.

### Zugriffstoken abrufen

Eine einfache Schnittstelle wird mit einer Methode definiert, die access_token zurückgibt. Der Code für die Klasse, die diese Schnittstelle implementiert, wird weiter unten auf der Seite aufgelistet.

```java
package com.marketoandforms.core;
public interface MarketoService {
    String getAccessToken();
}
```

Der folgende Code ist vom Dienst, der das access_token zurückgibt, das bei der Ausführung der REST-API-Aufrufe verwendet werden soll. Der Code in diesem Dienst greift auf die Konfigurationsparameter zu, die für den GET-Aufruf erforderlich sind. Wie Sie sehen können, übergeben wir die client_id,client_secret in der GET-URL, um das access_token zu generieren. Dieses access_token wird dann an die aufrufende Anwendung zurückgegeben.

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

Der folgende Screenshot zeigt die Konfigurationseigenschaften, die festgelegt werden müssen. Diese Konfigurationseigenschaften werden im oben aufgeführten Code gelesen, um das access_token abzurufen.

![config](assets/marketoconfig.jfif)

### Konfiguration

Der folgende Code wurde zum Erstellen der Konfigurationseigenschaften verwendet. Diese Eigenschaften sind spezifisch für Ihre Marketing-Instanz.

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

Der folgende Code liest die Konfigurationseigenschaften und gibt dasselbe über die get-Methoden zurück

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

1. Erstellen Sie das Bundle und stellen Sie es auf Ihrem AEM Server bereit.
1. [Verweisen Sie Ihren Browser auf die ](http://localhost:4502/system/console/configMgr) configMgrand-Suche nach &quot;Konfiguration des Marketing-Anmeldeinformationsdienstes&quot;.
1. Geben Sie die entsprechenden Eigenschaften für Ihre Marketing-Instanz an

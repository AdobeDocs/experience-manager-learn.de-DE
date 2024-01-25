---
title: Aufrufen interner APIs, die über private Zertifikate verfügen
description: Erfahren Sie, wie Sie interne APIs mit privaten oder selbstsignierten Zertifikaten aufrufen.
feature: Security
version: 6.5, Cloud Service
topic: Security, Development
role: Admin, Architect, Developer
level: Experienced
jira: KT-11548
thumbnail: KT-11548.png
doc-type: Article
last-substantial-update: 2023-08-25T00:00:00Z
exl-id: c88aa724-9680-450a-9fe8-96e14c0c6643
duration: 363
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '467'
ht-degree: 100%

---

# Aufrufen interner APIs, die über private Zertifikate verfügen

Erfahren Sie, wie Sie HTTPS-Aufrufe von AEM zu Web-APIs durchführen, die private oder selbstsignierte Zertifikate verwenden.

>[!VIDEO](https://video.tv.adobe.com/v/3424853?quality=12&learn=on)

Wenn Sie versuchen, eine HTTPS-Verbindung zu einer Web-API herzustellen, die ein selbstsigniertes Zertifikat verwendet, schlägt die Verbindung standardmäßig mit dieser Fehlermeldung fehl:

```
PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
```

Dieses Problem tritt in der Regel auf, wenn **das SSL-Zertifikat der API nicht von einer anerkannten Zertifizierungsstelle ausgestellt wird** und die Java™-Anwendung das SSL/TLS-Zertifikat nicht überprüfen kann.

Im Folgenden erfahren Sie, wie Sie APIs mit privaten oder selbstsignierten Zertifikaten mithilfe von [Apache HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) und **dem globalen TrustStore von AEM** erfolgreich aufrufen können.


## Prototypischer API-Aufruf-Code unter Verwendung von HttpClient

Der folgende Code stellt eine HTTPS-Verbindung zu einer Web-API her:

```java
...
String API_ENDPOINT = "https://example.com";

// Create HttpClientBuilder
HttpClientBuilder httpClientBuilder = HttpClientBuilder.create();

// Create HttpClient
CloseableHttpClient httpClient = httpClientBuilder.build();

// Invoke API
CloseableHttpResponse closeableHttpResponse = httpClient.execute(new HttpGet(API_ENDPOINT));

// Code that reads response code and body from the 'closeableHttpResponse' object
...
```

Der Code verwendet die [HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html)-Bibliotheksklassen von [Apache HttpComponent](https://hc.apache.org/) und deren Methoden.


## HttpClient und Laden des Materials aus dem AEM TrustStore

Um einen API-Endpunkt aufzurufen, der über ein _privates oder selbstsigniertes Zertifikat_ verfügt, muss der `SSLContextBuilder` des [HttpClients](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) mit dem TrustStore von AEM geladen und verwendet werden, um die Verbindung zu erleichtern.

Führen Sie die folgenden Schritte aus:

1. Melden Sie sich bei **AEM Author** als **Admin** an.
1. Navigieren Sie zu **AEM Author > Tools > Sicherheit > Trust Store** und öffnen Sie den **Global Trust Store**. Legen Sie beim erstmaligen Zugriff ein Kennwort für den Global Trust Store fest.

   ![Global Trust Store](assets/internal-api-call/global-trust-store.png)

1. Um ein privates Zertifikat zu importieren, klicken Sie auf **Zertifikatdatei auswählen** und wählen Sie die gewünschte Zertifikatdatei mit einer `.cer`-Erweiterung aus. Klicken Sie auf **Senden**, um sie zu importieren.

1. Aktualisieren Sie den Java™-Code wie unten beschrieben. Beachten Sie Folgendes: um den AEM `KeyStoreService` mit `@Reference` aufrufen zu können, muss der Aufruf-Code eine OSGi-Komponente/Dienst oder ein Sling-Modell sein (und dort `@OsgiService` verwendet werden).

   ```java
   ...
   
   // Get AEM's KeyStoreService reference
   @Reference
   private com.adobe.granite.keystore.KeyStoreService keyStoreService;
   
   ...
   
   // Get AEM TrustStore using KeyStoreService
   KeyStore aemTrustStore = getAEMTrustStore(keyStoreService, resourceResolver);
   
   if (aemTrustStore != null) {
   
       // Create SSL Context
       SSLContextBuilder sslbuilder = new SSLContextBuilder();
   
       // Load AEM TrustStore material into above SSL Context
       sslbuilder.loadTrustMaterial(aemTrustStore, null);
   
       // Create SSL Connection Socket using above SSL Context
       SSLConnectionSocketFactory sslsf = new SSLConnectionSocketFactory(
               sslbuilder.build(), NoopHostnameVerifier.INSTANCE);
   
       // Create HttpClientBuilder
       HttpClientBuilder httpClientBuilder = HttpClientBuilder.create();
       httpClientBuilder.setSSLSocketFactory(sslsf);
   
       // Create HttpClient
       CloseableHttpClient httpClient = httpClientBuilder.build();
   
       // Invoke API
       closeableHttpResponse = httpClient.execute(new HttpGet(API_ENDPOINT));
   
       // Code that reads response code and body from the 'closeableHttpResponse' object
       ...
   } 
   
   /**
    * 
    * Returns the global AEM TrustStore
    * 
    * @param keyStoreService OOTB OSGi service that makes AEM based KeyStore
    *                         operations easy.
    * @param resourceResolver
    * @return
    */
   private KeyStore getAEMTrustStore(KeyStoreService keyStoreService, ResourceResolver resourceResolver) {
   
       // get AEM TrustStore from the KeyStoreService and ResourceResolver
       KeyStore aemTrustStore = keyStoreService.getTrustStore(resourceResolver);
   
       return aemTrustStore;
   }
   
   ...
   ```

   * Fügen Sie den vorkonfigurierten OSGi-Dienst `com.adobe.granite.keystore.KeyStoreService` in Ihre OSGi-Komponente ein.
   * Rufen Sie den globalen AEM TrustStore mit `KeyStoreService` und `ResourceResolver` ab. Dies erfolgt mit der Methode `getAEMTrustStore(...)`.
   * Erstellen Sie ein Objekt von `SSLContextBuilder`, siehe Java™ [API-Details](https://javadoc.io/static/org.apache.httpcomponents/httpcore/4.4.8/index.html?org/apache/http/ssl/SSLContextBuilder.html).
   * Laden Sie den globalen AEM TrustStore mit der `loadTrustMaterial(KeyStore truststore,TrustStrategy trustStrategy)`-Methode in `SSLContextBuilder`.
   * Übergeben Sie in der oben genannten Methode `null` für `TrustStrategy`, um sicherzustellen, dass nur vertrauenswürdige AEM-Zertifikate während der API-Ausführung erfolgreich sind.


>[!CAUTION]
>
>API-Aufrufe mit gültigen CA-Zertifikaten schlagen fehl, wenn sie mit dem genannten Ansatz ausgeführt werden. Nur API-Aufrufe mit vertrauenswürdigen AEM-Zertifikaten können mit dieser Methode erfolgreich sein.
>
>Verwenden Sie den [Standardansatz](#prototypical-api-invocation-code-using-httpclient) zum Ausführen von API-Aufrufen gültiger CA-Zertifikate. Das bedeutet, dass nur APIs, die mit privaten Zertifikaten verknüpft sind, mit der oben genannten Methode ausgeführt werden sollten.

## Vermeiden von JVM-Keystore-Änderungen

Ein herkömmlicher Ansatz zum effektiven Aufruf interner APIs mit privaten Zertifikaten beinhaltet ein Ändern des JVM-Keystore. Hierzu werden die privaten Zertifikate mithilfe des Java™-Befehls [keytool](https://docs.oracle.com/en/java/javase/11/tools/keytool.html#GUID-5990A2E4-78E3-47B7-AE75-6D1826259549) importiert.

Da diese Methode jedoch nicht den Best Practices zur Sicherheit entspricht, bietet AEM eine bessere Option durch die Verwendung des **Global Trust Store** und des [KeyStoreService](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/granite/keystore/KeyStoreService.html).


## Lösungspaket

Das im Video vorgestellte Node.js-Beispielprojekt kann von [hier](assets/internal-api-call/REST-APIs.zip) heruntergeladen werden.

Der AEM-Servlet-Code ist in der Verzweigung `tutorial/web-api-invocation` des WKND Sites-Projekts verfügbar, [siehe](https://github.com/adobe/aem-guides-wknd/tree/tutorial/web-api-invocation/core/src/main/java/com/adobe/aem/guides/wknd/core/servlets).

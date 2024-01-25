---
title: Gegenseitige Authentifizierung (mTLS, Mutual Transport Layer Security) über AEM
description: Erfahren Sie, wie Sie HTTPS-Aufrufe von AEM zu Web-APIs durchführen, für die eine mTLS-Authentifizierung (Mutual Transport Layer Security) erforderlich ist.
feature: Security
version: 6.5, Cloud Service
topic: Security, Development
role: Admin, Architect, Developer
level: Experienced
jira: KT-13881
thumbnail: KT-13881.png
doc-type: Article
last-substantial-update: 2023-10-10T00:00:00Z
exl-id: 7238f091-4101-40b5-81d9-87b4d57ccdb2
duration: 548
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '731'
ht-degree: 100%

---

# Gegenseitige Authentifizierung (mTLS, Mutual Transport Layer Security) über AEM

Erfahren Sie, wie Sie HTTPS-Aufrufe von AEM zu Web-APIs durchführen, für die eine mTLS-Authentifizierung (Mutual Transport Layer Security) erforderlich ist.

>[!VIDEO](https://video.tv.adobe.com/v/3424855?quality=12&learn=on)

Die mTLS- oder bidirektionale TLS-Authentifizierung verbessert die Sicherheit des TLS-Protokolls, da sich **Client und Server gegenseitige authentifizieren müssen**. Diese Authentifizierung erfolgt mithilfe digitaler Zertifikate. Sie wird häufig in Szenarien verwendet, in denen eine starke Sicherheits- und Identitätsüberprüfung von entscheidender Bedeutung ist.

Standardmäßig schlägt die Verbindung bei dem Versuch, eine HTTPS-Verbindung zu einer Web-API herzustellen, für die eine mTLS-Authentifizierung erforderlich ist, mit folgendem Fehler fehl:

```
javax.net.ssl.SSLHandshakeException: Received fatal alert: certificate_required
```

Dieses Problem tritt auf, wenn der Client kein Zertifikat zur Selbstauthentifizierung bereitstellt.

Nachfolgend erfahren Sie, wie Sie APIs, die eine mTLS-Authentifizierung erfordern, mithilfe von [Apache HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) und **AEM KeyStore und TrustStore** erfolgreich aufrufen.


## HttpClient und Laden des AEM KeyStore-Materials

Im Großen und Ganzen sind die folgenden Schritte erforderlich, um eine mTLS-geschützte API über AEM aufzurufen.

### Generierung von AEM-Zertifikaten

Fordern Sie das AEM-Zertifikat in Zusammenarbeit mit dem Sicherheits-Team Ihres Unternehmens an. Das Sicherheits-Team stellt die zertifikatbezogenen Details wie Schlüssel, Zertifikatsignaturanfragen (CSR) bereit oder fragt sie ab und verwendet CSR, um das Zertifikat auszustellen.

Generieren Sie zu Demozwecken die zertifikatbezogenen Details wie Schlüssel, Zertifikatsignaturanfragen (CSR). Im folgenden Beispiel wird eine selbstsignierte Zertifizierungsstelle verwendet, um das Zertifikat auszustellen.

- Generieren Sie zunächst das Zertifikat der internen Zertifizierungsstelle (CA).

  ```shell
  # Create an internal Certification Authority (CA) certificate
  openssl req -new -x509 -days 9999 -keyout internal-ca-key.pem -out internal-ca-cert.pem
  ```

- Generieren Sie das AEM-Zertifikat.

  ```shell
  # Generate Key
  openssl genrsa -out client-key.pem
  
  # Generate CSR
  openssl req -new -key client-key.pem -out client-csr.pem
  
  # Generate certificate and sign with internal Certification Authority (CA)
  openssl x509 -req -days 9999 -in client-csr.pem -CA internal-ca-cert.pem -CAkey internal-ca-key.pem -CAcreateserial -out client-cert.pem
  
  # Verify certificate
  openssl verify -CAfile internal-ca-cert.pem client-cert.pem
  ```

- Konvertieren Sie den privaten AEM-Schlüssel in das DER-Format. Der AEM-KeyStore benötigt den privaten Schlüssel im DER-Format.

  ```shell
  openssl pkcs8 -topk8 -inform PEM -outform DER -in client-key.pem -out client-key.der -nocrypt
  ```

>[!TIP]
>
>Die selbstsignierten CA-Zertifikate werden nur zu Entwicklungszwecken verwendet. Verwenden Sie für die Produktion eine vertrauenswürdige Zertifizierungsstelle (CA), um das Zertifikat auszustellen.


### Zertifikataustausch

Wenn Sie wie oben eine selbstsignierte Zertifizierungsstelle für das AEM-Zertifikat verwenden, senden Sie das Zertifikat oder das Zertifikat der internen Zertifizierungsstelle (CA) an den API-Provider.

Wenn der API-Provider ein selbstsigniertes CA-Zertifikat verwendet, erhalten Sie das Zertifikat oder das Zertifikat der internen Zertifizierungsstelle (CA) vom API-Provider.

### Zertifikatimport

Gehen Sie wie folgt vor, um AEM-Zertifikate zu importieren:

1. Melden Sie sich bei **AEM Author** als **Admin** an.

1. Navigieren Sie zu **AEM Author> Tools> Sicherheit> Benutzer> Vorhandenen Benutzer erstellen oder auswählen**.

   ![Vorhandenen Benutzer erstellen oder auswählen](assets/mutual-tls-authentication/create-or-select-user.png)

   Zu Demozwecken wird ein neuer Benutzer mit dem Namen `mtl-demo-user` erstellt.

1. Klicken Sie auf den Benutzernamen, um die **Benutzereigenschaften** zu öffnen.

1. Öffnen Sie die Registerkarte **Keystore** und klicken Sie auf **Keystore erstellen**. Legen Sie im Dialogfeld **KeyStore-Zugriffskennwort festlegen** ein Kennwort für den Keystore dieses Benutzers fest und klicken Sie auf „Speichern“.

   ![KeyStore erstellen](assets/mutual-tls-authentication/create-keystore.png)

1. Führen Sie im neuen Bildschirm unter dem Abschnitt **PRIVATEN SCHLÜSSEL AUS DER DATEI HINZUFÜGEN** folgende Schritte aus:

   1. Eingeben eines Alias

   1. Importieren Sie den privaten AEM-Schlüssel im zuvor generierten DER-Format.

   1. Importieren Sie die zuvor generierten Zertifikatkettendateien.

   1. Klicken Sie auf „Senden“

      ![Importieren eines privaten AEM-Schlüssels](assets/mutual-tls-authentication/import-aem-private-key.png)

1. Überprüfen Sie, ob das Zertifikat erfolgreich importiert wurde.

   ![Privater AEM-Schlüssel und Zertifikat importiert](assets/mutual-tls-authentication/aem-privatekey-cert-imported.png)

Wenn der API-Provider ein selbstsigniertes CA-Zertifikat verwendet, importieren Sie das erhaltene Zertifikat im AEM TrustStore und führen Sie die Schritte ab [hier](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/call-internal-apis-having-private-certificate.html?lang=de#httpclient-and-load-aem-truststore-material) aus.

Wenn AEM ein selbstsigniertes CA-Zertifikat verwendet, fordern Sie den API-Provider ebenfalls auf, es zu importieren.

### Prototypischer mTLS-API-Aufruf-Code mithilfe von HttpClient

Aktualisieren Sie den Java™-Code wie unten beschrieben. Um die `@Reference`-Anmerkung zum Abrufen des AEM `KeyStoreService`-Dienstes verwenden zu können, muss der Aufruf-Code eine OSGi-Komponente/Dienst oder ein Sling-Modell sein (und dort `@OsgiService` verwendet werden).


```java
...

// Get AEM's KeyStoreService reference
@Reference
private com.adobe.granite.keystore.KeyStoreService keyStoreService;

...

// Get AEM KeyStore using KeyStoreService
KeyStore aemKeyStore = getAEMKeyStore(keyStoreService, resourceResolver);

if (aemKeyStore != null) {

    // Create SSL Context
    SSLContextBuilder sslbuilder = new SSLContextBuilder();

    // Load AEM KeyStore material into above SSL Context with keystore password
    // Ideally password should be encrypted and stored in OSGi config
    sslbuilder.loadKeyMaterial(aemKeyStore, "admin".toCharArray());

    // If API provider cert is self-signed, load AEM TrustStore material into above SSL Context
    // Get AEM TrustStore
    KeyStore aemTrustStore = getAEMTrustStore(keyStoreService, resourceResolver);
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
    closeableHttpResponse = httpClient.execute(new HttpGet(MTLS_API_ENDPOINT));

    // Code that reads response code and body from the 'closeableHttpResponse' object
    ...
} 

/**
 * Returns the AEM KeyStore of a user. In this example we are using the
 * 'mtl-demo-user' user.
 * 
 * @param keyStoreService
 * @param resourceResolver
 * @return AEM KeyStore
 */
private KeyStore getAEMKeyStore(KeyStoreService keyStoreService, ResourceResolver resourceResolver) {

    // get AEM KeyStore of 'mtl-demo-user' user, you can create a user or use an existing one. 
    // Then create keystore and upload key, certificate files.
    KeyStore aemKeyStore = keyStoreService.getKeyStore(resourceResolver, "mtl-demo-user");

    return aemKeyStore;
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

- Fügen Sie den vorkonfigurierten OSGi-Dienst `com.adobe.granite.keystore.KeyStoreService` in Ihre OSGi-Komponente ein.
- Rufen Sie den AEM KeyStore der Benutzerin bzw. des Benutzers mit `KeyStoreService` und `ResourceResolver` ab. Dies erfolgt mit der Methode `getAEMKeyStore(...)`.
- Wenn der API-Provider ein selbstsigniertes Zertifizierungsstellenzertifikat verwendet, rufen Sie den globalen AEM TrustStore ab. Dies erfolgt mit der Methode `getAEMTrustStore(...)`.
- Erstellen Sie ein Objekt von `SSLContextBuilder`, siehe Java™ [API-Details](https://javadoc.io/static/org.apache.httpcomponents/httpcore/4.4.8/index.html?org/apache/http/ssl/SSLContextBuilder.html).
- Laden Sie den AEM KeyStore der Benutzerin bzw. des Benutzers in `SSLContextBuilder` mit der Methode `loadKeyMaterial(final KeyStore keystore,final char[] keyPassword)`.
- Das Keystore-Kennwort ist das Kennwort, das beim Erstellen des Keystores festgelegt wurde. Es sollte in der OSGi-Konfiguration gespeichert werden, siehe [Geheime Konfigurationswerte](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=de#secret-configuration-values).

## Vermeiden von JVM-Keystore-Änderungen

Ein herkömmlicher Ansatz zum effektiven Aufrufen von mTLS-APIs mit privaten Zertifikaten beinhaltet die Änderung des JVM Keystores. Hierzu werden die privaten Zertifikate mithilfe des Java™-Befehls [keytool](https://docs.oracle.com/en/java/javase/11/tools/keytool.html#GUID-5990A2E4-78E3-47B7-AE75-6D1826259549) importiert.

Da diese Methode jedoch nicht den Best Practices zur Sicherheit entspricht, bietet AEM eine bessere Option durch die Verwendung von **benutzerspezifischen KeyStores und Global TrustStore** und [KeyStoreService](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/granite/keystore/KeyStoreService.html).

## Lösungspaket

Das im Video vorgestellte Node.js-Beispielprojekt kann von [hier](assets/internal-api-call/REST-APIs.zip) heruntergeladen werden.

Der AEM-Servlet-Code ist in der Verzweigung `tutorial/web-api-invocation` des WKND Sites-Projekts verfügbar, [siehe](https://github.com/adobe/aem-guides-wknd/tree/tutorial/web-api-invocation/core/src/main/java/com/adobe/aem/guides/wknd/core/servlets).

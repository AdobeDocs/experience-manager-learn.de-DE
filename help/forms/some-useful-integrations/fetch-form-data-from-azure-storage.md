---
title: Speichern einer Formularübermittlung in Azure Storage
description: Speichern von Formulardaten in Azure Storage mit der REST-API
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-10-23T00:00:00Z
jira: KT-14238
duration: 108
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 75%

---

# Abrufen von Daten aus dem Azure-Speicher

In diesem Artikel erfahren Sie, wie Sie ein adaptives Formular mit den Daten ausfüllen, die im Azure-Speicher gespeichert sind.
Es wird davon ausgegangen, dass Sie die Daten des adaptiven Formulars im Azure-Speicher gespeichert haben und jetzt Ihr adaptives Formular mit diesen Daten vorausfüllen möchten.

## Erstellen einer GET-Anfrage

Der nächste Schritt besteht darin, den Code zu schreiben, um die Daten mithilfe der Blob-ID aus dem Azure-Speicher abzurufen. Der folgende Code wurde zum Abrufen der Daten geschrieben: Die URL wurde mithilfe der Werte für „sasToken“ und „storageURI“ aus der OSGi-Konfiguration und der Blob-ID konstruiert, die an die Funktion „getBlobData“ übergeben wurde.

```java
 @Override
public String getBlobData(String blobID) {
    String sasToken = azurePortalConfigurationService.getSASToken();
    String storageURI = azurePortalConfigurationService.getStorageURI();
    log.debug("The SAS Token is " + sasToken);
    log.debug("The Storage URL is " + storageURI);
    String httpGetURL = storageURI + blobID;
    httpGetURL = httpGetURL + sasToken;
    HttpGet httpGet = new HttpGet(httpGetURL);

    org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    CloseableHttpResponse httpResponse = null;
    try {
        httpResponse = httpClient.execute(httpGet);
        HttpEntity httpEntity = httpResponse.getEntity();
        String blobData = EntityUtils.toString(httpEntity);
        log.debug("The blob data I got was " + blobData);
        return blobData;

    } catch (ClientProtocolException e) {

        log.debug("Got Client Protocol Exception " + e.getMessage());
    } catch (IOException e) {

        log.debug("Got IOEXception " + e.getMessage());
    }

    return null;
}
```

Wenn ein adaptives Formular mit einem GUID-Parameter in der URL wiedergegeben wird, ruft die mit der Vorlage verknüpfte benutzerdefinierte Seitenkomponente das adaptive Formular ab und füllt es mit den Daten aus Azure Storage.
Im Folgenden finden Sie den Code im JSP der Seitenkomponente, die mit der Vorlage verknüpft ist

```java
com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage azureStorage = sling.getService(com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage.class);


String guid = request.getParameter("guid");

if(guid!=null&&!guid.isEmpty())
{
    String dataXml = azureStorage.getBlobData(guid);
    slingRequest.setAttribute("data",dataXml);

}
```

## Testen der Lösung

* [Stelllen Sie das benutzerdefinierte OSGi-Bundle bereit.](./assets/SaveAndFetchFromAzure.core-1.0.0-SNAPSHOT.jar)

* [Importieren Sie die benutzerdefinierte Vorlage für das adaptive Formular und die Seitenkomponente, die mit der Vorlage verknüpft ist.](./assets/store-and-fetch-from-azure.zip)

* [Importieren Sie das adaptive Beispielformular.](./assets/bank-account-sample-form.zip)

* Geben Sie die entsprechenden Werte in der Azure Portal-Konfiguration mithilfe der OSGi-Konfigurationskonsole an.

* [Zeigen Sie das Bankkonto-Formular in der Vorschau an und senden Sie es ab](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled).

* Überprüfen Sie, ob die Daten im gewünschten Azure-Speicher-Container gespeichert sind. Kopieren Sie die Blob-ID.

* [Zeigen Sie das Bankkonto-Formular in der Vorschau an](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6) und geben Sie die Blob-ID als einen GUID-Parameter in die URL ein, damit das Formular mit den Daten aus dem Azure-Speicher vorausgefüllt werden kann.


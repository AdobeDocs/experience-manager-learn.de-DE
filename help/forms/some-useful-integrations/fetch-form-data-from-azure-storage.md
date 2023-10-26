---
title: Formularübermittlung in Azure Storage speichern
description: Speichern von Formulardaten in Azure Storage mithilfe der REST-API
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-10-23T00:00:00Z
kt: 14238
source-git-commit: 5e761ef180182b47c4fd2822b0ad98484db23aab
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 2%

---

# Daten aus Azure-Speicher abrufen

In diesem Artikel erfahren Sie, wie Sie ein adaptives Formular mit den in Azure Storage gespeicherten Daten ausfüllen.
Es wird davon ausgegangen, dass Sie die adaptiven Formulardaten im Azure-Speicher gespeichert haben und jetzt Ihr adaptives Formular mit diesen Daten im Voraus ausfüllen möchten.

## Erstellen einer GET-Anfrage

Der nächste Schritt besteht darin, den Code zu schreiben, um die Daten mithilfe der blobID aus dem Azure-Speicher abzurufen. Der folgende Code wurde geschrieben, um die Daten abzurufen. Die URL wurde mithilfe der sasToken- und storageURI-Werte aus der OSGi-Konfiguration und der an die Funktion getBlobData übergebenen blobID erstellt

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

        log.error("Got Client Protocol Exception " + e.getMessage());
    } catch (IOException e) {

        log.error("Got IOEXception " + e.getMessage());
    }

    return null;
}
```

Wenn ein adaptives Formular mit einer `guid` -Parameter in der URL, ruft die mit der Vorlage verknüpfte benutzerdefinierte Seitenkomponente das adaptive Formular ab und füllt es mit den Daten aus Azure Storage.
Die Seitenkomponente, die mit der Vorlage verknüpft ist, hat den folgenden JSP-Code.

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

* [Bereitstellen des benutzerdefinierten OSGi-Bundles](./assets/SaveAndFetchFromAzure.core-1.0.0-SNAPSHOT.jar)

* [Importieren Sie die benutzerdefinierte Vorlage für das adaptive Formular und die Seitenkomponente, die mit der Vorlage verknüpft ist.](./assets/store-and-fetch-from-azure.zip)

* [Importieren Sie das adaptive Beispielformular.](./assets/bank-account-sample-form.zip)

* Geben Sie die entsprechenden Werte in der Azure Portal-Konfiguration mithilfe der OSGi-Konfigurationskonsole an
* [Vorschau erstellen und Bankkonto-Formular übermitteln](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled)

* Überprüfen Sie, ob die Daten im gewünschten Azure-Speicherbehälter gespeichert sind. Kopieren Sie die Blob-ID.
* [Vorschau des Bankkonto-Formulars](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6) und geben Sie die Blob-ID als einen GUID-Parameter in die URL ein, damit das Formular mit den Daten aus Azure Storage vorausgefüllt werden kann.


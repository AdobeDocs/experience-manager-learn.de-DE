---
title: Formularübermittlung in Azure Storage speichern
description: Speichern von Formulardaten in Azure Storage mithilfe der REST-API
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-08-14T00:00:00Z
kt: 13781
exl-id: 2bec5953-2e0c-4ae6-ae98-34492d4cfbe4
source-git-commit: 5e761ef180182b47c4fd2822b0ad98484db23aab
workflow-type: tm+mt
source-wordcount: '597'
ht-degree: 1%

---

# Formularübermittlungen in Azure Storage speichern

In diesem Artikel erfahren Sie, wie Sie REST-Aufrufe durchführen, um gesendete AEM Forms-Daten in Azure Storage zu speichern.
Um gesendete Formulardaten in Azure Storage speichern zu können, müssen die folgenden Schritte ausgeführt werden.

## Azure Storage-Konto erstellen

[Melden Sie sich bei Ihrem Azure Portal-Konto an und erstellen Sie ein Speicherkonto](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal#create-a-storage-account-1). Geben Sie einen aussagekräftigen Namen für Ihr Speicherkonto ein, klicken Sie auf Überprüfen und dann auf Erstellen . Dadurch wird Ihr Speicherkonto mit allen Standardwerten erstellt. Für die Zwecke dieses Artikels haben wir unser Speicherkonto benannt. `aemformstutorial`.


## Container erstellen

Als Nächstes müssen wir einen Container erstellen, in dem die Daten aus Formularübermittlungen gespeichert werden.
Klicken Sie auf der Seite Speicherkonto auf das Menüelement Container auf der linken Seite und erstellen Sie einen Container namens `formssubmissions`. Stellen Sie sicher, dass die öffentliche Zugriffsebene auf &quot;privat&quot;eingestellt ist.
![container](./assets/new-container.png)

## Erstellen von SAS im Container

Wir werden uns über die Shared Access Signature- oder SAS-Autorisierungsmethode für die Interaktion mit dem Azure Storage-Container informieren.
Navigieren Sie zum Container im Speicherkonto, klicken Sie auf die Auslassungspunkte und wählen Sie die Option SAS generieren aus, wie im Screenshot gezeigt.
![sas-on-container](./assets/sas-on-container.png)
Stellen Sie sicher, dass Sie die entsprechenden Berechtigungen und das entsprechende Enddatum angeben, wie im Screenshot unten dargestellt, und klicken Sie auf SAS-Token und URL generieren . Kopieren Sie das Blob SAS-Token und die Blob SAS-URL. Wir werden diese beiden Werte verwenden, um unsere HTTP-Aufrufe durchzuführen
![shared-access-keys](./assets/shared-access-signature.png)


## Blob-SAS-Token und Speicher-URI angeben

Um den Code allgemeiner zu gestalten, können die beiden Eigenschaften mit der OSGi-Konfiguration wie unten dargestellt konfiguriert werden. Die _**aemformstutorial**_ der Name des Speicherkontos, _**formsubmissions**_ ist der Container, in dem die Daten gespeichert werden.
![osgi-configuration](./assets/azure-portal-osgi-configuration.png)


## PUT-Anfrage erstellen

Der nächste Schritt besteht darin, eine PUT-Anfrage zu erstellen, um die gesendeten Formulardaten in Azure Storage zu speichern. Jede Formularübermittlung muss durch eine eindeutige BLOB-ID identifiziert werden. Die eindeutige BLOB-ID wird normalerweise in Ihrem Code erstellt und in die URL der PUT-Anfrage eingefügt.
Im Folgenden finden Sie die Teil-URL der PUT-Anfrage. Die `aemformstutorial` der Name des Speicherkontos ist, ist &quot;formsubmit&quot;der Container, in dem die Daten mit einer eindeutigen BLOB-ID gespeichert werden. Der Rest der URL bleibt gleich.
https://aemformstutorial.blob.core.windows.net/formsubmissions/blobid/sastoken Die folgende Funktion wurde geschrieben, um die gesendeten Formulardaten mithilfe einer PUT-Anfrage in Azure Storage zu speichern. Beachten Sie die Verwendung von Containername und UUID in der URL. Sie können einen OSGi-Dienst oder ein Sling-Servlet mithilfe des unten aufgeführten Beispielcodes erstellen und die Formularübermittlungen in Azure Storage speichern.

```java
 public String saveFormDatainAzure(String formData) {
    log.debug("in SaveFormData!!!!!" + formData);
    String sasToken = azurePortalConfigurationService.getSASToken();
    String storageURI = azurePortalConfigurationService.getStorageURI();
    log.debug("The SAS Token is " + sasToken);
    log.debug("The Storage URL is " + storageURI);
    org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    UUID uuid = UUID.randomUUID();
    String putRequestURL = storageURI + uuid.toString();
    putRequestURL = putRequestURL + sasToken;
    HttpPut httpPut = new HttpPut(putRequestURL);
    httpPut.addHeader("x-ms-blob-type", "BlockBlob");
    httpPut.addHeader("Content-Type", "text/plain");

    try {
        httpPut.setEntity(new StringEntity(formData));

        CloseableHttpResponse response = httpClient.execute(httpPut);
        log.debug("Response code " + response.getStatusLine().getStatusCode());
        if (response.getStatusLine().getStatusCode() == 201) {
            return uuid.toString();
        }
    } catch (IOException e) {
        log.error("Error: " + e.getMessage());
        throw new RuntimeException(e);
    }
    return null;

}
```

## Gespeicherte Daten im Container überprüfen

![form-data-in-container](./assets/form-data-in-container.png)

## Testen der Lösung

* [Bereitstellen des benutzerdefinierten OSGi-Bundles](./assets/SaveAndFetchFromAzure.core-1.0.0-SNAPSHOT.jar)

* [Importieren Sie die benutzerdefinierte Vorlage für das adaptive Formular und die Seitenkomponente, die mit der Vorlage verknüpft ist.](./assets/store-and-fetch-from-azure.zip)

* [Importieren Sie das adaptive Beispielformular.](./assets/bank-account-sample-form.zip)

* Geben Sie die entsprechenden Werte in der Azure Portal-Konfiguration mithilfe der OSGi-Konfigurationskonsole an
* [Vorschau erstellen und Bankkonto-Formular übermitteln](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled)

* Überprüfen Sie, ob die Daten im gewünschten Azure-Speicherbehälter gespeichert sind. Kopieren Sie die Blob-ID.
* [Vorschau des Bankkonto-Formulars](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6) und geben Sie die Blob-ID als einen GUID-Parameter in die URL ein, damit das Formular mit den Daten aus Azure Storage vorausgefüllt werden kann.


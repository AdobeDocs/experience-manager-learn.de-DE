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
source-git-commit: 17f6148ce6f897052d9d13f23e3f1792646eb958
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# Formularübermittlungen in Azure Storage speichern

In diesem Artikel erfahren Sie, wie Sie REST-Aufrufe durchführen, um gesendete AEM Forms-Daten in Azure Storage zu speichern.
Um gesendete Formulardaten in Azure Storage speichern zu können, müssen die folgenden Schritte ausgeführt werden.

## Azure Storage-Konto erstellen

[Melden Sie sich bei Ihrem Azure Portal-Konto an und erstellen Sie ein Speicherkonto](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal#create-a-storage-account-1). Geben Sie einen aussagekräftigen Namen für Ihr Speicherkonto ein, klicken Sie auf Überprüfen und dann auf Erstellen . Dadurch wird Ihr Speicherkonto mit allen Standardwerten erstellt. Für die Zwecke dieses Artikels haben wir unser Speicherkonto benannt. `aemformstutorial`.

## Freigegebenen Zugriff erstellen

Wir werden uns über die Shared Access Signature- oder SAS-Autorisierungsmethode für die Interaktion mit dem Azure Storage-Container informieren.
Klicken Sie auf der Seite Speicherkonto im Portal links auf das Menüelement Freigegebener Zugriff-Signatur , um die neue Seite mit den Einstellungen für den Signaturschlüssel für freigegebenen Zugriff zu öffnen. Stellen Sie sicher, dass Sie die Einstellungen und das entsprechende Enddatum wie im Screenshot unten gezeigt angeben, und klicken Sie auf die Schaltfläche SAS und Verbindungszeichenfolge generieren . Kopieren Sie die Blob Service SAS-URL. Wir werden diese URL verwenden, um unsere HTTP-Aufrufe durchzuführen
![shared-access-keys](./assets/shared-access-signature.png)

## Container erstellen

Als Nächstes müssen wir einen Container erstellen, in dem die Daten aus Formularübermittlungen gespeichert werden.
Klicken Sie auf der Seite Speicherkonto auf das Menüelement Container auf der linken Seite und erstellen Sie einen Container namens `formssubmissions`. Stellen Sie sicher, dass die öffentliche Zugriffsebene auf &quot;privat&quot;eingestellt ist.
![container](./assets/new-container.png)

## PUT-Anfrage erstellen

Der nächste Schritt besteht darin, eine PUT-Anfrage zu erstellen, um die gesendeten Formulardaten in Azure Storage zu speichern. Wir müssen die Blob Service SAS-URL ändern, um den Containernamen und die BLOB-ID in die URL aufzunehmen. Jede Formularübermittlung muss durch eine eindeutige BLOB-ID identifiziert werden. Die eindeutige BLOB-ID wird normalerweise in Ihrem Code erstellt und in die URL der PUT-Anfrage eingefügt.
Im Folgenden finden Sie die Teil-URL der PUT-Anfrage. Die `aemformstutorial` der Name des Speicherkontos ist, ist &quot;formsubmit&quot;der Container, in dem die Daten mit einer eindeutigen BLOB-ID gespeichert werden. Der Rest der URL bleibt gleich.
https://aemformstutorial.blob.core.windows.net/formsubmissions/00cb1a0c-a891-4dd7-9bd2-67a22bef3b8b?...............

Die folgende Funktion wurde geschrieben, um die gesendeten Formulardaten mithilfe einer PUT-Anfrage in Azure Storage zu speichern. Beachten Sie die Verwendung von Containername und UUID in der URL. Sie können einen OSGi-Dienst oder ein Sling-Servlet mithilfe des unten aufgeführten Beispielcodes erstellen und die Formularübermittlungen in Azure Storage speichern.

```java
 public String saveFormDatainAzure(String formData) {
        System.out.println("in SaveFormData!!!!!"+formData);
        org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        UUID uuid = UUID.randomUUID();
        
        String url = "https://aemformstutorial.blob.core.windows.net/formsubmissions/"+uuid.toString();
        url = url+"?sv=2022-11-02&ss=bf&srt=o&sp=rwdlaciytfx&se=2024-06-28T00:42:59Z&st=2023-06-27T16:42:59Z&spr=https&sig=v1MR%2FJuhEledioturDFRTd9e2fIDVSGJuAiUt6wNlkLA%3D";
        HttpPut httpPut = new HttpPut(url);
        httpPut.addHeader("x-ms-blob-type","BlockBlob");
        httpPut.addHeader("Content-Type","text/plain");
        try {
            httpPut.setEntity(new StringEntity(formData));
            CloseableHttpResponse response = httpClient.execute(httpPut);
            log.debug("Response code "+response.getStatusLine().getStatusCode());
        } catch (IOException e) {
            log.error("Error: "+e.getMessage());
            throw new RuntimeException(e);
        }
        return uuid.toString();


    }
```

## Gespeicherte Daten im Container überprüfen

![form-data-in-container](./assets/form-data-in-container.png)






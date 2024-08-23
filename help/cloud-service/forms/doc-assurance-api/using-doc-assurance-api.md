---
title: Verwenden des DocAssurance-API
description: Beispiel-Code zum Aufrufen des DocAssurance-API mithilfe von Apache-HTTP-Komponenten in Java
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-15508
exl-id: 40617082-4d23-4c91-a016-2d947187052b
source-git-commit: 426020f59c7103829b7b7b74acb0ddb7159b39fa
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 100%

---

# Verwenden des DocAssurance-API

Der [DocAssurance-Dienst](https://developer.adobe.com/experience-manager-forms-cloud-service-developer-reference/api/docassurance/#tag/DocAssurance) bietet die Möglichkeit, verschiedene digitale Signatur- oder Verschlüsselungsvorgänge mit PDF-Dokumenten durchzuführen, z. B. Signieren, Zertifizieren, Hinzufügen von Signaturfeldern, Verschlüsseln, Entschlüsseln usw.
Dieser Artikel bietet Ihnen Java-Code-Ausschnitte, die Ihnen den Einstieg in die Verwendung des API erleichtern. Das Codesnippet verwendet ein Zugriffs-Token. [In diesem Artikel werden die zum Generieren eines Zugriffs-Tokens erforderlichen Schritte erläutert](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/forms/doc-gen-formscs/introduction)


<span class="preview">Diese Funktion ist im Rahmen des Early-Adopter-Programms verfügbar. Sie können von Ihrer offiziellen E-Mail-Adresse aus an aem-forms-ea@adobe.com schreiben, um dem Early-Adopter-Programm beizutreten und den Zugriff auf diese Funktion zu beantragen. </span>


## Voraussetzungen

* Erfahrung mit AEM Forms Cloud Service
* Erfahrung im Umgang mit [Apache-HTTP-Komponenten](https://hc.apache.org/httpcomponents-client-4.5.x/)
* Zugriff auf die AEM Forms Cloud Service-Umgebung

## Inspizieren eines Dokuments

Verwenden Sie die Inspizierungs-API, um den Sicherheitstyp eines bestimmten PDF-Dokuments abzurufen. Das folgende Codesnippet soll Ihnen den Einstieg erleichtern.

```java
...
File fileToInspect = new File("path_to_your_pdf_file)";
HttpPost httpPost = new HttpPost("<your_aem_forms_instance>/adobe/forms/document/assure/inspect");
httpPost.addHeader("Authorization", "Bearer " + accessToken);
MultipartEntityBuilder builder = MultipartEntityBuilder.create();
byte[] fileContent = FileUtils.readFileToByteArray(fileToInspect);
builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"), "BenefitOverview.pdf");
try
{
    HttpEntity entity = builder.build();
    httpPost.setEntity(entity);
    CloseableHttpClient httpclient = HttpClients.createDefault();
    CloseableHttpResponse response = httpclient.execute(httpPost);
    if (response.getStatusLine().getStatusCode() == 200)   
    {
        String json = EntityUtils.toString(response.getEntity(), StandardCharsets.UTF_8);
        log.info("The mode of encryption is  " + JsonParser.parseString(json).getAsJsonObject().get("mode").getAsString());
    }

} 
catch (Exception e)
{
   log.error(e.getMessage());
}
...
```


## Verschlüsseln eines Dokuments

Verwenden Sie das Verschlüsselungs-API, um PDF-Dokumente mit einem Kennwort zu verschlüsseln. Das folgende Beispiel-Codesnippet verschlüsselt eine bestimmte PDF-Datei.

```java
...
File fileToEncrypt = new File("path_to_your_pdf_file");
HttpPost httpPost = new HttpPost(postURL);
httpPost.addHeader("Authorization", "Bearer " + accessToken ");
MultipartEntityBuilder builder = MultipartEntityBuilder.create(); byte[] fileContent = FileUtils.readFileToByteArray(fileToEncrypt); builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"), "BenefitOverview.pdf");
String config = "{\"mode\":\"ENCRYPT_WITH_PASSWORD\",\"params\":{\"openPassword\":\"adobe\",\"permPassword\":\"systems\",\"permissions\":[\"ALL_PERM\"]}}";
 builder.addTextBody("config", config, ContentType.APPLICATION_JSON);
try
 {
    HttpEntity entity = builder.build();
    httpPost.setEntity(entity);
    CloseableHttpClient httpclient = HttpClients.createDefault();
    CloseableHttpResponse response = httpclient.execute(httpPost);
    if (response.getStatusLine().getStatusCode() == 200)
    {
       InputStream generatedPDF = response.getEntity().getContent();
       byte[] bytes = IOUtils.toByteArray(generatedPDF);
       File encryptedFile = new File("c:\\aem_forms_cs_api\\encrypted.pdf");
       FileOutputStream outputStream = new FileOutputStream(encryptedFile);
       outputStream.write(bytes);
       outputStream.close();
    }

}
catch (Exception e)
 {
    log.error(e.getMessage());
 }

...
```

## Hinzufügen eines Signaturfeldes zu einer PDF-Datei

Verwenden Sie die Signaturfeld-API, um der bereitgestellten PDF-Datei eine Signatur hinzuzufügen. Im folgenden Beispiel-Codesnippet wird ein Signaturfeld namens „SignHere“ auf Seite 4 des Dokuments hinzugefügt

```java
...
File pdfFile = new File(pdfFile1.getPath());
HttpPost httpPost = new HttpPost(postURL);
httpPost.addHeader("Authorization", "Bearer "+accessToken);
MultipartEntityBuilder builder = MultipartEntityBuilder.create();
byte[] fileContent = FileUtils.readFileToByteArray(pdfFile);
builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"), "BenefitOverview.pdf");
builder.addTextBody("field", "SignHere", ContentType.TEXT_PLAIN);
String rectangle = "{\"lowerLeftX\":1,\"lowerLeftY\":40,\"width\":100,\"height\":100}";
builder.addTextBody("rectangle", rectangle, ContentType.APPLICATION_JSON);
```


## Entfernen der Verschlüsselung

Verwenden Sie den PUT-Vorgang auf der Verschlüsselungs-API, um die Verschlüsselung von der bereitgestellten PDF zu entfernen. Das folgende Java-Codesnippet sollte Ihnen bei den ersten Schritten helfen.

```java
...
File fileToDecrypt = new File("path_to_your_pdf_file");

HttpPut httpPut = new HttpPut(putURL);
httpPut.addHeader("Authorization", "Bearer " + accessToken);

MultipartEntityBuilder builder = MultipartEntityBuilder.create();
byte[] fileContent = FileUtils.readFileToByteArray(fileToDecrypt);
builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"), "BenefitOverview.pdf");
builder.addTextBody("config", "systems", ContentType.TEXT_PLAIN);

try {
    HttpEntity entity = builder.build();
    httpPut.setEntity(entity);
    CloseableHttpClient httpclient = HttpClients.createDefault();
    CloseableHttpResponse response = httpclient.execute(httpPut);

if (response.getStatusLine().getStatusCode() == 200) {
        InputStream generatedPDF = response.getEntity().getContent();
        byte[] bytes = IOUtils.toByteArray(generatedPDF);
        File encryptionRemoved = new File("c:\\aem_forms_cs_api\\encryption_removed.pdf");
        FileOutputStream outputStream = new FileOutputStream(encryptionRemoved);
        outputStream.write(bytes);
        outputStream.close();
        httpclient.close();
    }
} catch (Exception e) {
    log.error(e.getMessage());
}
...
```

### Postman-Sammlung

Eine Postman-Sammlung des API kann [zu Testzwecken von hier heruntergeladen werden](assets/DocAssuranceAPI.postman_collection.json). Sie können das API mit der Standardauthentifizierung oder dem Bearer-Token-Typ der Authentifizierung aufrufen.

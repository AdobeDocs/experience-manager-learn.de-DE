---
title: Verwenden der DocAssurance-API
description: Beispielcode zum Aufrufen der DocAssurance-API mithilfe von Apache HTTP Components in Java
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-15508
source-git-commit: 97fbe450823c6122a25dc46c851296094894683e
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Verwenden der DocAssurance-API

Die [DocAssurance-Dienst](https://developer.adobe.com/experience-manager-forms-cloud-service-developer-reference/api/docassurance/#tag/DocAssurance) bietet die Möglichkeit, verschiedene digitale Signatur- oder Verschlüsselungsvorgänge mit PDF-Dokumenten durchzuführen, z. B. Signatur, Zertifizierung, Hinzufügung von Signaturfeldern, Verschlüsselung, Entschlüsselung usw.
Dieser Artikel enthält Java-Code-Snippets, die Ihnen die ersten Schritte mit der API erleichtern. Das Code-Snippet verwendet das Zugriffstoken. [In diesem Artikel werden die Schritte erläutert, die zum Generieren des Zugriffstokens erforderlich sind](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/doc-gen-formscs/introduction)


<span class="preview">Diese Funktion ist im Rahmen des Programms für frühe Anwender verfügbar. Sie können von Ihrer offiziellen E-Mail-ID aus unter aem-forms-ea@adobe.com schreiben, um dem frühen Adopter-Programm beizutreten und Zugriff auf diese Funktion anzufordern</span>


## Voraussetzungen

* Erlebnis mit AEM Forms Cloud Service
* Erlebnis bei der Verwendung von [Apache HTTP-Komponenten](https://hc.apache.org/httpcomponents-client-4.5.x/)
* Zugriff auf die Cloud Service-Umgebung von AEM Forms

## Inspect Document

Verwenden Sie die Überprüfungs-API, um den Sicherheitstyp für ein bestimmtes PDF-Dokument abzurufen. Das folgende Code-Snippet soll Ihnen bei den ersten Schritten helfen.

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


## Dokument verschlüsseln

Verwenden Sie die Verschlüsselungs-API, um PDF-Dokumente mit einem Kennwort zu verschlüsseln. Das folgende Codebeispiel verschlüsselt eine bestimmte PDF.

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

## Signaturfeld zum PDF hinzufügen

Verwenden Sie die Signaturfeld-API, um der bereitgestellten PDF eine Signatur hinzuzufügen. Im folgenden Codebeispiel wird ein Signaturfeld namens SignHere auf Seite 4 des Dokuments hinzugefügt

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


## Verschlüsselung entfernen

Verwenden Sie den PUT-Vorgang auf der Verschlüsselungs-API, um die Verschlüsselung von der bereitgestellten PDF zu entfernen. Das folgende Java-Codefragment sollte Ihnen bei den ersten Schritten helfen.

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

Eine Postman-Sammlung der API kann [von hier heruntergeladen zu Testzwecken](assets/DocAssuranceAPI.postman_collection.json). Sie können die API mit der Standardauthentifizierung oder dem Träger-Token-Typ der Authentifizierung aufrufen.

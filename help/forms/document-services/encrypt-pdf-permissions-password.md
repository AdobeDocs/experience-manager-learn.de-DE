---
title: PDF mit einem Berechtigungskennwort verschlüsseln
description: Verwenden von DocAssuranceService zum Verschlüsseln eines PDF
feature: Document Services
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-15849
last-substantial-update: 2024-07-19T00:00:00Z
exl-id: 5df8581c-a44c-449c-bf3b-8cdf57635c4d
source-git-commit: b823f9e294c42ba258049a942816f9a154a6e1a6
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 31%

---

# PDF mit einem Berechtigungskennwort verschlüsseln

Zum Kopieren, Bearbeiten oder Drucken eines PDF-Dokuments ist ein Berechtigungskennwort erforderlich, das auch als Inhaber oder Master-Kennwort bezeichnet wird. Erfahren Sie, wie Sie mit der API [DocAssuranceService](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/docassurance/client/api/DocAssuranceService.html) ein Berechtigungskennwort programmgesteuert auf eine PDF anwenden

Der folgende JSP-Code verschlüsselt eine PDF mit einem Berechtigungskennwort:

```java
<%--
     Encrypt PDF with permissions password
--%>
    <%@include file="/libs/foundation/global.jsp"%>
<%@ page import="com.adobe.fd.docassurance.client.api.EncryptionOptions,java.util.*,java.io.*,com.adobe.fd.encryption.client.*" %>
    <%@page session="false" %>
<%
    String filePath = request.getParameter("saveLocation");
    InputStream pdfIS = null;
    com.adobe.aemfd.docmanager.Document generatedDocument = null;
    // get the pdf file
    javax.servlet.http.Part pdfPart = request.getPart("pdfFile");
    pdfIS = pdfPart.getInputStream();
    com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);


// encrypt the document with permssions password. You can only print this document
    PasswordEncryptionOptionSpec poSpec = new PasswordEncryptionOptionSpec();    
    poSpec.setCompatability(PasswordEncryptionCompatability.ACRO_X);
    poSpec.setEncryptOption(PasswordEncryptionOption.ALL);
    List<PasswordEncryptionPermission> permissionList = new ArrayList<PasswordEncryptionPermission>();
    permissionList.add(PasswordEncryptionPermission.PASSWORD_PRINT_LOW);
    //hardcoding passwords into code is for demonstration purposes only.In real life scenarios the password is sourced from a secure location
    poSpec.setPermissionPassword("adobe");
    poSpec.setPermissionsRequested(permissionList);
    EncryptionOptions encryptionOptions = EncryptionOptions.getInstance();
    encryptionOptions.setEncryptionType(com.adobe.fd.docassurance.client.api.DocAssuranceServiceOperationTypes.ENCRYPT_WITH_PASSWORD);
    encryptionOptions.setPasswordEncryptionOptionSpec(poSpec);
    com.adobe.fd.docassurance.client.api.DocAssuranceService docAssuranceService = sling.getService(com.adobe.fd.docassurance.client.api.DocAssuranceService.class);
    com.adobe.aemfd.docmanager.Document securedDocument = docAssuranceService.secureDocument(pdfDocument,encryptionOptions,null,null,null);
    securedDocument.copyToFile(new java.io.File(filePath));
    out.println("Document encrypted and saved to " +filePath);
%>
```


**So testen Sie das Beispielpaket auf Ihrem System:**

[Laden Sie das Paket herunter und installieren Sie es mit Package Manager.](assets/encryptpdf.zip)

**Fügen Sie nach der Installation des Pakets die folgenden URLs der OSGi-Konfigurationsdatei des Adobe-Granite-CSRF-Filters hinzu:**

1. [Melden Sie sich bei configMgr an.](http://localhost:4502/system/console/configMgr)
1. Suchen Sie nach „Adobe Granite CSRF Filter“.
1. Fügen Sie den folgenden Pfad in den ausgeschlossenen Abschnitten hinzu und speichern Sie
1. /content/AemFormsSamples/encrypt

## Probe testen

Es gibt verschiedene Möglichkeiten, den Beispiel-Code zu testen. Am schnellsten und einfachsten lässt sich hier die Postman-App verwenden. Postman ermöglicht es Ihnen, POST-Anfragen an Ihren Server zu stellen. Der folgende Screenshot zeigt Ihnen die Anforderungsparameter, die erforderlich sind, damit die POST-Anfrage funktioniert. Bitte geben Sie den entsprechenden Autorisierungstyp an, bevor Sie die Anfrage senden.

![encrypt-pdf-postman](assets/encrypt-pdf-postman.png)
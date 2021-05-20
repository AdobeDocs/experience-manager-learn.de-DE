---
title: HTML5-Formularübermittlung handhaben
description: HTML5-Formularübermittlungshandler erstellen
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5269
thumbnail: kt-5269.jpg
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 5%

---


# HTML5-Formularübermittlung handhaben

HTML5-Formulare können an Servlet gesendet werden, das in AEM gehostet wird. Auf die übermittelten Daten kann im Servlet als Eingabestream zugegriffen werden. Um Ihr HTML5-Formular zu senden, müssen Sie Ihrer Formularvorlage mithilfe von AEM Forms Designer &quot;HTTP-Senden-Schaltfläche&quot;hinzufügen

## Erstellen des Submit-Handlers

Ein einfaches Servlet kann erstellt werden, um die Übermittlung des HTML5-Formulars zu verarbeiten. Die übermittelten Daten können dann mithilfe des folgenden Codes extrahiert werden. Dieses [Servlet](assets/html5-submit-handler.zip) wird Ihnen im Rahmen dieses Tutorials zur Verfügung gestellt. Installieren Sie das [Servlet](assets/html5-submit-handler.zip) mit [Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

Der Code aus Zeile 9 kann zum Aufrufen des J2EE-Prozesses verwendet werden. Stellen Sie sicher, dass Sie [Adobe LiveCycle Client SDK Configuration](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html) konfiguriert haben, wenn Sie den Code zum Aufrufen des J2EE-Prozesses verwenden möchten.

```java
StringBuffer stringBuffer = new StringBuffer();
String line = null;
java.io.InputStreamReader isReader = new java.io.InputStreamReader(request.getInputStream(), "UTF-8");
java.io.BufferedReader reader = new java.io.BufferedReader(isReader);
while ((line = reader.readLine()) != null) {
    stringBuffer.append(line);
}
System.out.println("The submitted form data is " + stringBuffer.toString());
/*
        * java.util.Map params = new java.util.HashMap();
        * params.put("in",stringBuffer.toString());
        * com.adobe.livecycle.dsc.clientsdk.ServiceClientFactoryProvider scfp =
        * sling.getService(com.adobe.livecycle.dsc.clientsdk.
        * ServiceClientFactoryProvider.class);
        * com.adobe.idp.dsc.clientsdk.ServiceClientFactory serviceClientFactory =
        * scfp.getDefaultServiceClientFactory(); com.adobe.idp.dsc.InvocationRequest ir
        * = serviceClientFactory.createInvocationRequest("Test1/NewProcess1", "invoke",
        * params, true);
        * ir.setProperty(com.adobe.livecycle.dsc.clientsdk.InvocationProperties.
        * INVOKER_TYPE,com.adobe.livecycle.dsc.clientsdk.InvocationProperties.
        * INVOKER_TYPE_SYSTEM); com.adobe.idp.dsc.InvocationResponse response1 =
        * serviceClientFactory.getServiceClient().invoke(ir);
        * System.out.println("The response is "+response1.getInvocationId());
        */
```


## Konfigurieren der Sende-URL des HTML5-Formulars

![submit-url](assets/submit-url.PNG)

* Tippen Sie auf die xdp und klicken Sie auf _Properties_->_Advanced_
* Kopieren Sie http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html und fügen Sie es in das Textfeld URL senden ein.
* Klicken Sie auf die Schaltfläche _SaveAndClose_.

### Eintrag in den Ausschlusspfaden hinzufügen

* Navigieren Sie zu [configMgr](http://localhost:4502/system/console/configMgr).
* Suchen Sie nach _Adobe Granite CSRF Filter_
* Fügen Sie den folgenden Eintrag im Abschnitt Ausgeschlossene Pfade hinzu
* _/content/AEMFormsSamples/handlehml5formsubmission_
* Speichern Sie Ihre Änderungen

### Testen des Formulars

* Tippen Sie auf die xdp-Vorlage.
* Klicken Sie auf _Vorschau_->Vorschau als HTML anzeigen
* Geben Sie Daten in das Formular ein und klicken Sie auf &quot;Senden&quot;
* Sie sollten die gesendeten Daten sehen, die in die stdout.log-Datei Ihres Servers geschrieben wurden.

### Zusätzliche Lektion

Dieser [Artikel](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html) zum Generieren von PDF-Dateien aus der Übermittlung von HTML5-Formularen wird ebenfalls empfohlen.





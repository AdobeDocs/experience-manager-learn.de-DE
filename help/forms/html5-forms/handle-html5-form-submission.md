---
title: HTML5-Formularübermittlung bearbeiten
description: HTML5-Formular-Übermittlungshandler erstellen
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5269
thumbnail: kt-5269.jpg
translation-type: tm+mt
source-git-commit: c60a46027cc8d71fddd41aa31dbb569e4df94823
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 4%

---


# HTML5-Formularübermittlung bearbeiten

HTML5-Formulare können an ein in AEM gehostetes Servlet gesendet werden. Auf die gesendeten Daten kann im Servlet als Eingabestream zugegriffen werden. Zum Senden Ihres HTML5-Formulars müssen Sie Ihrer Formularvorlage mit AEM Forms Designer &quot;HTTP-Senden-Schaltfläche&quot;hinzufügen.

## Senden-Handler erstellen

Ein einfaches Servlet kann erstellt werden, um die Übermittlung des HTML5-Formulars zu verarbeiten. Die gesendeten Daten können dann mithilfe des folgenden Codes extrahiert werden. Dieses [Servlet](assets/html5-submit-handler.zip) wird Ihnen im Rahmen dieses Lernprogramms zur Verfügung gestellt. Installieren Sie das [Servlet](assets/html5-submit-handler.zip) mit [Paketmanager](http://localhost:4502/crx/packmgr/index.jsp)

Der Code aus Zeile 9 kann zum Aufrufen des J2EE-Prozesses verwendet werden. Vergewissern Sie sich, dass Sie [Adobe LiveCycle Client SDK Configuration](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html) konfiguriert haben, wenn Sie den Code zum Aufrufen des J2EE-Prozesses verwenden möchten.

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

* Tippen Sie auf die xdp-Datei und klicken Sie auf _Eigenschaften_->_Erweitert_
* Kopieren Sie http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html und fügen Sie es in das Textfeld &quot;URL senden&quot;ein
* Klicken Sie auf die Schaltfläche _SaveAndClose_.

### hinzufügen Eintrag in den Ausschließen-Pfaden

* Navigieren Sie zu [configMgr](http://localhost:4502/system/console/configMgr).
* Suchen Sie nach _Adobe Granite CSRF Filter_
* hinzufügen den folgenden Eintrag im Abschnitt Ausgeschlossene Pfade
* _/content/AEMFormsSamples/handlehml5formsubmission_
* Speichern Sie Ihre Änderungen

### Formular testen

* Tippen Sie auf die xdp-Vorlage.
* Klicken Sie auf _Vorschau_->Vorschau als HTML
* Geben Sie einige Daten in das Formular ein und klicken Sie auf Senden
* Sie sollten die gesendeten Daten sehen, die in die Datei stdout.log Ihres Servers geschrieben wurden.

### Zusätzliche Lesung

Dieser [Artikel](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html) beim Generieren von PDF-Dateien aus der Übermittlung von HTML5-Formularen wird ebenfalls empfohlen.





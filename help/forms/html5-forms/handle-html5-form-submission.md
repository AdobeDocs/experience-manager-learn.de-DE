---
title: Handhabung einer HTML5-Formularübermittlung
description: Erstellen eines HTML5-Formularübermittlungs-Handlers
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
jira: KT-5269
thumbnail: kt-5269.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 93e1262b-0e93-4ba8-aafc-f9c517688ce9
last-substantial-update: 2020-07-07T00:00:00Z
duration: 78
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 100%

---

# Handhabung einer HTML5-Formularübermittlung

HTML5-Formulare können an ein in AEM gehostetes Servlet gesendet werden. Auf die übermittelten Daten kann im Servlet als Eingabe-Stream zugegriffen werden. Um Ihr HTML 5-Formular zu senden, müssen Sie Ihrer Formularvorlage mithilfe von AEM Forms Designer eine Schaltfläche zur HTTP-Übermittlung hinzufügen.

## Erstellen des Übermittlungs-Handlers

Ein einfaches Servlet kann für die Übermittlung des HTML5-Formulars erstellt werden. Die übermittelten Daten können dann mithilfe des folgenden Codes extrahiert werden. Dieses [Servlet](assets/html5-submit-handler.zip) wird Ihnen im Rahmen dieses Tutorials zur Verfügung gestellt. Installieren Sie das [Servlet](assets/html5-submit-handler.zip) mit [Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

Der Code aus Zeile 9 kann zum Aufrufen des J2EE-Prozesses verwendet werden. Stellen Sie sicher, dass Sie die [Adobe LiveCycle Client SDK-Konfiguration](https://helpx.adobe.com/de/aem-forms/6/submit-form-data-livecycle-process.html) eingerichtet haben, wenn Sie den Code zum Aufrufen des J2EE-Prozesses verwenden möchten.

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


## Konfigurieren der Übermittlungs-URL des HTML5-Formulars

![Übermittlungs-URL](assets/submit-url.PNG)

* Klicken Sie auf die XDP-Datei und dann auf _Eigenschaften_ > _Erweitert_.
* Kopieren Sie den Pfad „http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html“ und fügen Sie ihn in das Textfeld „Übermittlungs-URL“ ein.
* Klicken Sie auf die Schaltfläche _Speichern und schließen_.

### Hinzufügen eines Eintrags in die Ausschlusspfade

* Navigieren Sie zu [configMgr](http://localhost:4502/system/console/configMgr).
* Suchen Sie nach _Adobe Granite CSRF Filter_.
* Fügen Sie den folgenden Eintrag im Abschnitt „Ausgeschlossene Pfade“ hinzu:
* _/content/AemFormsSamples/handlehml5formsubmission_
* Speichern Sie Ihre Änderungen

### Testen des Formulars

* Klicken Sie auf die XDP-Vorlage.
* Klicken Sie auf _Vorschau > Vorschau als HTML_.
* Geben Sie Daten in das Formular ein und klicken Sie auf „Senden“.
* Sie sollten die übermittelten Daten in der stdout.log-Datei Ihres Servers sehen.

### Zusätzliche Informationen

Sie sollten auch diesen [Artikel](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html?lang=de) zur PDF-Generierung bei HTML5-Formularübermittlung lesen.

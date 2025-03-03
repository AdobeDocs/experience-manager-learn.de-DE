---
title: Verarbeiten einer HTML5-Formularübermittlung
description: Erstellen Sie einen HTML5-Formularübermittlungs-Handler.
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
duration: 66
source-git-commit: 52b7e6afbfe448fd350e84c3e8987973c87c4718
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 18%

---


# Verarbeiten der Übermittlung von HTML5-Formularen

HTML5-Formulare können an ein Servlet gesendet werden, das in AEM gehostet wird. Auf die übermittelten Daten kann im Servlet als Eingabe-Stream zugegriffen werden. Um Ihr HTML5-Formular zu senden, fügen Sie Ihrer Formularvorlage mit AEM Forms Designer eine „HTTP-Senden-Schaltfläche“ hinzu.

## Erstellen des Übermittlungs-Handlers

Ein einfaches Servlet kann die Übermittlung von HTML5-Formularen verarbeiten. Extrahieren Sie die übermittelten Daten mithilfe des folgenden Codeausschnitts. Laden Sie das [Servlet](assets/html5-submit-handler.zip) herunter, das in diesem Tutorial bereitgestellt wird. Installieren Sie das [Servlet](assets/html5-submit-handler.zip) mithilfe [Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

```java
StringBuffer stringBuffer = new StringBuffer();
String line = null;
java.io.InputStreamReader isReader = new java.io.InputStreamReader(request.getInputStream(), "UTF-8");
java.io.BufferedReader reader = new java.io.BufferedReader(isReader);
while ((line = reader.readLine()) != null) {
    stringBuffer.append(line);
}
System.out.println("The submitted form data is " + stringBuffer.toString());
```

Stellen Sie sicher, dass Sie die [Adobe LiveCycle Client SDK-Konfiguration konfiguriert haben](https://helpx.adobe.com/de/aem-forms/6/submit-form-data-livecycle-process.html) wenn Sie den Code verwenden möchten, um einen J2EE-Prozess aufzurufen.

## Konfigurieren der Übermittlungs-URL des HTML5-Formulars

![Sende-URL](assets/submit-url.PNG)

- Öffnen Sie die XDP-Datei und navigieren Sie _Eigenschaften_ > _Erweitert_.
- Kopieren Sie http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html und fügen Sie es in das Textfeld Übermittlungs-URL ein.
- Klicken Sie auf _Schaltfläche „Speichern und_&quot;.

### Hinzufügen eines Eintrags in die Ausschlusspfade

- Wechseln Sie zu [configMgr](http://localhost:4502/system/console/configMgr).
- Suchen Sie nach _Adobe Granite CSRF Filter_.
- Fügen Sie im Abschnitt „Ausgeschlossene Pfade“ den folgenden Eintrag hinzu: _/content/AemFormsSamples/handlehml5formsubmission_.
- Speichern Sie Ihre Änderungen.

### Testen des Formulars

- Öffnen Sie die XDP-Vorlage.
- Klicken Sie auf _Vorschau_->Vorschau als HTML.
- Geben Sie Daten in das Formular ein und klicken Sie auf Senden.
- Überprüfen Sie die Datei stdout.log des Servers auf die gesendeten Daten.

### Zusätzliche Informationen

Weitere Informationen zum Generieren von PDFs aus HTML5-Formularübermittlungen finden Sie in diesem [Artikel](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html?lang=de).


---
title: Generieren von PDF-Dateien aus der HTML5-Formularübermittlung
seo-title: Generieren von PDF aus HTML5-Formularübermittlung
description: Generieren von PDF-Dateien aus der Übermittlung von Mobile Forms
seo-description: Generieren von PDF-Dateien aus der Übermittlung von Mobile Forms
uuid: 61f07029-d440-44ec-98bc-f2b5eef92b59
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 816f1a75-6ceb-457b-ba18-daf229eed057
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '585'
ht-degree: 0%

---


# Generiert PDF-Dateien aus der HTML5-Formularübermittlung {#generate-pdf-from-htm-form-submission}

Dieser Artikel führt Sie durch die Schritte, die zum Generieren von PDF-Dateien aus einer HTML5-Formularübermittlung (auch Mobile Forms genannt) erforderlich sind. In dieser Demo werden auch die Schritte erläutert, die zum Hinzufügen eines Bildes zum HTML5-Formular und zum Zusammenführen des Bildes in das endgültige PDF-Dokument erforderlich sind.

Um eine Live-Demonstration dieser Funktion zu sehen, besuchen Sie den [Beispielserver](https://forms.enablementadobe.com/content/samples/samples.html?query=0) und suchen Sie nach &quot;Mobile Form To PDF&quot;.

Gehen Sie wie folgt vor, um die übermittelten Daten mit der XDP-Vorlage zusammenzuführen

Schreiben eines Servlets zur Verarbeitung der HTML5-Formularübermittlung

* Innerhalb dieses Servlets speichern Sie die gesendeten Daten
* Zusammenführen dieser Daten mit der xdp-Vorlage zum Generieren von PDF
* PDF-Datei an die aufrufende Anwendung zurücksenden

Im Folgenden finden Sie den Servlet-Code, der die gesendeten Daten aus der Anfrage extrahiert. Anschließend ruft es die benutzerdefinierte Methode documentServices .mobileFormToPDF auf, um das PDF-Dokument abzurufen.

```java
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  StringBuffer stringBuffer = new StringBuffer();
  String line = null;
  try {
   InputStreamReader isReader = new InputStreamReader(request.getInputStream(), "UTF-8");
   BufferedReader reader = new BufferedReader(isReader);
   while ((line = reader.readLine()) != null)
    stringBuffer.append(line);
  } catch (Exception e) {
   System.out.println("Error");
  }
  String xmlData = new String(stringBuffer);
  Document generatedPDF = documentServices.mobileFormToPDF(xmlData);
  try {
   
   InputStream fileInputStream = generatedPDF.getInputStream();
   response.setContentType("application/pdf");
   response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
   response.setContentLength((int) fileInputStream.available());
   OutputStream responseOutputStream = response.getOutputStream();
   int bytes;
   while ((bytes = fileInputStream.read()) != -1) {
    responseOutputStream.write(bytes);
   }
   responseOutputStream.flush();
   responseOutputStream.close();

  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }

 }
```

Um ein Bild zum mobilen Formular hinzuzufügen und dieses Bild im PDF-Dokument anzuzeigen, haben wir Folgendes verwendet

XDP-Vorlage - In der xdp-Vorlage haben wir ein Bildfeld und eine Schaltfläche namens btnAddImage hinzugefügt. Der folgende Code verarbeitet das Klickereignis von btnAddImage in unserem benutzerdefinierten Profil. Wie Sie sehen können, wird das &quot;file1 click&quot;-Ereignis Trigger. Für dieses Anwendungsbeispiel ist in der xdp keine Kodierung erforderlich

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[Benutzerdefiniertes Profil](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles). Die Verwendung eines benutzerdefinierten Profils erleichtert die Bearbeitung von HTML-DOM-Objekten des mobilen Formulars. Ein ausgeblendetes Dateielement wird der Datei &quot;HTML.jsp&quot;hinzugefügt. Wenn der Benutzer auf &quot;Foto hinzufügen&quot;klickt, wird das click -Ereignis des Dateielements Trigger. Dadurch kann der Benutzer das anzuhängende Foto durchsuchen und auswählen. Dann verwenden wir das JavaScript FileReader-Objekt, um die base64-kodierte Zeichenfolge des Bildes zu erhalten. Die base64-Bildzeichenfolge wird im Textfeld im Formular gespeichert. Wenn das Formular übermittelt wird, extrahieren wir diesen Wert und fügen ihn in das img -Element der XML ein. Diese XML wird dann verwendet, um mit der xdp zusammenzuführen und das endgültige PDF zu generieren.

Das für diesen Artikel verwendete benutzerdefinierte Profil wurde Ihnen als Teil der Assets dieses Artikels zur Verfügung gestellt.

```javascript
function readURL(input) {
            if (input.files && input.files[0]) {
                var reader = new FileReader();
                reader.onload = function (e) {
                  window.formBridge.setFieldValue("xfa.form.topmostSubform.Page1.base64image",reader.result);
                    $('.img img').show();
                     $('.img img')
                        .attr('src', e.target.result)
                        .width(180)
                        .height(200)
                };

                reader.readAsDataURL(input.files[0]);
            }
        }
```

Der obige Code wird ausgeführt, wenn das click -Ereignis des Dateielements Trigger wird. In Zeile 5 wird der Inhalt der hochgeladenen Datei als base64-Zeichenfolge extrahiert und im Textfeld gespeichert. Dieser Wert wird dann extrahiert, wenn das Formular an unser Servlet gesendet wird.

Anschließend konfigurieren wir die folgenden Eigenschaften (erweitert) unseres mobilen Formulars in AEM

* Sende-URL - http://localhost:4502/bin/handlemobileformsubmission. Dies ist unser Servlet, das die gesendeten Daten mit der xdp-Vorlage zusammenführt
* HTML Render Profile - Stellen Sie sicher, dass Sie &quot;AddImageToMobileForm&quot;auswählen. Dadurch wird der Code zum Hinzufügen eines Bildes zum Formular Trigger.

Gehen Sie wie folgt vor, um diese Funktion auf Ihrem eigenen Server zu testen:

* [Bereitstellen des AEM Forms DocumentServices-Bundles](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [Bereitstellen des Pakets Entwickeln mit Dienstbenutzer](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Laden Sie das mit diesem Artikel verknüpfte Paket herunter und installieren Sie es.](assets/pdf-from-mobile-form-submission.zip)

* Vergewissern Sie sich, dass die Sende-URL und das HTML-Renderprofil korrekt eingestellt sind, indem Sie die Eigenschaftenseite von [xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp) anzeigen.

* [Anzeigen einer Vorschau der XDP als HTML](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* Fügen Sie dem Formular ein Bild hinzu und senden Sie es. Sie sollten die PDF-Datei mit dem Bild zurückerhalten.


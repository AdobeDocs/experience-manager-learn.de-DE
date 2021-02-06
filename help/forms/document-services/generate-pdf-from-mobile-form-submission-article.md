---
title: Generiert PDF-Dateien aus der HTML5-Formularübermittlung.
seo-title: Generiert PDF-Dateien aus HTML5-Formularübermittlung.
description: Generiert PDF-Dateien aus der Übermittlung des Mobile Forms
seo-description: Generiert PDF-Dateien aus der Übermittlung des Mobile Forms
uuid: 61f07029-d440-44ec-98bc-f2b5eef92b59
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 816f1a75-6ceb-457b-ba18-daf229eed057
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---


# Generiert PDF-Dateien aus der HTML5-Formularübermittlung {#generate-pdf-from-htm-form-submission}

Dieser Artikel führt Sie durch die Schritte zum Generieren von PDF-Dateien aus einer HTML5-Formularübermittlung (auch Mobile Forms genannt). In dieser Demo werden auch die Schritte erläutert, die zum Hinzufügen eines Bildes zum HTML5-Formular und zum Zusammenführen des Bildes in der endgültigen PDF-Datei erforderlich sind.

Um eine Live-Demonstration dieser Funktion anzuzeigen, besuchen Sie bitte den [Beispielserver](https://forms.enablementadobe.com/content/samples/samples.html?query=0) und suchen Sie nach &quot;Mobile Form To PDF&quot;.

Um die gesendeten Daten mit der xdp-Vorlage zusammenzuführen, führen wir die folgenden Schritte aus

Schreiben eines Servlets zur Verarbeitung der HTML5-Formularübermittlung

* In diesem Servlet speichern Sie die gesendeten Daten ab
* Zusammenführen dieser Daten mit der xdp-Vorlage zum Generieren von PDF
* PDF-Datei an die aufrufende Anwendung weiterleiten

Im Folgenden sehen Sie den Servlet-Code, der die gesendeten Daten aus der Anforderung extrahiert. Anschließend wird die benutzerdefinierte Methode documentServices .mobileFormToPDF aufgerufen, um die PDF-Datei abzurufen.

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

Um dem Formular für Mobilgeräte ein Bild hinzuzufügen und dieses Bild im PDF-Format anzuzeigen, haben wir Folgendes verwendet:

XDP-Vorlage - In der XDP-Vorlage haben wir ein Bildfeld und eine Schaltfläche namens btnAddImage hinzugefügt. Der folgende Code behandelt das click-Ereignis des btnAddImage in unserem benutzerdefinierten Profil. Wie Sie sehen können, Trigger der Datei1 klicken Ereignis. Für diesen Verwendungsfall ist keine Kodierung in der xdp erforderlich.

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[Benutzerdefiniertes Profil](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles). Mithilfe von benutzerdefiniertem Profil ist es einfacher, HTML-DOM-Objekte des mobilen Formulars zu bearbeiten. Dem HTML.jsp wird ein ausgeblendetes Dateielement hinzugefügt. Wenn der Benutzer auf &quot;Hinzufügen Ihr Foto&quot;klickt, wird das click-Ereignis des Dateielements Trigger. Dadurch kann der Benutzer das anzuhängende Foto durchsuchen und auswählen. Anschließend verwenden wir das javascript FileReader-Objekt, um die Base64-kodierte Zeichenfolge des Bildes abzurufen. Die base64-Bildzeichenfolge wird im Textfeld im Formular gespeichert. Wenn das Formular gesendet wird, extrahieren wir diesen Wert und fügen ihn in das img-Element der XML ein. Diese XML wird dann zum Zusammenführen mit der xdp verwendet, um die endgültige PDF zu generieren.

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

Der obige Code wird ausgeführt, wenn das click-Ereignis des Dateielements Trigger wird. In Zeile 5 wird der Inhalt der hochgeladenen Datei als base64-Zeichenfolge extrahiert und im Textfeld gespeichert. Dieser Wert wird dann extrahiert, wenn das Formular an unser Servlet gesendet wird.

Anschließend konfigurieren wir die folgenden Eigenschaften (erweitert) unseres mobilen Formulars in AEM

* Sende-URL - http://localhost:4502/bin/handlemobileformsubmission. Dies ist unser Servlet, das die gesendeten Daten mit der xdp-Vorlage zusammenführt.
* HTML-Render-Profil - Stellen Sie sicher, dass Sie &quot;AddImageToMobileForm&quot;auswählen. Dadurch wird der Code zum Hinzufügen eines Bilds zum Formular Trigger.

Gehen Sie wie folgt vor, um diese Funktion auf Ihrem eigenen Server zu testen:

* [AEMFormsDocumentServices-Bundle bereitstellen](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [Bereitstellen des Pakets Entwickeln mit Dienstbenutzern](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Laden Sie das mit diesem Artikel verknüpfte Paket herunter und installieren Sie es.](assets/pdf-from-mobile-form-submission.zip)

* Vergewissern Sie sich, dass die Sende-URL und das HTML-Render-Profil korrekt eingestellt sind, indem Sie die Eigenschaftsseite von [xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp) anzeigen

* [Vorschau der XDP als HTML](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* hinzufügen Sie ein Bild in das Formular und senden Sie es ab. Sie sollten die PDF-Datei mit dem darin enthaltenen Bild zurückerhalten.


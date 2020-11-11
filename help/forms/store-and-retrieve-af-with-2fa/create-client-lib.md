---
title: Erstellen von Clientbibliotheken
description: Erstellen Sie clientlibrary, um das click-Ereignis der Schaltfläche "Speichern und beenden"zu verarbeiten
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 6%

---

# Client-Bibliothek erstellen

Erstellen Sie eine [Client-Bibliothek](https://docs.adobe.com/content/help/de-DE/experience-manager-65/developing/introduction/clientlibs.html) , die den Code zum Aufrufen der Methode `doAjaxSubmitWithFileAttachment` der `guideBridge` API im click-Ereignis der Schaltfläche enthält, die durch die CSS-Klasse- **Speicherung** identifiziert wird.  Wir geben die Daten des adaptiven Formulars `fileMap`und den `mobileNumber` an den Endpunkt weiter, der beim `**/bin/storeafdatawithattachments`

Nach dem Speichern der Formulardaten wird eine eindeutige Anwendungs-ID generiert und dem Benutzer in einem Dialogfeld angezeigt. Wenn das Dialogfeld geschlossen wird, wird der Benutzer zum Formular weitergeleitet, damit er das gespeicherte adaptive Formular mit der eindeutigen Anwendungs-ID abrufen kann.

```java
$(document).ready(function () {
  
  $(".savebutton").click(function () {
    var tel = guideBridge.resolveNode(
      "guide[0].guide1[0].guideRootPanel[0].contactInformation[0].basicContact[0].telephoneNumber[0]"
    );
    var telephoneNumber = tel.value;
    guideBridge.getFormDataString({
      success: function (data) {
        var map = guideBridge._getFileAttachmentMapForSubmit();
        guideBridge.doAjaxSubmitWithFileAttachment(
          "/bin/storeafdatawithattachments",
          {
            data: data.data,
            fileMap: map,
            mobileNumber: telephoneNumber,
          },
          {
            success: function (x) {
              bootbox.alert(
                "This is your reference number.<br>" +
                  x.data.path +
                  " <br>You will need this to retrieve your application",
                function () {
                  console.log(
                    "This was logged in the callback! After the ok button was pressed"
                  );
                  window.location.href =
                    "http://localhost:4502/content/dam/formsanddocuments/myaccountform/jcr:content?wcmmode=disabled";
                }
              );
              console.log(x.data.path);
            },
          },
          guideBridge._getFileAttachmentsList()
        );
      },
    });
  });
});
```

>[!NOTE]
> Wir haben [Bootbox JavaScript-Bibliothek](http://bootboxjs.com/examples.html) zum Anzeigen des Dialogfelds verwendet

Die in diesem Beispiel verwendeten Client-Bibliotheken können hier [heruntergeladen werden](assets/client-libraries.zip)

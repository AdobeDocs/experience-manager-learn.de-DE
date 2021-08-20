---
title: Erstellen von Client-Bibliotheken
description: Erstellen Sie eine Client-Bibliothek, um das Klickereignis der Schaltfläche "Speichern und Beenden"zu verarbeiten.
feature: Adaptive Formulare
type: Tutorial
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
topic: Entwicklung
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 2%

---

# Client-Bibliothek erstellen

Erstellen Sie [client lib](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html) , die den Code zum Aufrufen der Methode `doAjaxSubmitWithFileAttachment` der `guideBridge`-API beim Klickereignis der Schaltfläche enthält, das von der CSS-Klasse **savebutton** identifiziert wird.  Wir übergeben die adaptiven Formulardaten `fileMap` und die `mobileNumber` an den Endpunkt, der unter `**/bin/storeafdatawithattachments` überwacht.

Nachdem die Formulardaten gespeichert wurden, wird eine eindeutige Anwendungs-ID generiert und dem Benutzer in einem Dialogfeld angezeigt. Wenn das Dialogfeld geschlossen wird, wird der Benutzer zum Formular geleitet, über das er das gespeicherte adaptive Formular mit der eindeutigen Anwendungs-ID abrufen kann.

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
> Wir haben [Bootbox-JavaScript-Bibliothek](http://bootboxjs.com/examples.html) verwendet, um das Dialogfeld anzuzeigen

Die in diesem Beispiel verwendeten Client-Bibliotheken können [von hier heruntergeladen](assets/client-libraries.zip) werden.

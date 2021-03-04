---
title: Erstellen von Clientbibliotheken
description: Erstellen Sie clientlibrary, um das click-Ereignis der Schaltfläche "Speichern und beenden"zu verarbeiten
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
topic: Entwicklung
role: Entwickler
level: Zwischenschaltung
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 8%

---

# Client-Bibliothek erstellen

Erstellen Sie [client lib](https://docs.adobe.com/content/help/de-DE/experience-manager-65/developing/introduction/clientlibs.html), die den Code zum Aufrufen der `doAjaxSubmitWithFileAttachment`-API für das click-Ereignis der Schaltfläche enthält, das von der CSS-Klasse **savebutton** identifiziert wird.  `guideBridge`  Die adaptiven Formulardaten `fileMap` und `mobileNumber` werden an den Endpunkt weitergeleitet, der `**/bin/storeafdatawithattachments` überwacht

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
> Wir haben [Bootbox-Javascript-Bibliothek](http://bootboxjs.com/examples.html) verwendet, um das Dialogfeld anzuzeigen

Die in diesem Beispiel verwendeten Clientbibliotheken können [von hier heruntergeladen werden](assets/client-libraries.zip)

---
title: Erstellen von Client-Bibliotheken
description: Erstellen Sie eine Client-Bibliothek, um das Klickereignis der Schaltfläche "Speichern und Beenden"zu verarbeiten.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
topic: Development
role: Developer
level: Intermediate
exl-id: c90eea73-bd44-40af-aa98-d766aa572415
source-git-commit: 420dbb7bab84c0f3e79be0cc6b5cff0d5867f303
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 62%

---

# Erstellen einer Client-Bibliothek

Erstellen [Client-Bibliothek](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=de) , der den Code zum Aufrufen der Methode enthält `doAjaxSubmitWithFileAttachment` des `guideBridge` API für das Klickereignis der Schaltfläche, die von der CSS-Klasse identifiziert wird **Speichern-Schaltfläche**.  Wir übergeben die adaptiven Formulardaten `fileMap` und `mobileNumber` an den Endpunkt, der auf `**/bin/storeafdatawithattachments` hört

Nachdem die Formulardaten gespeichert wurden, wird eine eindeutige Anwendungs-ID generiert und den Benutzenden in einem Dialogfeld angezeigt. Wenn das Dialogfeld geschlossen wird, werden die Benutzenden zum Formular geleitet, über das sie das gespeicherte adaptive Formular mit der eindeutigen Anwendungs-ID abrufen können.

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
                  x.data.applicationID +
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
> Wir haben [Bootbox-JavaScript-Bibliothek](https://bootboxjs.com/examples.html) zum Anzeigen des Dialogfelds

Die in diesem Beispiel verwendeten Client-Bibliotheken können [hier heruntergeladen werden.](assets/store-af-with-attachments-client-lib.zip)

## Nächste Schritte

[Überprüfen von Benutzenden mit dem OTP-Dienst](./verify-users-with-otp.md)

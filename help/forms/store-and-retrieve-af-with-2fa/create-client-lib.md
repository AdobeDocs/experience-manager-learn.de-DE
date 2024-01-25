---
title: Erstellen von Client-Bibliotheken
description: Erstellen einer Client-Bibliothek zur Verarbeitung des Klick-Ereignisses der Schaltfläche „Speichern und Beenden“
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6597
thumbnail: 6597.pg
topic: Development
role: Developer
level: Intermediate
exl-id: c90eea73-bd44-40af-aa98-d766aa572415
duration: 53
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 100%

---

# Erstellen einer Client-Bibliothek

Erstellen Sie eine [Client-Bibliothek](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=de) mit dem Code, der die Methode `doAjaxSubmitWithFileAttachment` der `guideBridge`-API beim Anklicken der durch die CSS-Klasse festgelegten **Speichern-Schaltfläche** aufruft. Wir übergeben die adaptiven Formulardaten `fileMap` und `mobileNumber` an den Endpunkt, der auf `**/bin/storeafdatawithattachments` hört

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
> Zum Aufruf des Dialogfelds haben wir die [Bootbox-JavaScript-Bibliothek](https://bootboxjs.com/examples.html) verwendet

Die in diesem Beispiel verwendeten Client-Bibliotheken können [hier heruntergeladen werden.](assets/store-af-with-attachments-client-lib.zip)

## Nächste Schritte

[Überprüfen von Benutzenden mit dem OTP-Dienst](./verify-users-with-otp.md)

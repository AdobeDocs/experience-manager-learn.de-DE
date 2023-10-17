---
title: Erstellen von MyAccountForm
description: Erstellen Sie das Formular „myaccount“, um das teilweise ausgefüllte Formular bei erfolgreicher Überprüfung der Anwendungs-ID und Telefonnummer abzurufen.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
topic: Development
role: User
level: Beginner
exl-id: 1ecd8bc0-068f-4557-bce4-85347c295ce0
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: ht
source-wordcount: '265'
ht-degree: 100%

---

# Erstellen von MyAccountForm

Das Formular **MyAccountForm** wird verwendet, um das teilweise ausgefüllte adaptive Formular abzurufen, nachdem die Benutzenden die Anwendungs-ID und die Mobiltelefonnummer der Anwendungs-ID überprüft haben.

![Mein Kontoformular](assets/6599.JPG)

Wenn Benutzende die Anwendungs-ID eingeben und auf die Schaltfläche **FetchApplication** klicken, wird die mit der Anwendungs-ID verbundene Mobiltelefonnummer mithilfe des Get-Vorgangs des Formulardatenmodells aus der Datenbank abgerufen.

Dieses Formular nutzt den POST-Aufruf des Formulardatenmodells, um die Mobiltelefonnummer mithilfe von OTP zu verifizieren. Die Übermittlungsaktion des Formulars wird bei erfolgreicher Überprüfung der Mobiltelefonnummer mit folgendem Code ausgelöst. Wir lösen das Klick-Ereignis der Senden-Schaltfläche mit dem Namen **submitForm** aus.

>[!NOTE]
> Sie müssen den API-Schlüssel und das API-Geheimnis für Ihr [Nexmo](https://dashboard.nexmo.com/)-Konto in die entsprechenden Felder von MyAccountForm eingeben

![trigger-submit](assets/trigger-submit.JPG)



Dieses Formular ist mit einer benutzerdefinierten Übermittlungsaktion verknüpft, die die Formularübermittlung an das Servlet auf **/bin/renderaf** weiterleitet:

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

Der Code im Servlet auf **/bin/renderaf** leitet die Anfrage weiter, um das mit den gespeicherten Daten vorausgefüllte adaptive Formular „storeafwithattachments“ zu rendern.


* MyAccountForm kann [hier heruntergeladen werden](assets/my-account-form.zip)

* Die Beispielformulare basieren auf einer [benutzerdefinierten adaptiven Formularvorlage](assets/custom-template-with-page-component.zip), die in AEM importiert werden muss, damit die Beispielformulare korrekt dargestellt werden

* Der [benutzerdefinierte Übermittlungs-Handler](assets/custom-submit-my-account-form.zip), der mit der MyAccountForm-Übermittlung verbunden ist, muss in AEM importiert werden

## Nächste Schritte

[Testen Sie die Lösung, indem Sie die Beispiel-Assets bereitstellen](./deploy-this-sample.md)

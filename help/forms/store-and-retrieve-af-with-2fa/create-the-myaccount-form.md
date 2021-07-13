---
title: Erstellen des MyAccountForm
description: Erstellen Sie das Formular myaccount , um das teilweise ausgefüllte Formular bei erfolgreicher Überprüfung der Anwendungs-ID und Telefonnummer abzurufen.
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
topic: Entwicklung
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 1%

---



# Erstellen des MyAccountForm

Das Formular **MyAccountForm** wird verwendet, um das teilweise ausgefüllte adaptive Formular abzurufen, nachdem der Benutzer die Anwendungs-ID und die Mobiltelefonnummer überprüft hat, die mit der Anwendungs-ID verknüpft sind.

![Mein Kontoformular](assets/6599.JPG)

Wenn der Benutzer die Anwendungs-ID eingibt und auf die Schaltfläche **FetchApplication** klickt, wird die mit der Anwendungs-ID verknüpfte Mobiltelefonnummer mithilfe des Vorgangs Get des Formulardatenmodells aus der Datenbank abgerufen.

Dieses Formular nutzt den Aufruf der POST des Formulardatenmodells, um die Mobiltelefonnummer mithilfe von OTP zu überprüfen. Die Übermittlungsaktion des Formulars wird bei erfolgreicher Überprüfung der Mobiltelefonnummer mit folgendem Code ausgelöst. Wir lösen das Klickereignis der Senden-Schaltfläche mit dem Namen **submitForm** aus.

>[!NOTE]
> Sie müssen den API-Schlüssel und die API-Geheimniswerte für Ihr [Nexmo](https://dashboard.nexmo.com/)-Konto in den entsprechenden Feldern von MyAccountForm angeben.

![Trigger-submit](assets/trigger-submit.JPG)



Dieses Formular ist mit einer benutzerdefinierten Übermittlungsaktion verknüpft, die die Formularübermittlung an das Servlet weiterleitet, das auf **/bin/renderaf** bereitgestellt wurde.

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

Der Code im Servlet, der auf **/bin/renderaf** bereitgestellt wird, leitet die Anforderung weiter, das mit den gespeicherten Daten vorausgefüllte adaptive Formular &quot;storepwithattachments&quot;wiederzugeben.


* Das MyAccountForm kann [hier heruntergeladen werden](assets/my-account-form.zip)

* Beispielformulare basieren auf [benutzerdefinierten adaptiven Formularvorlagen](assets/custom-template-with-page-component.zip), die in AEM importiert werden müssen, damit die Beispielformulare korrekt wiedergegeben werden.

* [Benutzerdefinierter Sende-](assets/custom-submit-my-account-form.zip) Handler, der mit der Übermittlung von MyAccountForm verknüpft ist, muss in AEM importiert werden.

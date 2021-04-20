---
title: MyAccountForm erstellen
description: Erstellen Sie das Formular "myaccount", um das teilweise ausgefüllte Formular bei erfolgreicher Überprüfung der Anwendungs-ID und der Telefonnummer abzurufen.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
topic: Development
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 1%

---



# MyAccountForm erstellen

Das Formular **MyAccountForm** wird zum Abrufen des teilweise ausgefüllten adaptiven Formulars verwendet, nachdem der Benutzer die Anwendungs-ID und die mit der Anwendungs-ID verknüpfte Mobilnummer überprüft hat.

![Mein Kontoformular](assets/6599.JPG)

Wenn der Benutzer die Anwendungs-ID eingibt und auf die Schaltfläche **FetchApplication** klickt, wird die mit der Anwendungs-ID verknüpfte Mobilnummer mithilfe des Vorgangs Get des Formulardatenmodells aus der Datenbank abgerufen.

Dieses Formular verwendet den Aufruf der POST des Formulardatenmodells, um die Mobiltelefonnummer mit OTP zu überprüfen. Die Übermittlungsaktion des Formulars wird bei erfolgreicher Überprüfung der Mobiltelefonnummer mit dem folgenden Code ausgelöst. Wir lösen das click-Ereignis der Senden-Schaltfläche mit dem Namen **submitForm** aus.

>[!NOTE]
> Sie müssen den API-Schlüssel und die für Ihr [Nexmo](https://dashboard.nexmo.com/)-Konto spezifischen Werte für den geheimen API-Schlüssel in den entsprechenden Feldern von MyAccountForm angeben.

![Trigger-submit](assets/trigger-submit.JPG)



Dieses Formular ist mit einer benutzerdefinierten Übermittlungsaktion verknüpft, die die Formularübermittlung an das Servlet weiterleitet, das auf **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

Der Code im Servlet, der auf **/bin/renderaf** gemountet ist, leitet die Anforderung zum Rendern des stoeurewithattachments-adaptiven Formulars weiter, das mit den gespeicherten Daten vorausgefüllt ist.


* Das MyAccountForm kann [von hier heruntergeladen werden](assets/my-account-form.zip)

* Musterformulare basieren auf [einer benutzerdefinierten adaptiven Formularvorlage](assets/custom-template-with-page-component.zip), die in AEM importiert werden muss, damit die Musterformulare korrekt dargestellt werden.

* [Benutzerdefinierter Sende-](assets/custom-submit-my-account-form.zip) Handler, der mit der MyAccountForm-Übermittlung verknüpft ist, muss in AEM importiert werden.

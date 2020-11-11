---
title: MyAccountForm erstellen
description: Erstellen Sie das Formular "myaccount", um das teilweise ausgefüllte Formular bei erfolgreicher Überprüfung der Anwendungs-ID und der Telefonnummer abzurufen.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---



# MyAccountForm erstellen

Das Formular **MyAccountForm** wird zum Abrufen des teilweise ausgefüllten adaptiven Formulars verwendet, nachdem der Benutzer die Anwendungs-ID und die mit der Anwendungs-ID verknüpfte Mobilnummer überprüft hat.

![Mein Kontoformular](assets/6599.JPG)

Wenn der Benutzer die Anwendungs-ID eingibt und auf die Schaltfläche **FetchApplication** klickt, wird die mit der Anwendungs-ID verknüpfte Mobilnummer mithilfe des Vorgangs Get des Formulardatenmodells aus der Datenbank abgerufen.

Dieses Formular verwendet den Aufruf der POST des Formulardatenmodells, um die Mobiltelefonnummer mit OTP zu überprüfen. Die Übermittlungsaktion des Formulars wird bei erfolgreicher Überprüfung der Mobiltelefonnummer mit dem folgenden Code ausgelöst. Wir lösen das click-Ereignis der Senden-Schaltfläche mit dem Namen **submitForm** aus.

>[!NOTE]
> Sie müssen den API-Schlüssel und die für Ihr [Nexmo](https://dashboard.nexmo.com/) -Konto spezifischen Werte für den geheimen API-Schlüssel in den entsprechenden Feldern von MyAccountForm angeben.

![trigger-submit](assets/trigger-submit.JPG)



Dieses Formular ist mit einer benutzerdefinierten Übermittlungsaktion verknüpft, die die Formularübermittlung an das am **/bin/renderaf bereitgestellte Servlet weiterleitet**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

Der Code im Servlet, das auf **/bin/renderaf** gemountet wird, leitet die Anforderung weiter, das stoeuCONE-Anlagen-adaptives Formular zu rendern, das mit den gespeicherten Daten vorausgefüllt ist.


* Das MyAccountForm können Sie hier [herunterladen](assets/my-account-form.zip)

* Musterformulare basieren auf einer [benutzerdefinierten Vorlage](assets/custom-template-with-page-component.zip) für adaptive Formulare, die in AEM importiert werden muss, damit die Musterformulare korrekt wiedergegeben werden können.

* [Der mit der Übermittlung von MyAccountForm verknüpfte benutzerdefinierte Übermittlungshandler](assets/custom-submit-my-account-form.zip) muss in AEM importiert werden.

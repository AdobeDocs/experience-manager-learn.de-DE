---
title: Erstellen des MyAccountForm
description: Erstellen Sie das Formular myaccount , um das teilweise ausgefüllte Formular bei erfolgreicher Überprüfung der Anwendungs-ID und Telefonnummer abzurufen.
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
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# Erstellen des MyAccountForm

Das Formular **MyAccountForm** wird verwendet, um das teilweise ausgefüllte adaptive Formular abzurufen, nachdem der Benutzer die Anwendungs-ID und die Mobiltelefonnummer der Anwendungs-ID überprüft hat.

![Mein Kontoformular](assets/6599.JPG)

Wenn der Benutzer die Anwendungs-ID eingibt und auf die **FetchApplication** -Schaltfläche die Mobiltelefonnummer, die mit der Anwendungs-ID verknüpft ist, mithilfe des Vorgangs Get des Formulardatenmodells aus der Datenbank abgerufen.

Dieses Formular nutzt den Aufruf der POST des Formulardatenmodells, um die Mobiltelefonnummer mithilfe von OTP zu überprüfen. Die Übermittlungsaktion des Formulars wird bei erfolgreicher Überprüfung der Mobiltelefonnummer mit folgendem Code ausgelöst. Wir lösen das Klickereignis der Senden-Schaltfläche mit dem Namen **submitForm**.

>[!NOTE]
> Sie müssen den API-Schlüssel und die API-Geheimniswerte für Ihre [Nexmo](https://dashboard.nexmo.com/) in den entsprechenden Feldern des MyAccountForm

![Trigger-submit](assets/trigger-submit.JPG)



Dieses Formular ist mit einer benutzerdefinierten Übermittlungsaktion verknüpft, die die Formularübermittlung an das Servlet weiterleitet, das auf **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

Der Code im Servlet, der auf bereitgestellt wird **/bin/renderaf** leitet die Anforderung weiter, das mit den gespeicherten Daten vorausgefüllte adaptive Formular &quot;storepwithattachments&quot;wiederzugeben.


* Das MyAccountForm kann [heruntergeladen von hier](assets/my-account-form.zip)

* Beispielformulare basieren auf [benutzerdefinierte adaptive Formularvorlage](assets/custom-template-with-page-component.zip) müssen in AEM importiert werden, damit die Musterformulare korrekt wiedergegeben werden.

* [Benutzerdefinierter Sende-Handler](assets/custom-submit-my-account-form.zip) mit der Übermittlung von MyAccountForm verknüpft ist, muss in AEM importiert werden.

## Nächste Schritte

[Testen Sie die Lösung, indem Sie die Beispiel-Assets bereitstellen.](./deploy-this-sample.md)

---
title: Anzeigen der Sendekennung bei Formularübermittlung
seo-title: Display submission id on form submission
description: Anzeigen der Antwort auf eine Formulardatenmodellübermittlung auf der Dankeseite
seo-description: Display the response of an form data model submission in thank you page
feature: Adaptive Forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13900
last-substantial-update: 2023-09-09T00:00:00Z
exl-id: 18648914-91cc-470d-8f27-30b750eb2f32
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 0%

---

# Dankeseite anpassen

Wenn Sie ein adaptives Formular an einen REST-Endpunkt senden, möchten Sie eine Bestätigungsmeldung anzeigen, die den Benutzer darüber informiert, dass die Formularübermittlung erfolgreich war. Die POST-Antwort enthält Details zur Übermittlung, wie z. B. die Sende-ID und eine gut durchdachte Bestätigungsnachricht, die die Sende-ID enthält, die zu einem besseren Benutzererlebnis beiträgt. Diese Antwort kann auf der Dankeseite angezeigt werden, die mit Ihrem adaptiven Formular konfiguriert wurde.

Der folgende Screenshot zeigt, wie ein Formular mit der Übermittlungsaktion &quot;Formulardatenmodell&quot;mit einer konfigurierten Dankeseite gesendet wird

![thank-you-page](./assets/thank-you-page-fdm-submit.png)

Die POST eines Formulardatenmodells gibt in der Antwort immer ein JSON-Objekt zurück. Diese JSON-Datei ist in der Dankeseite-URL als Abfrageparameter mit dem Namen _fdmSubmitResult_. Sie können diesen Abfrageparameter analysieren und die JSON-Elemente auf der Dankeseite anzeigen.
Der folgende Beispielcode analysiert die JSON-Antwort, um den Wert des Zahlenfelds zu extrahieren. Die entsprechende XML-Datei wird dann erstellt und in der slingRequest übergeben, um das Formular auszufüllen. Dieser Code wird normalerweise in das JSP der Seitenkomponente geschrieben, die mit der Vorlage für adaptive Formulare verknüpft ist.

```java
if(request.getParameter("fdmSubmitResult")!=null)
{
    String fdmSubmitResult =  request.getParameter("fdmSubmitResult");
    String status = request.getParameter("status");
    com.google.gson.JsonObject jsonObject = com.google.gson.JsonParser.parseString(fdmSubmitResult).getAsJsonObject();
    String caseNumber = jsonObject.get("result").getAsJsonObject().get("number").getAsString();
    slingRequest.setAttribute("data","<afData><afUnboundData><data><caseNumber>"+caseNumber+"</caseNumber><status>"+status+"</status></data></afUnboundData></afData>");
}
```

Es wird empfohlen, Ihre Dankeseite auf einer neuen Vorlage für adaptive Formulare zu basieren, mit der Sie den benutzerdefinierten Code schreiben können, um die Antwort aus den Abfrageparametern zu extrahieren.

## Testen der Lösung

Erstellen Sie ein adaptives Formular und konfigurieren Sie es so, dass es mit der Übermittlungsaktion für das Formulardatenmodell gesendet wird.
[Bereitstellen der Beispielvorlage für adaptive Formulare](assets/thank-you-page-template.zip)
Erstellen Sie ein Dankesformular anhand dieser Vorlage Verknüpfen Sie diese Dankeseite mit Ihrem Hauptformular Ändern Sie den JSP-Code im [createXml.jsp](http://localhost:4502/apps/thank-you-page-template/component/page/thankyoupage/createxml.jsp) um die XML zu erstellen, die zum Vorausfüllen des adaptiven Formulars erforderlich ist.
Anzeigen einer Vorschau und Senden des adaptiven Formulars
Die Dankeseite sollte angezeigt und mit Daten vorausgefüllt werden, wie in der XML angegeben

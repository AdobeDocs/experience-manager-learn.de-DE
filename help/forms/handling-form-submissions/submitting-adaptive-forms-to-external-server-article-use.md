---
title: Übermitteln des adaptiven Formulars an einen externen Server
description: Senden des adaptiven Formulars an den REST-Endpunkt, der auf einem externen Server ausgeführt wird
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
topic: Development
role: Developer
level: Beginner
exl-id: 5363c3f7-9006-4430-b647-f3283a366a64
last-substantial-update: 2020-07-07T00:00:00Z
duration: 109
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 100%

---

# Übermitteln des adaptiven Formulars an einen externen Server {#submitting-adaptive-form-to-external-server}

Verwenden Sie die Aktion „An REST-Endpunkt übermitteln“, um die übertragenen Daten einer REST-URL bereitzustellen. Die URL kann sich auf einem internen (dem Server, auf dem das Formular gerendert wird) oder auf einem externen Server befinden.

Normalerweise möchten Kundinnen und Kunden die Formulardaten zur weiteren Verarbeitung an einen externen Server übermitteln.

Um Daten auf einem internen Server bereitzustellen, geben Sie den Pfad der Ressource an. Die Daten werden an den Pfad der Ressource gesendet. Ein Beispiel hierfür ist etwa „&lt;/content/restEndPoint>“. Für diese POST-Anfragen werden die Authentifizierungsinformationen der Übermittlungsanfrage verwendet.

Geben Sie eine URL an, um Daten an einen externen Server zu senden. Das Format der URL ist <http://host:port/path_to_rest_end_point>. Stellen Sie sicher, dass Sie den Pfad zur anonymen Verarbeitung der POST-Anfrage konfiguriert haben.

Für die Zwecke dieses Artikels wurde eine einfache WAR-Datei erstellt, die auf Ihrer Tomcat-Instanz bereitgestellt werden kann. Wenn Ihr Tomcat auf Port 8080 ausgeführt wird, lautet die POST-URL wie folgt:

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

Wenn Sie Ihr adaptives Formular so konfigurieren, dass es an diesen Endpunkt übermittelt wird, können die Formulardaten und die Anhänge, sofern vorhanden, durch den folgenden Code im Servlet extrahiert werden:

```java
System.out.println("form was submitted");
Part attachment = request.getPart("attachments");
if(attachment!=null)
{
    System.out.println("The content type of the attachment added is "+attachment.getContentType());
}
Enumeration<String> params = request.getParameterNames();
while(params.hasMoreElements())
{
String paramName = params.nextElement();
System.out.println("The param Name is "+paramName);
String data = request.getParameter(paramName);System.out.println("The data  is "+data);
}
```

![Formularübermittlung](assets/formsubmission.gif)
Um dies auf Ihrem Server zu testen, gehen Sie wie folgt vor:

1. Installieren Sie Tomcat, falls noch nicht geschehen. [Anweisungen zur Installation von Tomcat finden Sie hier.](https://helpx.adobe.com/de/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. Laden Sie die mit diesem Artikel verbundene [ZIP-Datei](assets/aemformsenablement.zip) herunter. Entpacken Sie die Datei, um die WAR-Datei zu erhalten.
1. Stellen Sie die WAR-Datei auf Ihrem Tomcat-Server bereit.
1. Erstellen Sie ein einfaches adaptives Formular mit der Komponente „Dateianhang“ und konfigurieren Sie die zugehörige Übermittlungsaktion wie im Screenshot oben gezeigt. Die POST-URL ist <http://localhost:8080/AemFormsEnablement/HandleFormSubmission>. Wenn AEM und Tomcat nicht auf „localhost“ ausgeführt werden, ändern Sie die URL entsprechend.
1. Damit die mehrteilige Formulardatenübermittlung an Tomcat möglich ist, fügen Sie das folgende Attribut zum Kontextelement von „&lt;tomcatInstallDir>\conf\context.xml“ hinzu und starten Sie Ihren Tomcat-Server neu.
1. **&lt;Context allowCasualMultipartParsing=&quot;true&quot;>**
1. Zeigen Sie das adaptive Formular in einer Vorschau an, fügen Sie einen Anhang hinzu und übermitteln Sie das Formular. Überprüfen Sie das Fenster der Tomcat-Konsole auf Meldungen.

---
title: Übermitteln des adaptiven Formulars an einen externen Server
seo-title: Submitting Adaptive Form to External Server
description: Senden des adaptiven Formulars an den REST-Endpunkt, der auf einem externen Server ausgeführt wird
seo-description: Submitting Adaptive Form to REST endpoint running on external server
uuid: 1a46e206-6188-4096-816a-d59e9fb43263
feature: Adaptive Forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
topic: Development
role: Developer
level: Beginner
exl-id: 5363c3f7-9006-4430-b647-f3283a366a64
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 12%

---

# Übermitteln des adaptiven Formulars an einen externen Server {#submitting-adaptive-form-to-external-server}

Verwenden Sie die Aktion &quot;An REST-Endpunkt übermitteln&quot;, um die gesendeten Daten an eine REST-URL zu posten. Die URL kann sich auf einem internen (dem Server, auf dem das Formular gerendert wird) oder auf einem externen Server befinden.

Normalerweise möchten Kunden die Formulardaten zur weiteren Verarbeitung an einen externen Server senden.

Um Daten an einen internen Server zu posten, geben Sie einen Pfad der Ressource an. Die Daten werden an den Pfad der Ressource veröffentlicht. Beispiel: &lt;/content restendpoint=&quot;&quot;> . Für solche Post-Anfragen werden die Authentifizierungsinformationen der Sendeanforderung verwendet.

Stellen Sie die URL bereit, um Daten an einen externen Server zu veröffentlichen. Das Format der URL ist <http://host:port/path_to_rest_end_point>. Stellen Sie sicher, dass Sie den Pfad für die anonyme Verarbeitung der POST-Anfrage konfiguriert haben.

Für die Zwecke dieses Artikels habe ich eine einfache War-Datei geschrieben, die auf Ihrer Tomcat-Instanz bereitgestellt werden kann. Wenn Ihr Tomcat auf Port 8080 ausgeführt wird, wird die POST-URL

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

Wenn Sie Ihr adaptives Formular so konfigurieren, dass es an diesen Endpunkt gesendet wird, können die Formulardaten und die Anlagen, falls vorhanden, durch den folgenden Code im Servlet extrahiert werden

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

![formsubmission](assets/formsubmission.gif)
Um dies auf Ihrem Server zu testen, gehen Sie wie folgt vor:

1. Installieren Sie Tomcat, falls noch nicht geschehen. [Anweisungen zur Installation von Tomcat finden Sie hier .](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. Laden Sie die [ZIP-Datei](assets/aemformsenablement.zip) mit diesem Artikel verknüpft ist. Entpacken Sie die Datei, um die War-Datei zu erhalten.
1. Stellen Sie die WAR-Datei auf Ihrem Tomcat-Server bereit.
1. Erstellen Sie ein einfaches adaptives Formular mit der Dateianlagenkomponente und konfigurieren Sie die Sendeaktion wie im Screenshot oben gezeigt. Die POST-URL lautet <http://localhost:8080/AemFormsEnablement/HandleFormSubmission>. Wenn Ihre AEM und Tomcat nicht auf localhost ausgeführt werden, ändern Sie die URL entsprechend.
1. Damit die mehrteilige Formulardatenübermittlung möglich ist, fügen Sie dem Kontextelement der &lt;tomcatinstalldir>\conf\context.xml und starten Sie Ihren Tomcat-Server neu.
1. **&lt;Context allowCasualMultipartParsing=&quot;true&quot;>**
1. Zeigen Sie eine Vorschau des adaptiven Formulars an, fügen Sie einen Anhang hinzu und senden Sie ihn. Überprüfen Sie das Fenster der Tomcat-Konsole auf Meldungen.

---
title: Senden des adaptiven Formulars an den externen Server
seo-title: Senden des adaptiven Formulars an den externen Server
description: Senden des adaptiven Formulars an den REST-Endpunkt, der auf einem externen Server ausgeführt wird
seo-description: Senden des adaptiven Formulars an den REST-Endpunkt, der auf einem externen Server ausgeführt wird
uuid: 1a46e206-6188-4096-816a-d59e9fb43263
feature: adaptive-forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 10%

---


# Senden des adaptiven Formulars an den externen Server {#submitting-adaptive-form-to-external-server}

Verwenden Sie die Aktion An REST-Endpunkt übermitteln, um die gesendeten Daten an eine REST-URL zu senden. Die URL kann sich auf einem internen (der Server, auf dem das Formular wiedergegeben wird) oder auf einem externen Server befinden.

Normalerweise möchten Kunden die Formulardaten zur weiteren Verarbeitung an einen externen Server senden.

Um Daten an einen internen Server zu senden, geben Sie einen Pfad der Ressource an. Die Daten werden an den Pfad der Ressource veröffentlicht. Beispiel: &lt;/content/restEndPoint> . Bei solchen Post-Anfragen werden die Authentifizierungsinformationen der Senden-Anforderung verwendet.

Stellen Sie die URL bereit, um Daten an einen externen Server zu veröffentlichen. The format of the URL is <http://host:port/path_to_rest_end_point>. Vergewissern Sie sich, dass Sie den Pfad für die anonyme Verarbeitung der POST konfiguriert haben.

Für die Zwecke dieses Artikels habe ich eine einfache Kriegsdatei geschrieben, die auf Ihrer Tomcat-Instanz bereitgestellt werden kann. Wenn Ihr tomcat auf Port 8080 ausgeführt wird, wird die POST-URL

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

Wenn Sie das adaptive Formular so konfigurieren, dass es an diesen Endpunkt gesendet wird, können die Formulardaten und die Anlagen, falls vorhanden, mit dem folgenden Code im Servlet extrahiert werden

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

![formsubmission](assets/formsubmission.gif)Um dies auf Ihrem Server zu testen, führen Sie die folgenden Schritte aus:

1. Installieren Sie Tomcat, wenn Sie es nicht bereits haben. [Anweisungen zur Installation von Tomcat finden Sie hier](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. Laden Sie die mit diesem Artikel verknüpfte [ZIP-Datei](assets/aemformsenablement.zip) herunter. Dekomprimieren Sie die Datei, um die Kriegsdatei abzurufen.
1. Stellen Sie die Kriegsdatei auf Ihrem Tomcat-Server bereit.
1. Erstellen Sie ein einfaches adaptives Formular mit der Dateianlagenkomponente und konfigurieren Sie die Sendeaktion, wie im Screenshot oben gezeigt. The POST URL is <http://localhost:8080/AemFormsEnablement/HandleFormSubmission>. Wenn Ihr AEM und Ihre Tomcat nicht auf localhost ausgeführt werden, ändern Sie bitte die URL entsprechend.
1. Um die Übermittlung mehrteiliger Formulardaten zu aktivieren, fügen Sie das folgende Attribut zum Kontextelement von &lt;tomcatInstallDir>\conf\context.xml hinzu und starten Sie Ihren Tomcat-Server neu.
1. **&lt;Context allowCasualMultipartParsing=&quot;true&quot;>**
1. Vorschau des adaptiven Formulars, Hinzufügen eines Anhangs und Senden. Überprüfen Sie, ob Meldungen im Fenster Tomcat-Konsole angezeigt werden.


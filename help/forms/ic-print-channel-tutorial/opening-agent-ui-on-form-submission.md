---
title: Öffnen der Benutzeroberfläche von Agenten beim Übermitteln von POST
seo-title: Opening Agent UI On POST Submission
description: Dies ist Teil 11 des mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments für den Druckkanal. In diesem Teil starten wir die Benutzeroberfläche für Agenten zum Erstellen von Ad-hoc-Korrespondenz bei der Formularübermittlung.
seo-description: This is part 11 of multistep tutorial for creating your first interactive communications document for the print channel. In this part, we will launch the agent ui interface for creating ad-hoc correspondence on form submission.
uuid: 96f34986-a5c3-400b-b51b-775da5d2cbd7
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6168
thumbnail: 40122.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 509b4d0d-9f3c-46cb-8ef7-07e831775086
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 2%

---

# Öffnen der Benutzeroberfläche von Agenten beim Übermitteln von POST

In diesem Teil starten wir die Benutzeroberfläche für Agenten zum Erstellen von Ad-hoc-Korrespondenz bei der Formularübermittlung.

Dieser Artikel führt Sie durch die Schritte, die zum Öffnen der Benutzeroberfläche des Agenten beim Senden eines Formulars erforderlich sind. In der Regel muss der Kundendienstmitarbeiter ein Formular mit Eingabeparametern ausfüllen und die Benutzeroberfläche für den Formularübermittlungsagenten wird mit Daten geöffnet, die vorab vom Vorfülldienst für Formulardatenmodelle ausgefüllt wurden. Die Eingabeparameter für den Vorfülldienst für Formulardatenmodelle werden aus der Formularübermittlung extrahiert.

Das folgende Video zeigt den Anwendungsfall

>[!VIDEO](https://video.tv.adobe.com/v/40122?quality=12&learn=on)

```java
String accountNumber = request.getParameter("accountnumber"))
ParameterMap parameterMap = new ParameterMap();
RequestParameter icLetterId[] = new RequestParameter[1];
icLetterId[0] = new FormFieldRequestParameter("/content/dam/formsanddocuments/retirementstatementprint");
parameterMap.put("documentId", icLetterId);
RequestParameter Random[] = new RequestParameter[1];
Random[0] = new FormFieldRequestParameter("209457");
parameterMap.put("Random", Random);
Map map = new HashMap();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,parameterMap,"GET");
wrapperRequest.getRequestDispatcher("/aem/forms/createcorrespondence.html").include(wrapperRequest, response);
```

Zeile 1: Abrufen der Kontonummer vom Anforderungsparameter

Zeile 2-8: Erstellen Sie eine Parameterzuordnung und legen Sie geeignete Schlüssel und Werte fest, die documentId,Random widerspiegeln.

Linie 9-10: Erstellen Sie ein weiteres Zuordnungsobjekt, um den im Formulardatenmodell definierten Eingabeparameter zu speichern.

Zeile 11: Festlegen des slingRequest-Attributs &quot;paramMap&quot;

Zeile 12-13: Weiterleiten der Anfrage an das Servlet

So testen Sie diese Funktion auf Ihrem Server

* [Importieren und installieren Sie die mit diesem Artikel verknüpften Assets mit Package Manager.](assets/launch-agent-ui.zip)
* [Bei configMgr anmelden](http://localhost:4502/system/console/configMgr)
* Suchen Sie nach _Adobe Granite CSRF-Filter_
* Hinzufügen _/content/getprintchannel_ in den ausgeschlossenen Pfaden
* Speichern Sie Ihre Änderungen.
* [Öffnen Sie POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp). Stellen Sie sicher, dass die an FormFieldRequestParameter übergebene Zeichenfolge gültige documentId ist.(Zeile 19).
* [Öffnen Sie die Webseite.](http://localhost:4502/content/OpenPrintChannel.html) und geben Sie die Kontonummer ein und senden Sie das Formular ab.
* Die Benutzeroberfläche des Agenten sollte geöffnet werden, wobei die Daten vorausgefüllt sind, die spezifisch für die im Formular eingegebene Kontonummer sind.

>[!NOTE]
>
>Stellen Sie sicher, dass der Eingabeparameter des Get-Vorgangs des Formulardatenmodells an das Anforderungsattribut mit der Bezeichnung &quot;Kundennummer&quot;gebunden ist, damit dies funktioniert. Wenn Sie den Namen des Bindungswerts in einen anderen Namen ändern, stellen Sie sicher, dass die Änderung in Zeile 25 der POST.jsp übernommen wird.

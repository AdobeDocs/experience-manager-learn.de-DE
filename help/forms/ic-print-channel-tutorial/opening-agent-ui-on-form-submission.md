---
title: Benutzeroberfläche des Agenten beim Senden der POST öffnen
seo-title: Benutzeroberfläche des Agenten beim Senden der POST öffnen
description: Dies ist Teil 11 des mehrstufigen Lernprogramms zur Erstellung Ihres ersten interaktiven Kommunikations-Dokuments für den Print-Kanal. In diesem Teil starten wir die Benutzeroberfläche des Agenten-UI zum Erstellen der Ad-hoc-Korrespondenz beim Senden des Formulars.
seo-description: Dies ist Teil 11 des mehrstufigen Lernprogramms zur Erstellung Ihres ersten interaktiven Kommunikations-Dokuments für den Print-Kanal. In diesem Teil starten wir die Benutzeroberfläche des Agenten-UI zum Erstellen der Ad-hoc-Korrespondenz beim Senden des Formulars.
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
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 3%

---


# Benutzeroberfläche des Agenten beim Senden der POST öffnen

In diesem Teil starten wir die Benutzeroberfläche des Agenten-UI zum Erstellen der Ad-hoc-Korrespondenz beim Senden des Formulars.

In diesem Artikel werden Sie durch die Schritte geführt, die beim Senden eines Formulars in der Benutzeroberfläche des Eröffnungsvermittlers erforderlich sind. In der Regel müssen Kundendienstmitarbeiter ein Formular mit einigen Eingabeparametern ausfüllen und beim Öffnen des Formularübermittlungsagenten wird ui mit Daten geöffnet, die im Vorausfülldienst des Formulardatenmodells vorausgefüllt sind. Die Eingabeparameter zum Vorausfülldienst des Formulardatenmodells werden aus der Formularübermittlung extrahiert.

Das folgende Video zeigt den Verwendungsfall

>[!VIDEO](https://video.tv.adobe.com/v/40122/?quality=9&learn=on)

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

Zeile 1: Die Kontonummer aus dem Abfrageparameter abrufen

Linie 2-8: Erstellen Sie eine Parameterzuordnung und legen Sie die entsprechenden Schlüssel und Werte fest, um documentId,Random widerzuspiegeln.

Linie 9-10: Erstellen Sie ein weiteres Map-Objekt, um den im Formulardatenmodell definierten Eingabeparameter aufzunehmen.

Zeile 11: Festlegen des slingRequest-Attributs &quot;paramMap&quot;

Linie 12-13: Weiterleiten der Anforderung an das Servlet

So testen Sie diese Funktion auf Ihrem Server

* [Importieren und installieren Sie die Assets, die mit diesem Artikel in Verbindung stehen, mithilfe des Paketmanagers.](assets/launch-agent-ui.zip)
* [Bei configMgr anmelden](http://localhost:4502/system/console/configMgr)
* Suchen Sie nach _Adobe Granite CSRF Filter_
* hinzufügen _/content/getprintchannel_ in den Ausgeschlossenen Pfaden
* Speichern Sie Ihre Änderungen.
* [Öffnen Sie POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp). Vergewissern Sie sich, dass die an FormFieldRequestParameter übergebene Zeichenfolge documentId gültig ist.(Linie 19).
* [Öffnen Sie die ](http://localhost:4502/content/OpenPrintChannel.html) Webseite, geben Sie die Kontonummer ein und senden Sie das Formular.
* Die Benutzeroberfläche des Agenten sollte geöffnet werden, wobei die Daten vorab ausgefüllt werden, die spezifisch für die im Formular eingegebene Kontonummer sind.

>[!NOTE]
>
>Stellen Sie sicher, dass der Eingabeparameter des Formulardatenmodells Get (Abrufen) des Vorgangs an das Anforderungsattribut mit der Bezeichnung &quot;Kontennummer&quot;gebunden ist, damit dies funktioniert. Wenn Sie den Namen des Bindungswerts in einen anderen Namen ändern, stellen Sie sicher, dass die Änderung in Zeile 25 der POST.jsp übernommen wird.


---
title: Öffnen der Agenten-Benutzeroberfläche bei der POST-Übermittlung
description: Dies ist Teil 11 des mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments für den Druckkanal. In diesem Teil starten wir die Agenten-Benutzeroberfläche zum Erstellen von Ad-hoc-Korrespondenz bei der Formularübermittlung.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
jira: KT-6168
thumbnail: 40122.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 509b4d0d-9f3c-46cb-8ef7-07e831775086
duration: 199
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 100%

---

# Öffnen der Agenten-Benutzeroberfläche bei der POST-Übermittlung

In diesem Teil starten wir die Agenten-Benutzeroberfläche zum Erstellen von Ad-hoc-Korrespondenz bei der Formularübermittlung.

Dieser Artikel führt Sie durch die Schritte, die zum Öffnen der Agenten-Benutzeroberfläche beim Senden eines Formulars erforderlich sind. In der Regel müssen Kundendienstmitarbeitende ein Formular mit Eingabeparametern ausfüllen, woraufhin die Benutzeroberfläche für den Formularübermittlungsagenten mit Daten geöffnet wird, die vorab vom Vorfülldienst für Formulardatenmodelle ausgefüllt wurden. Die Eingabeparameter für den Vorfülldienst für Formulardatenmodelle werden aus der Formularübermittlung extrahiert.

Das folgende Video zeigt den Anwendungsfall:

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

Zeile 1: Abrufen der Kontonummer aus dem Anfrageparameter.

Zeile 2–8: Erstellen einer Parameterzuordnung und Festlegen geeigneter Schlüssel und Werte, um „documentId,Random“ widerzuspiegeln.

Zeile 9–10: Erstellen eines weiteren Zuordnungsobjekts, um den im Formulardatenmodell definierten Eingabeparameter zu speichern.

Zeile 11: Festlegen des slingRequest-Attributs „paramMap“.

Zeile 12–13: Weiterleiten der Anfrage an das Servlet.

So testen Sie diese Funktion auf Ihrem Server:

* [Importieren und installieren Sie die Assets, die sich auf diesen Artikel beziehen, mit Package Manager.](assets/launch-agent-ui.zip)
* [Melden Sie sich bei configMgr an.](http://localhost:4502/system/console/configMgr)
* Suchen Sie nach _Adobe Granite CSRF-Filter_.
* Fügen Sie _/content/getprintchannel_ zu den ausgeschlossenen Pfaden hinzu.
* Speichern Sie Ihre Änderungen.
* [Öffnen Sie POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp). Stellen Sie sicher, dass die an FormFieldRequestParameter übergebene Zeichenfolge eine gültige documentId ist.(Zeile 19).
* [Öffnen Sie die Web-Seite](http://localhost:4502/content/OpenPrintChannel.html), geben Sie die Kontonummer ein und senden Sie das Formular ab.
* Die Agenten-Benutzeroberfläche sollte sich mit vorausgefüllten Daten öffnen, die spezifisch für die im Formular eingegebene Kontonummer sind.

>[!NOTE]
>
>Stellen Sie sicher, dass der Eingabeparameter des Get-Vorgangs Ihres Formulardatenmodells an das Anfrage-Attribut „accountnumber“ gebunden ist, damit dies funktioniert. Wenn Sie den Namen des Bindungswerts in einen anderen Namen ändern, stellen Sie sicher, die Änderung in Zeile 25 der POST.jsp zu übernehmen.

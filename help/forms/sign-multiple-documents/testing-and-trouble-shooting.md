---
title: Fehlerbehebung beim Signieren mehrerer Dokumente
description: Testen Sie die Lösung und beschleunigen Sie den Vorgang.
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6960
thumbnail: 6960.jpg
topic: Entwicklung
role: Entwickler
level: Zwischenschaltung
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---


# Testen und Fehlerbehebung


## Vorschau der Refinanzierungsform

Der Anwendungsfall wird ausgelöst, wenn der Kundendienstmitarbeiter [Refinanzierungsformular](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled) ausfüllt und sendet.

Der Arbeitsablauf &quot;Mehrere Forms signieren&quot;ruft Trigger bei dieser Formularübermittlung ab und erhält eine E-Mail-Benachrichtigung mit einem Link zum Beginn des Ausfüllens und Unterschreibens des Formulars.

## Ausfüllen von Formularen im Paket

Der Kunde wird vorgeführt, um das erste Formular im Paket auszufüllen und zu unterzeichnen. Nach erfolgreicher Unterzeichnung des Formulars kann der Kunde zum nächsten Formular im Paket navigieren. Sobald alle Formulare ausgefüllt und unterzeichnet sind, wird dem Kunden das Formular &quot;**AllDone**&quot;angezeigt.

## Fehlerbehebung

### E-Mail-Benachrichtigung wird nicht generiert

Die E-Mail-Benachrichtigung wird von der Komponente &quot;E-Mail senden&quot;im Arbeitsablauf &quot;Mehrere Formulare signieren&quot;gesendet. Wenn einer der Schritte in diesem Workflow fehlschlägt, wird die E-Mail-Benachrichtigung gesendet. Stellen Sie sicher, dass beim benutzerdefinierten Prozessschritt im Workflow Zeilen in Ihrer MySQL-Datenbank erstellt werden. Wenn die Zeilen erstellt werden, überprüfen Sie Ihre Konfigurationseinstellungen für den Day CQ Mail Service

### Der Link in der E-Mail-Benachrichtigung funktioniert nicht

Die Links in den E-Mail-Benachrichtigungen werden dynamisch generiert. Wenn Ihr AEM-Server nicht auf localhost:4502 ausgeführt wird, geben Sie den korrekten Servernamen und Anschluss in den Argumenten des Schritts &quot;Forms zum Signieren speichern&quot;des Workflows &quot;Mehrere Forms signieren&quot;an

### Das Formular kann nicht signiert werden

Dies kann vorkommen, wenn das Formular nicht korrekt ausgefüllt wurde, indem die Daten aus der Datenquelle abgerufen werden. Überprüfen Sie die Stdout-Protokolle Ihres Servers. Die Datei &quot;fetchformdata.jsp&quot;schreibt einige nützliche Meldungen an den Stdout.

### Es ist nicht möglich, zum nächsten Formular im Paket zu navigieren

Beim erfolgreichen Signieren eines Formulars im Paket wird der Arbeitsablauf zum Aktualisieren des Signaturstatus ausgelöst. Der erste Schritt im Workflow aktualisiert den Signaturstatus des Formulars in der Datenbank. Bitte überprüfen Sie, ob der Status des Formulars von 0 bis 1 aktualisiert wurde.

### Das Formular &quot;AlleFertig&quot;wird nicht angezeigt

Wenn keine weiteren Formulare mehr zum Anmelden im Paket vorhanden sind, wird dem Benutzer das AllDone-Formular angezeigt.Wenn das AllDone-Formular nicht angezeigt wird, überprüfen Sie die URL, die in Zeile 33 der Datei GetNextFormToSign.js verwendet wird, die Teil der **getnextform** Client-Bibliothek ist.












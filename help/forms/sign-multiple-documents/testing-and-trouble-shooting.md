---
title: Fehlerbehebung beim Signieren mehrerer Dokumente
description: Testen und Ersetzen von Problemen
feature: Adaptive Forms
version: 6.4,6.5
kt: 6960
thumbnail: 6960.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 99cba29e-4ae3-4160-a4c7-a5b6579618c0
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 1%

---

# Testen und Fehlerbehebung


## Vorschau des Refinanzierungsformulars

Der Anwendungsfall wird ausgelöst, wenn der Kundendienstmitarbeiter Folgendes ausfüllt und sendet [Refinanzierungsformular](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled).

Der Workflow &quot;Mehrere Forms signieren&quot;erhält Trigger bei der Übermittlung des Formulars und erhält eine E-Mail-Benachrichtigung mit einem Link, um den Prozess zum Ausfüllen und Signieren des Formulars zu starten.

## Formulare im Paket ausfüllen

Der Kunde wird aufgefordert, das erste Formular im Paket auszufüllen und zu unterschreiben. Nach erfolgreicher Unterzeichnung des Formulars kann der Kunde zum nächsten Formular im Paket navigieren. Sobald alle Formulare ausgefüllt und unterzeichnet sind, wird dem Kunden der Text &quot;**AllDone**&quot;.

## Fehlerbehebung

### E-Mail-Benachrichtigung wird nicht generiert

Die E-Mail-Benachrichtigung wird von der Komponente E-Mail senden im Workflow Sign Multiple Form gesendet. Wenn einer der Schritte in diesem Workflow fehlschlägt, wird die E-Mail-Benachrichtigung gesendet. Stellen Sie sicher, dass im benutzerdefinierten Prozessschritt im Workflow Zeilen in Ihrer MySQL-Datenbank erstellt werden. Wenn die Zeilen erstellt werden, überprüfen Sie Ihre Konfigurationseinstellungen für Day CQ Mail Service .

### Der Link in der E-Mail-Benachrichtigung funktioniert nicht

Die Links in den E-Mail-Benachrichtigungen werden dynamisch generiert. Wenn Ihr AEM-Server nicht auf localhost:4502 ausgeführt wird, geben Sie in den Argumenten des Schritts &quot;Forms zum Signieren speichern&quot;des Workflows &quot;Sign Multiple Forms&quot;den richtigen Servernamen und Port an

### Formular kann nicht signiert werden

Dies kann vorkommen, wenn das Formular nicht richtig ausgefüllt wurde, indem die Daten aus der Datenquelle abgerufen wurden. Überprüfen Sie die Stdout-Protokolle Ihres Servers. Die Datei fetchformdata.jsp schreibt einige nützliche Meldungen in das stdout.

### Es ist nicht möglich, zum nächsten Formular im Paket zu navigieren

Bei erfolgreicher Unterzeichnung eines Formulars im Paket wird der Workflow Signaturstatus aktualisieren ausgelöst. Der erste Schritt im Workflow aktualisiert den Signaturstatus des Formulars in der Datenbank. Überprüfen Sie, ob der Status des Formulars von 0 auf 1 aktualisiert wurde.

### Das AllDone-Formular wird nicht angezeigt

Wenn das Paket keine weiteren Formulare enthält, wird dem Benutzer das AllDone-Formular angezeigt. Wenn das AllDone-Formular nicht angezeigt wird, überprüfen Sie die in Zeile 33 der Datei GetNextFormToSign.js verwendete URL der Datei GetNextFormToSign.js , die Teil des **getnextform** Client-Bibliothek.

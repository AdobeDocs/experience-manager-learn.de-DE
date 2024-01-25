---
title: Fehlerbehebung beim Signieren mehrerer Dokumente
description: Testen und Fehlerbehebung der Lösung
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6960
thumbnail: 6960.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 99cba29e-4ae3-4160-a4c7-a5b6579618c0
duration: 86
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 100%

---

# Testen und Fehlerbehebung


## Vorschau des Refinanzierungsformulars

Der Anwendungsfall wird ausgelöst, wenn Kundendienstmitarbeitende das [Refinanzierungsformular](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled) ausfüllen und einreichen.

Der Workflow „Mehrere Formulare unterschreiben“ wird bei der Übermittlung des Formulars ausgelöst, und die Kundin bzw. der Kunde erhält eine E-Mail-Benachrichtigung mit einem Link, um zu beginnen, das Formular auszufüllen und zu unterschreiben.

## Ausfüllen der Formulare im Paket

Die Kundin bzw. der Kunde wird aufgefordert, das erste Formular im Paket auszufüllen und zu unterschreiben. Nach erfolgreicher Unterzeichnung des Formulars kann sie bzw. er zum nächsten Formular im Paket navigieren. Sobald die Person alle Formulare ausgefüllt und unterschrieben sind, wird ihr das Formular „**AllDone**“ vorgelegt.

## Fehlerbehebung

### E-Mail-Benachrichtigung wird nicht generiert

Die E-Mail-Benachrichtigung wird von der Komponente „E-Mail senden“ im Workflow „Mehrere Formulare unterschreiben“ gesendet. Wenn einer der Schritte in diesem Workflow fehlschlägt, wird die E-Mail-Benachrichtigung gesendet. Achten Sie darauf, dass der benutzerdefinierte Prozessschritt im Workflow Zeilen in Ihrer MySQL-Datenbank erstellt.  Wenn die Zeilen erstellt werden, überprüfen Sie Ihre Konfigurationseinstellungen für Day CQ Mail Service.

### Der Link in der E-Mail-Benachrichtigung funktioniert nicht

Die Links in den E-Mail-Benachrichtigungen werden dynamisch generiert. Wenn Ihr AEM-Server nicht auf localhost:4502 ausgeführt wird, geben Sie den richtigen Server-Namen und -Port in den Argumenten des Schritts „Zu signierende Formulare speichern“ des Workflows „Mehrere Formulare unterschreiben“ an.

### Formular kann nicht signiert werden

Dies kann passieren, wenn das Formular beim Abrufen der Daten aus der Datenquelle nicht korrekt vorausgefüllt wurde.  Überprüfen Sie die Stdout-Protokolle Ihres Servers. Die Datei „fetchformdata.jsp“ schreibt einige nützliche Meldungen in das Stdout.

### Unmöglich, zum nächsten Formular im Paket zu wechseln

Bei erfolgreicher Unterzeichnung eines Formulars im Paket wird der Workflow „Signaturstatus aktualisieren“ ausgelöst. Der erste Schritt im Workflow aktualisiert den Signaturstatus des Formulars in der Datenbank. Überprüfen Sie, ob der Status des Formulars von 0 auf 1 aktualisiert wurde.

### Das AllDone-Formular wird nicht angezeigt

Wenn das Paket keine weiteren Formulare enthält, wird den Benutzenden das AllDone-Formular angezeigt. Wenn das AllDone-Formular nicht angezeigt wird, überprüfen Sie die URL, die in Zeile 33 der Datei „GetNextFormToSign.js“ verwendet wird, die Teil der Client-Bibliothek **getnextform** ist.

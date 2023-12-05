---
title: Füllen einer Tabelle eines adaptiven Formulars
description: Füllen einer Tabelle eines adaptiven Formulars mit den Ergebnissen aus Formulardatenmodell-Dienstaufrufen
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
exl-id: 6e4b901a-6534-4c34-b315-2f2620b74247
last-substantial-update: 2019-06-09T00:00:00Z
duration: 64
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 100%

---

# Füllen einer Tabelle eines adaptiven Formulars mit den Ergebnissen aus einem Formulardatenmodell-Dienstaufruf

[Das Live-Formular wird hier gehostet](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
In diesem Artikel sehen wir uns das Füllen der Tabelle für adaptive Formulare an, indem wir Daten aus dem Aufruf des Formulardatenmodelldienstes abrufen. Wir werden einen Tilgungsplan in einer Tabelle erstellen, in der jede regelmäßige Zahlung für eine Hypothek im Laufe der Zeit aufgelistet wird. Die Tilgungsergebnisse werden von unserem Formulardatenmodell zurückgegeben. Der Dienst des Formulardatenmodells wird beim Klicken auf die Schaltfläche „calculate“ aufgerufen, wie im Screenshot zu sehen. Die Eingabe- und Ausgabeparameter des Dienstaufrufs werden entsprechend zugeordnet, wie im Screenshot zu sehen. Die Ausgabe wird den Spalten von Row1 zugeordnet
![clickevent](assets/amortization.PNG)

Row1 ist so konfiguriert, dass sie entsprechend den vom Dienstaufruf zurückgegebenen Daten wächst. Beachten Sie die hier angegebenen Wiederholungseinstellungen. Der Wert „-1“ bedeutet eine unbegrenzte Anzahl von Zeilen in der Tabelle
![Row1](assets/rowconfiguration.PNG)

## Stellen Sie dies auf Ihrem Server bereit

[Installieren Sie Tomcat wie hier angegeben](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[Installieren Sie die Datei SampleRest.war, die in dieser Zip-Datei enthalten ist, in Ihrem Tomcat](assets/sample-rest.zip)
[Installieren Sie die Assets](assets/amortizationschedule.zip) mithilfe des AEM Package Managers
[Öffnen Sie das Tilgungsplan-Formular](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
Geben Sie den entsprechenden Wert ein und klicken Sie auf „Berechnen“
Der Tilgungsplan sollte in Ihrem Formular ausgefüllt werden.

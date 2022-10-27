---
title: Tabelle "Adaptives Formular"ausfüllen
description: Füllen Sie die Tabelle "Adaptives Formular"mit den Ergebnissen aus Formulardatenmodell-Dienstaufrufen
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
exl-id: 6e4b901a-6534-4c34-b315-2f2620b74247
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 0%

---

# Füllen der Tabelle &quot;Adaptives Formular&quot;mit den Ergebnissen des Formulardatenmodell-Dienstaufrufs

[Das Live-Formular wird hier gehostet](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
In diesem Artikel sehen wir uns das Ausfüllen der Tabelle für adaptive Formulare an, indem wir Daten aus dem Aufruf des Formulardatenmodelldienstes abrufen. Wir werden einen Tilgungsplan in einer Tabelle erstellen, in der jede regelmäßige Zahlung einer Hypothek im Zeitverlauf aufgeführt wird. Die Amortisierungsergebnisse werden von unserem Formulardatenmodell zurückgegeben. Der Dienst des Formulardatenmodells wird beim Klicken auf die Schaltfläche &quot;calculate&quot;aufgerufen, wie im Screenshot gezeigt. Die Eingabe- und Ausgabeparameter des Dienstaufrufs werden entsprechend zugeordnet, wie im Screenshot gezeigt. Die Ausgabe wird den Spalten von Zeile1 zugeordnet
![clickevent](assets/amortization.PNG)

Zeile1 ist so konfiguriert, dass sie entsprechend den vom Dienstaufruf zurückgegebenen Daten wächst. Beachten Sie die hier angegebenen Wiederholungseinstellungen. Der Wert -1 zeigt eine unbegrenzte Anzahl von Zeilen in der Tabelle an
![Zeile1](assets/rowconfiguration.PNG)

## Bereitstellen auf Ihrem Server

[Installieren Sie Tomcat wie hier angegeben](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[Stellen Sie die in dieser ZIP-Datei enthaltene Datei SampleRest.war in Ihrem Tomcat bereit.](assets/sample-rest.zip)
[Installieren der Assets ](assets/amortizationschedule.zip) Verwenden AEM Package Manager
[Öffnen Sie das Formular &quot;Amortisierungszeitplan&quot;](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
Geben Sie den entsprechenden Wert ein und klicken Sie auf Automatisierungsplanung berechnen , um das Formular auszufüllen.

---
title: 'Tabelle "Adaptives Formular"ausfüllen '
seo-title: Tabelle "Adaptives Formular"ausfüllen
description: Füllen Sie die Tabelle "Adaptives Formular"mit den Ergebnissen aus Formulardatenmodell-Dienstaufrufen
seo-description: Füllen Sie die Tabelle "Adaptives Formular"mit den Ergebnissen aus Formulardatenmodell-Dienstaufrufen
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Entwicklung
role: User
level: Intermediate
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 1%

---


# Füllen der Tabelle &quot;Adaptives Formular&quot;mit den Ergebnissen des Formulardatenmodell-Dienstaufrufs

[Live-Formular wird ](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
hier gehostetIn diesem Artikel sehen wir uns das Ausfüllen der Tabelle für adaptive Formulare an, indem wir Daten aus dem Aufruf des Formulardatenmodelldienstes abrufen. Wir werden einen Tilgungsplan in einer Tabelle erstellen, in der jede regelmäßige Zahlung einer Hypothek im Zeitverlauf aufgeführt wird. Die Amortisierungsergebnisse werden von unserem Formulardatenmodell zurückgegeben. Der Dienst des Formulardatenmodells wird beim Klicken auf die Schaltfläche &quot;calculate&quot;aufgerufen, wie im Screenshot gezeigt. Die Eingabe- und Ausgabeparameter des Dienstaufrufs werden entsprechend zugeordnet, wie im Screenshot gezeigt. Die Ausgabe wird den Spalten von Zeile1 zugeordnet
![clickevent](assets/amortization.PNG)

Zeile1 ist so konfiguriert, dass sie entsprechend den vom Dienstaufruf zurückgegebenen Daten wächst. Beachten Sie die hier angegebenen Wiederholungseinstellungen. Der Wert -1 zeigt eine unbegrenzte Anzahl von Zeilen in der Tabelle an
![Zeile1](assets/rowconfiguration.PNG)

## Bereitstellen auf Ihrem Server

[Installieren Sie Tomcat wie ](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[hier angegebenBereitstellen der ](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
[Datei SampleRest.warInstallieren Sie die Assets  ](assets/amortizationschedule.zip) mit AEM Paketmanager 
[Öffnen Sie das ](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
Formular für den AmortisierungszeitplanGeben Sie den entsprechenden Wert ein und klicken Sie auf &quot;Automatisierungsplanung berechnen&quot;, um in Ihrem Formular auszufüllen.


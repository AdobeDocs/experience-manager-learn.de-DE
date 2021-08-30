---
title: 'Tabelle "Adaptives Formular"ausfüllen '
description: Füllen Sie die Tabelle "Adaptives Formular"mit den Ergebnissen aus Formulardatenmodell-Dienstaufrufen
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
source-git-commit: 2b7f0f6c34803672cc57425811db89146b38a70a
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 0%

---


# Füllen der Tabelle &quot;Adaptives Formular&quot;mit den Ergebnissen des Formulardatenmodell-Dienstaufrufs

[Live-Formular wird ](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
hier gehostetIn diesem Artikel sehen wir uns das Ausfüllen der Tabelle für adaptive Formulare an, indem wir Daten aus dem Aufruf des Formulardatenmodelldienstes abrufen. Wir werden einen Tilgungsplan in einer Tabelle erstellen, in der jede regelmäßige Zahlung einer Hypothek im Zeitverlauf aufgeführt wird. Die Amortisierungsergebnisse werden von unserem Formulardatenmodell zurückgegeben. Der Dienst des Formulardatenmodells wird beim Klicken auf die Schaltfläche &quot;calculate&quot;aufgerufen, wie im Screenshot gezeigt. Die Eingabe- und Ausgabeparameter des Dienstaufrufs werden entsprechend zugeordnet, wie im Screenshot gezeigt. Die Ausgabe wird den Spalten von Zeile1 zugeordnet
![clickevent](assets/amortization.PNG)

Zeile1 ist so konfiguriert, dass sie entsprechend den vom Dienstaufruf zurückgegebenen Daten wächst. Beachten Sie die hier angegebenen Wiederholungseinstellungen. Der Wert -1 zeigt eine unbegrenzte Anzahl von Zeilen in der Tabelle an
![Zeile1](assets/rowconfiguration.PNG)

## Bereitstellen auf Ihrem Server

[Installieren Sie Tomcat wie ](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[hier angegebenStellen Sie die Datei SampleRest.war in dieser ZIP-Datei in Ihrer ](assets/sample-rest.zip)
[TomcatInstallieren Sie die Assets  ](assets/amortizationschedule.zip) mithilfe AEM Paketmanagers 
[Öffnen Sie das Amortisierungszeitschema-](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
Formular. Geben Sie den entsprechenden Wert ein und klicken Sie auf Automatisierungszeitplan berechnen , um in Ihrem Formular auszufüllen.


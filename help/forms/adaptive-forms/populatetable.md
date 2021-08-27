---
title: 'Tabelle "Adaptives Formular"ausfüllen '
description: Füllen Sie die Tabelle "Adaptives Formular"mit den Ergebnissen aus Formulardatenmodell-Dienstaufrufen
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
source-git-commit: 0049c9fd864bd4dd4f8c33b1e40e94aad3ffc5b9
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---


# Füllen der Tabelle &quot;Adaptives Formular&quot;mit den Ergebnissen des Formulardatenmodell-Dienstaufrufs

[Live-Formular wird ](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
hier gehostetIn diesem Artikel sehen wir uns das Ausfüllen der Tabelle für adaptive Formulare an, indem wir Daten aus dem Aufruf des Formulardatenmodelldienstes abrufen. Wir werden einen Tilgungsplan in einer Tabelle erstellen, in der jede regelmäßige Zahlung einer Hypothek im Zeitverlauf aufgeführt wird. Die Amortisierungsergebnisse werden von unserem Formulardatenmodell zurückgegeben. Der Dienst des Formulardatenmodells wird beim Klicken auf die Schaltfläche &quot;calculate&quot;aufgerufen, wie im Screenshot gezeigt. Die Eingabe- und Ausgabeparameter des Dienstaufrufs werden entsprechend zugeordnet, wie im Screenshot gezeigt. Die Ausgabe wird den Spalten von Zeile1 zugeordnet
![clickevent](assets/amortization.PNG)

Zeile1 ist so konfiguriert, dass sie entsprechend den vom Dienstaufruf zurückgegebenen Daten wächst. Beachten Sie die hier angegebenen Wiederholungseinstellungen. Der Wert -1 zeigt eine unbegrenzte Anzahl von Zeilen in der Tabelle an
![Zeile1](assets/rowconfiguration.PNG)

## Bereitstellen auf Ihrem Server

[Tomcat wie ](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[hier angegeben installierenBereitstellen der Datei SampleRest.war in dieser ZIP-](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/assets/common-osgi-bundles/sample-rest.zip)
[Datei enthalten.Installieren Sie die Assets  ](assets/amortizationschedule.zip) mit AEM Paketmanager 
[Öffnen Sie das ](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
Formular für den Amortisierungszeitplan Geben Sie den entsprechenden Wert ein und klicken Sie auf &quot;Amortisierungszeitplan berechnen&quot;, um in Ihrem Formular auszufüllen.


---
title: 'Tabelle für adaptives Formular ausfüllen '
seo-title: Tabelle für adaptives Formular ausfüllen
description: Füllen Sie die Tabelle "Adaptives Formular"mit den Ergebnissen von Formulardatenmodell-Dienstaufrufen
seo-description: Füllen Sie die Tabelle "Adaptives Formular"mit den Ergebnissen von Formulardatenmodell-Dienstaufrufen
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 0%

---


# Tabelle für adaptive Formulare mit den Ergebnissen des Formulardatenmodelldienstaufrufs füllen

[Live-Formular wird ](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
hier gehostetIn diesem Artikel betrachten wir das Ausfüllen der Tabelle für adaptive Formulare, indem wir Daten aus dem Formulardatenmodelldienst-Aufruf abrufen. Wir werden einen Tilgungsplan in einer Tabelle erstellen, in der jede reguläre Zahlung mit der Zeit auf eine Hypothek Liste wird. Die Abschreibungsergebnisse werden von unserem Formulardatenmodell zurückgegeben. Der Dienst des Formulardatenmodells wird auf dem click-Ereignis der Berechnungsschaltfläche aufgerufen, wie im Screenshot dargestellt. Die Eingabe- und Ausgabeparameter des Dienstaufrufs werden entsprechend zugeordnet, wie im Screenshot dargestellt. Die Ausgabe wird den Spalten von Zeile1 zugeordnet
![clickEvent](assets/amortization.PNG)

Zeile1 ist so konfiguriert, dass sie je nach den vom Dienstaufruf zurückgegebenen Daten wächst. Beachten Sie die hier angegebenen Wiederholungseinstellungen. Der Wert -1 gibt eine unbegrenzte Anzahl Zeilen in der Tabelle an.
![Zeile1](assets/rowconfiguration.PNG)

## Bereitstellen auf dem Server

[Installieren Sie Tomcat wie ](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[hier angegebenBereitstellen der ](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
[Datei SampleRest.warInstallieren Sie die Assets  ](assets/amortizationschedule.zip) mit AEM Package Manager 
[Öffnen Sie das ](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
Formular für den Amortisierungszeitplan Geben Sie den entsprechenden Wert ein und klicken Sie auf &quot;Amortisierungszeitplan berechnen&quot;, um das Formular auszufüllen.


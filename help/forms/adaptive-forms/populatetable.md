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
source-git-commit: 4f51f7bf00827210d2631b9335768a9980f6655c
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 0%

---


# Tabelle für adaptive Formulare mit den Ergebnissen des Formulardatenmodelldienstaufrufs füllen

[Das Live-Formular wird hier](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)gehostet. In diesem Artikel betrachten wir das Ausfüllen der Tabelle für adaptive Formulare, indem wir Daten aus dem Formulardatenmodelldienst-Aufruf abrufen. Wir werden einen Tilgungsplan in einer Tabelle erstellen, in der jede reguläre Zahlung mit der Zeit auf eine Hypothek Liste wird. Die Abschreibungsergebnisse werden von unserem Formulardatenmodell zurückgegeben. Der Dienst des Formulardatenmodells wird auf dem click-Ereignis der Berechnungsschaltfläche aufgerufen, wie im Screenshot dargestellt. Die Eingabe- und Ausgabeparameter des Dienstaufrufs werden entsprechend zugeordnet, wie im Screenshot dargestellt. Die Ausgabe wird den Spalten von Row1![clickevent zugeordnet](assets/amortization.PNG)

Zeile1 ist so konfiguriert, dass sie je nach den vom Dienstaufruf zurückgegebenen Daten wächst. Beachten Sie die hier angegebenen Wiederholungseinstellungen. Der Wert -1 bedeutet eine unbegrenzte Anzahl Zeilen in der Tabelle![Zeile1](assets/rowconfiguration.PNG)

## Bereitstellen auf dem Server

[Installieren Sie Tomcat wie hier](/help/forms/ic-print-channel-tutorial/partone.md)[beschriebenBereitstellen der Datei](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)SampleRest.warArchivieren Sie die Assets[mit AEM Package Manager ](assets/amortizationschedule.zip) Öffnen Sie das Formular[](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)Amortisierungszeitplan Geben Sie den entsprechenden Wert ein und klicken Sie auf calculateAmortizationsplan sollte in Ihrem Formular ausgefüllt werden


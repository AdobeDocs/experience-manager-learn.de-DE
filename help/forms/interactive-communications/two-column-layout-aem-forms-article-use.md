---
title: Erstellen von zweispaltigen Layouts für Print Kanal-Dokumente
seo-title: Erstellen von zweispaltigen Layouts für Print Kanal-Dokumente
description: Erstellen von 2 Spalten-Layouts für Print Kanal Dokument
seo-description: Erstellen von 2 Spalten-Layouts für Print Kanal Dokument
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---


# Zwei Spaltenlayouts im Dokument &quot;Kanal drucken&quot;

In diesem kurzen Artikel werden die Schritte erläutert, die zum Erstellen eines zweispaltigen Layouts in Print Kanal erforderlich sind. Der Anwendungsfall besteht darin, 2 Dokumente mit Seite 1 mit 2 Spaltenlayout und Seite 2 mit dem standardmäßigen 1 Spaltenlayout zu generieren.

Im Folgenden werden die Schritte auf hoher Ebene beschrieben, die beim Erstellen von 2-Spalten-Layouts mit AEM Forms Designer durchgeführt werden müssen.

* Erstellen Sie zwei Inhaltsbereiche auf Seite 1 Übergeordnet
* Benennen Sie die beiden Inhaltsbereiche &quot;linke Spalte&quot;und &quot;rechte Spalte&quot;
* Zweite Übergeordnet-Seite mit einem Inhaltsbereich erstellen (dies ist die Standardseite)
* Wählen Sie die Registerkarte &quot;Paginierung&quot;(unbenannt - Teilformular) (Seite 1) und (unbenannt - Teilformular) (Seite 2) und legen Sie die Eigenschaften wie in den Screenshots unten dargestellt fest.

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

Sobald die Paginierungseigenschaften festgelegt sind, können wir Teilformulare oder Zielgruppen unter (unbenanntes Teilformular) (Seite 1) hinzufügen.

Anschließend können wir Dokument-Fragmente zu diesen Teilformularen oder Zielgruppen hinzufügen. Wenn die linke Spalte voll ist, fließt der Inhalt in die rechte Spalte.

Um dies auf Ihrem lokalen Server zu testen, laden Sie die Assets herunter, die mit diesem Artikel in Verbindung stehen. Bildlauf nach unten bis zum Ende dieser Seite

* [Herunterladen und Installieren des Beispieldrucken-Kanal-Dokuments mithilfe des Paketmanagers](assets/print-channel-with-two-column-layout.zip)
* [Vorschau des Print Kanal-Dokuments](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)

---
title: Erstellen von zwei Spaltenlayouts für Druckkanaldokumente
seo-title: Erstellen von zwei Spaltenlayouts für Druckkanaldokumente
description: Erstellen von 2 Spaltenlayouts für das Dokument "Druckkanal"
seo-description: Erstellen von 2 Spaltenlayouts für das Dokument "Druckkanal"
feature: Interaktive Kommunikation
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Entwicklung
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 2%

---


# Zwei Spaltenlayouts im Dokument &quot;Druckkanal&quot;

In diesem kurzen Artikel werden die Schritte erläutert, die zum Erstellen eines zweispaltigen Layouts im Druckkanal erforderlich sind. Der Anwendungsfall besteht darin, zwei Seitendokumente mit Seite 1 mit 2 Spaltenlayout und Seite 2 mit dem standardmäßigen Spaltenlayout 1 zu erstellen.

Im Folgenden werden die allgemeinen Schritte zum Erstellen von zweispaltigen Layouts mit AEM Forms Designer beschrieben.

* Erstellen von zwei Inhaltsbereichen auf Seite 1 Übergeordnete Seite
* Benennen Sie die beiden Inhaltsbereiche &quot;linke Spalte&quot;und &quot;rechte Spalte&quot;.
* Zweite Übergeordnete Seite mit einem Inhaltsbereich erstellen (dies ist die Standardeinstellung)
* Wählen Sie die Registerkarte &quot;Paginierung&quot;(unbenanntes Teilformular) (Seite 1) und (unbenanntes Teilformular) (Seite 2) und legen Sie die Eigenschaften wie in den Screenshots unten dargestellt fest.

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

Sobald die Paginierungseigenschaften festgelegt sind, können wir Teilformulare oder Zielbereiche unter (unbenanntes Teilformular) (Seite 1) hinzufügen.

Anschließend können wir diesen Teilformularen oder Zielbereichen Dokumentfragmente hinzufügen. Wenn die linke Spalte voll ist, fließt der Inhalt in die rechte Spalte.

Um dies auf Ihrem lokalen Server zu testen, laden Sie die Assets herunter, die mit diesem Artikel in Verbindung stehen. Scrollen Sie nach unten zum Ende dieser Seite

* [Laden Sie das Beispiel-Druckkanaldokument mit Package Manager herunter und installieren Sie es.](assets/print-channel-with-two-column-layout.zip)
* [Vorschau des Druckkanaldokuments anzeigen](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)

---
title: Konfigurieren des Bedienfelds für Rentenausblicke
description: Dies ist Teil 10 eines mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments. In diesem Teil konfigurieren wir das Bedienfeld für Rentenausblicke, indem wir Text- und Diagrammkomponenten hinzufügen.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
topic: Development
role: Developer
level: Beginner
exl-id: 0dd8a430-9a4e-4dc7-ad75-6ad2490430f2
duration: 100
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '347'
ht-degree: 100%

---

# Konfigurieren des Bedienfelds für Rentenausblicke{#configuring-retirement-outlook-panel}

* Dies ist Teil 10 eines mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments. In diesem Teil konfigurieren wir das Bedienfeld für Rentenausblicke, indem wir Text- und Diagrammkomponenten hinzufügen.

* Melden Sie sich bei AEM Forms an und navigieren Sie zu „Adobe Experience Manager“ > „Formulare“ > „Formulare und Dokumente“.

* Öffnen Sie den Ordner „401KStatement“.

* Öffnen Sie das Dokument „401KStatement“ im Bearbeitungsmodus.

**Konfigurieren des Zielbereich des linken Bedienfelds**

* Klicken Sie rechts auf den Zielbereich des linken Bedienfelds und dann auf das Pluszeichen, um das Dialogfeld „Komponente einfügen“ aufzurufen.

* Fügen Sie eine Textkomponente ein.

* Tippen Sie behutsam auf die neu hinzugefügte Textkomponente, um die Komponentensymbolleiste aufzurufen

* Wählen Sie das Stiftsymbol, um den Standardtext zu bearbeiten.

* Ersetzen Sie den Standardtext durch „**Ihr Renteneinkommensausblick“**

**Konfigurieren des Zielbereichs des rechten Bedienfelds**

* Klicken Sie auf der rechten Seite auf den Zielbereich des rechten Bedienfelds und danach auf das Pluszeichen, um das Dialogfeld „Komponente einfügen“ aufzurufen.

* Fügen Sie eine Textkomponente ein.

* Tippen Sie behutsam auf die neu hinzugefügte Textkomponente, um die Komponentensymbolleiste aufzurufen.

* Wählen Sie das Stiftsymbol, um den Standardtext zu bearbeiten.

* Ersetzen Sie den Standardtext durch „**Geschätztes monatliches Renteneinkommen**“

## Hinzufügen des Dokumentfragments für den Renteneinkommensausblick {#add-retirement-income-outlook-document-fragment}

* Klicken Sie auf das Assets-Symbol und wenden Sie den Filter an, um Assets vom Typ „Dokumentfragmente“ anzuzeigen. Ziehen Sie das Dokumentfragment „RetirementIncomeOutlook“ in den Zielbereich des linken Bedienfelds.

* Weitere Informationen zum Hinzufügen von Dokumentfragmenten zu Inhaltsbereichen finden Sie [unter dieser Seite](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/partseven.html?lang=de).

## Hinzufügen des Diagramms des geschätzten monatlichen Einkommens {#adding-estimated-monthly-income-chart}

* Klicken Sie auf den Zielbereich des rechten Bedienfelds auf der rechten Seite. Klicken Sie auf das Pluszeichen, um eine Diagrammkomponente einzufügen. Wir werden ein Säulendiagramm verwenden, um das geschätzte monatliche Einkommen anzuzeigen. Tippen Sie behutsam auf die neu eingefügte Diagrammkomponente. Wählen Sie das Schraubenschlüssel-Symbol, um das Konfigurationsdatenblatt zu öffnen. Konfigurieren Sie das Diagramm mit den folgenden Eigenschaften, wie im Screenshot unten dargestellt.

**AEM Forms 6.4: Konfiguration des Säulendiagramms für das geschätzte monatliche Einkommen**

![form64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5: Konfiguration des Säulendiagramms für das geschätzte monatliche Einkommen**

![forms65](assets/estimatedmonthlyincomechart65.PNG)

## Nächste Schritte

[Konfiguration eines Tortendiagramms](./parteleven.md)

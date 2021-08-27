---
title: Konfigurieren des Outlook-Bedienfelds "Retirement"
seo-title: Configuring Retirement Outlook Panel
description: Dies ist Teil 10 eines mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments. In diesem Teil konfigurieren wir das Outlook-Bedienfeld für die Einstellung, indem wir Text- und Diagrammkomponenten hinzufügen.
seo-description: This is part 10 of a multi-step tutorial for creating your first interactive communications document. In this part, we will configure Retirement Outlook Panel by adding text and chart components.
uuid: 1d5119b5-e797-4bf0-9b10-995b3f051f92
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
topic: Development
role: Developer
level: Beginner
source-git-commit: 0049c9fd864bd4dd4f8c33b1e40e94aad3ffc5b9
workflow-type: tm+mt
source-wordcount: '344'
ht-degree: 0%

---


# Konfigurieren des Outlook-Bedienfelds &quot;Retirement&quot;{#configuring-retirement-outlook-panel}

* Dies ist Teil 10 eines mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments. In diesem Teil konfigurieren wir das Outlook-Bedienfeld für die Einstellung, indem wir Text- und Diagrammkomponenten hinzufügen.

* Melden Sie sich bei AEM Forms an und navigieren Sie zu Adobe Experience Manager > Forms > Forms &amp; Documents.

* Öffnen Sie den Ordner 401KStatement .

* Öffnen Sie das Dokument 401KStatement im Bearbeitungsmodus.

**Konfigurieren des LeftPanel-Zielbereichs**

* Tippen Sie rechts auf den Zielbereich LeftPanel und klicken Sie auf das Symbol &quot;+&quot;, um das Dialogfeld &quot;Komponente einfügen&quot;aufzurufen.

* Fügen Sie die Textkomponente ein.

* Tippen Sie behutsam auf die neu hinzugefügte Textkomponente, um die Komponentensymbolleiste aufzurufen.

* Wählen Sie das Symbol &quot;Stift&quot;, um den Standardtext zu bearbeiten.

* Ersetzen Sie den Standardtext durch &quot;**Your Retirement Income Outlook&quot;**

**RightsPanel-Zielbereich konfigurieren**

* Tippen Sie auf der rechten Seite auf den Zielbereich RightPanel und klicken Sie auf das Symbol &quot;+&quot;, um das Dialogfeld &quot;Komponente einfügen&quot;aufzurufen.

* Fügen Sie die Textkomponente ein.

* Tippen Sie behutsam auf die neu hinzugefügte Textkomponente, um die Komponenten-Symbolleiste aufzurufen.

* Wählen Sie das Symbol &quot;Stift&quot;, um den Standardtext zu bearbeiten.

* Ersetzen Sie den Standardtext durch &quot;**Geschätztes monatliches Renteneinkommen&quot;**

## Hinzufügen des Einkommens aus Outlook-Dokumentfragments für die Bereitstellung {#add-retirement-income-outlook-document-fragment}

* Klicken Sie auf das Symbol Assets und wenden Sie den Filter an, um Assets vom Typ &quot;Dokumentfragmente&quot;anzuzeigen. Ziehen Sie das Dokumentfragment RetirementIncomeOutlook in den Zielbereich des linken Bedienfelds.

* Sie können [auf diese Seite](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/partseven.html) beim Hinzufügen von Dokumentfragmenten zu Inhaltsbereichen verweisen.

## Geschätzte monatliche Einkommensdiagramme hinzufügen {#adding-estimated-monthly-income-chart}

* Klicken Sie auf der rechten Seite auf den Zielbereich RightPanel . Klicken Sie auf das Symbol &quot;+&quot;, um die Diagrammkomponente einzufügen. Wir werden eine Spaltenübersicht verwenden, um das geschätzte monatliche Einkommen anzuzeigen. Tippen Sie behutsam auf die neu eingefügte Diagrammkomponente. Wählen Sie das Symbol &quot;Schraubenschlüssel&quot;, um das Konfigurationsdatenblatt zu öffnen. Konfigurieren Sie das Diagramm mit den folgenden Eigenschaften, wie im Screenshot unten dargestellt.

**AEM Forms 6.4 - Spaltendiagramm für das geschätzte monatliche Einkommen konfigurieren**

![form64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5 - Spaltendiagramm für das geschätzte monatliche Einkommen konfigurieren**

![forms65](assets/estimatedmonthlyincomechart65.PNG)





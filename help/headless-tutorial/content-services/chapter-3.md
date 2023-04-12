---
title: Kapitel 3 – Erstellen von Inhaltsfragmenten für Ereignisse – Content Services
seo-title: Getting Started with AEM Content Services - Chapter 3 - Authoring Event Content Fragments
description: Kapitel 3 des AEM Headless-Tutorials behandelt das Erstellen und Verfassen von Ereignis-Inhaltsfragmenten anhand des in Kapitel 2 erstellten Inhaltsfragmentmodells.
seo-description: Chapter 3 of the AEM Headless tutorial covers creating and authoring Event Content Fragments from the Content Fragment Model created in Chapter 2.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 100%

---

# Kapitel 3 – Erstellen von Inhaltsfragmenten für Ereignisse

Kapitel 3 des AEM Headless-Tutorials behandelt das Erstellen und Verfassen von Ereignis-Inhaltsfragmenten anhand des in [Kapitel 2](./chapter-2.md) erstellten Inhaltsfragmentmodells.

## Erstellen eines Ereignis-Inhaltsfragments

Nachdem ein [!DNL Event]-Inhaltsfragmentmodell erstellt und die AEM-Konfiguration für WKND auf den Asset-Ordner `/content/dam/wknd-mobile` (über die Eigenschaft `cq:conf`) angewendet wurde, kann ein [!DNL Event]-Inhaltsfragment erstellt werden.

Inhaltsfragmente sind eine Art von Asset und sollten in AEM Assets genauso organisiert und verwaltet werden wie andere Assets.

* Verwenden Sie in der Assets-Ordnerstruktur Ordner für Gebietsschemata, wenn eine Übersetzung erforderlich ist (oder sein könnte).
* Organisieren Sie Inhaltsfragmente logisch, damit sie leicht zu finden und zu verwalten sind.

In diesem Schritt erstellen wir im Assets-Ordner `/content/dam/wknd-mobile/en/events` ein neues [!DNL Event] für `Punkrock Fest`.

1. Navigieren Sie zu **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Dateien] > [!DNL WKND Mobile] >[!DNL English]** und erstellen Sie einen Asset-Ordner **[!DNL Events]**.
1. Erstellen Sie in **[!UICONTROL Assets] > [!UICONTROL Dateien] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** ein neues Inhaltsfragment vom Typ **[!DNL Event]** mit dem Titel **[!DNL Punkrock Fest]**.
1. Machen Sie für das neu erstellte [!DNL Event]-Inhaltsfragment die nötigen Angaben.

   * [!DNL Event Title]: **[!DNL Punkrock Fest]**
   * [!DNL Event Description]: **&lt;Geben Sie ein paar Zeilen mit einer Beschreibung ein...>**
   * [!DNL Event Date] : **&lt;Select a date in the future>**
   * [!DNL Event Type]: **Musik**
   * [!DNL Ticket Price]: **10**
   * [!DNL Event Image]: **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name]: **The Reptile House**
   * [!DNL Venue City]: **New York**

   Tippen Sie auf **[!UICONTROL Speichern]** in der oberen Aktionsleiste, um die Änderungen zu speichern.

1. Installieren Sie mit [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp) das folgende Paket auf AEM Author. Dieses Paket enthält eine Reihe von Ereignisinhaltsfragmenten.

   [Datei abrufen: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## Überprüfen der JCR-Struktur des Inhaltsfragments

*Dieser Abschnitt dient nur zu Informationszwecken und soll die zugrundeliegende JCR-Struktur von Inhaltsfragmenten, die aus Inhaltsfragmentmodellen erstellt wurden, verdeutlichen.*

1. Öffnen Sie **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** in AEM Author.
1. Navigieren Sie in CRXDE Lite im linken Hierarchie-Menü zu [/content/dam/wknd-mobile/de/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content), dem Knoten, der das [!DNL Event]-Inhaltsfragment [!DNL Punkrock Fest] im JCR darstellt.
1. Erweitern Sie den Knoten [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master).
Überprüfen Sie im Bereich **Properties**, ob die Eigenschaft `cq:model` vorhanden ist, die auf die Definition des Inhaltsfragmentmodells [!DNL Event] verweist.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Wählen Sie unter dem Knoten `data` den Knoten [master](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) und überprüfen Sie die Eigenschaften. Dieser Knoten enthält den Inhalt, der bei der Erstellung eines [!DNL Event]-Inhaltsfragmentmodells gesammelt wurde. Die JCR-Eigenschaftsnamen entsprechen den Eigenschaftsnamen des Inhaltsfragmentmodells, und die Werte entsprechen den autorisierten Werten des [!DNL Event]-Inhaltsfragments „[!DNL Punkrock Fest]“.

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## Nächster Schritt

Es wird empfohlen, das Inhaltspaket [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) auf AEM Author über [AEM [!UICONTROL Package Manager]](http://localhost:4502/crx/packmgr/index.jsp) zu installieren. Dieses Paket enthält die in diesem und den vorangegangenen Kapiteln des Tutorials beschriebenen Konfigurationen und Inhalte.

* [Kapitel 4 – Definieren von AEM Content Services-Vorlagen](./chapter-4.md)

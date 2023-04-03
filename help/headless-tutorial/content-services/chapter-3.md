---
title: Kapitel 3 - Erstellen von Inhaltsfragmenten für Ereignisse - Content Services
seo-title: Getting Started with AEM Content Services - Chapter 3 - Authoring Event Content Fragments
description: In Kapitel 3 des Tutorials AEM Headless wird das Erstellen und Bearbeiten von Ereignis-Inhaltsfragmenten aus dem in Kapitel 2 erstellten Inhaltsfragmentmodell beschrieben.
seo-description: Chapter 3 of the AEM Headless tutorial covers creating and authoring Event Content Fragments from the Content Fragment Model created in Chapter 2.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 4%

---

# Kapitel 3 - Erstellen von Inhaltsfragmenten für Ereignisse

Kapitel 3 des Tutorials AEM Headless umfasst das Erstellen und Bearbeiten von Ereignisinhaltsfragmenten aus dem Inhaltsfragmentmodell, das in [Kapitel 2](./chapter-2.md).

## Erstellen eines Ereignisinhaltsfragments

Mit [!DNL Event] Inhaltsfragmentmodell erstellt und die AEM Konfiguration für WKND auf die `/content/dam/wknd-mobile` Asset-Ordner (über `cq:conf` -Eigenschaft), a [!DNL Event] Inhaltsfragment kann erstellt werden.

Inhaltsfragmente, bei denen es sich um einen Asset-Typ handelt, sollten in AEM Assets wie andere Assets organisiert und verwaltet werden.

* Verwenden Sie Gebietsschemaordner in der Asset-Ordnerstruktur, wenn eine Übersetzung erforderlich ist (oder sein kann)
* Inhaltsfragmente logisch organisieren, damit sie leicht zu finden und zu verwalten sind

Erstellen Sie in diesem Schritt eine neue [!DNL Event] für `Punkrock Fest` im `/content/dam/wknd-mobile/en/events` Asset-Ordner.

1. Navigieren Sie zu **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Dateien] > [!DNL WKND Mobile] >[!DNL English]** und Erstellen von Asset-Ordnern **[!DNL Events]**.
1. Within **[!UICONTROL Assets] > [!UICONTROL Dateien] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** Erstellen Sie ein neues Inhaltsfragment vom Typ **[!DNL Event]** mit dem Titel **[!DNL Punkrock Fest]**.
1. Erstellen Sie die neu erstellte [!DNL Event] Inhaltsfragment.

   * [!DNL Event Title]: **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description...=&quot;&quot;>**
   * [!DNL Event Date] : **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] : **Musik**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **Das Reptilienhaus**
   * [!DNL Venue City] : **New York**

   Tippen **[!UICONTROL Speichern]** in der oberen Aktionsleiste, um Änderungen zu speichern.

1. Verwenden [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)installieren Sie das unten stehende Paket auf der AEM-Autoreninstanz. Dieses Paket enthält eine Reihe von Event-Inhaltsfragmenten.

   [Datei abrufen: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## Überprüfen der JCR-Struktur des Inhaltsfragments

*Dieser Abschnitt ist nur informativ und dient zum Verknüpfen der zugrunde liegenden JCR-Struktur von Inhaltsfragmenten, die aus Inhaltsfragmentmodellen erstellt wurden.*

1. Öffnen **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** in der AEM-Autoreninstanz.
1. Navigieren Sie in CRXDE Lite im linken Hierarchiemenü zu [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) , der den Knoten darstellt, der [!DNL Punkrock Fest] [!DNL Event] Inhaltsfragment im JCR.
1. Erweitern Sie die [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) Knoten.
Im Abschnitt **Eigenschaftenbereich** , dass es über eine Eigenschaft verfügt `cq:model` , die auf [!DNL Event] Definition des Inhaltsfragmentmodells.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Unter dem `data` Knoten wählen Sie die [Übergeordnet](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) und überprüfen Sie die Eigenschaften. Dieser Knoten enthält den Inhalt, der während der Bearbeitung eines [!DNL Event] Inhaltsfragmentmodell. Die JCR-Eigenschaftsnamen entsprechen den Eigenschaftsnamen des Inhaltsfragmentmodells und die Werte entsprechen den verfassten Werten der[!DNL Punkrock Fest]&quot; [!DNL Event] Inhaltsfragment.

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## Nächster Schritt

Es wird empfohlen, die [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) Inhaltspaket in der AEM-Autoreninstanz über [AEM [!UICONTROL Package Manager]](http://localhost:4502/crx/packmgr/index.jsp). Dieses Paket enthält die Konfigurationen und Inhalte, die in diesem und den vorherigen Kapiteln des Tutorials beschrieben werden.

* [Kapitel 4 - AEM Content Services-Vorlagen definieren](./chapter-4.md)

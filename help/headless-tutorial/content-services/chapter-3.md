---
title: Kapitel 3 - Erstellen von Inhaltsfragmenten für Ereignisse - Content Services
seo-title: Erste Schritte mit AEM Content Services - Kapitel 3 - Erstellen von Inhaltsfragmenten für Ereignisse
description: In Kapitel 3 des Tutorials AEM Headless wird das Erstellen und Bearbeiten von Ereignis-Inhaltsfragmenten aus dem in Kapitel 2 erstellten Inhaltsfragmentmodell beschrieben.
seo-description: In Kapitel 3 des Tutorials AEM Headless wird das Erstellen und Bearbeiten von Ereignis-Inhaltsfragmenten aus dem in Kapitel 2 erstellten Inhaltsfragmentmodell beschrieben.
feature: Inhaltsfragmente, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '516'
ht-degree: 3%

---


# Kapitel 3 - Erstellen von Inhaltsfragmenten für Ereignisse

Kapitel 3 des Tutorials AEM Headless umfasst das Erstellen und Bearbeiten von Ereignissen Inhaltsfragmente aus dem Inhaltsfragmentmodell, das in [Kapitel 2](./chapter-2.md) erstellt wurde.

## Erstellen eines Ereignisinhaltsfragments

Wenn ein [!DNL Event] Inhaltsfragmentmodell erstellt und die AEM Konfiguration für WKND auf den Ordner `/content/dam/wknd-mobile` Asset angewendet wird (über die Eigenschaft `cq:conf` ), kann ein [!DNL Event] Inhaltsfragment erstellt werden.

Inhaltsfragmente, bei denen es sich um einen Asset-Typ handelt, sollten wie andere Assets in AEM Assets organisiert und verwaltet werden.

* Verwenden Sie Gebietsschemaordner in der Asset-Ordnerstruktur, wenn eine Übersetzung erforderlich ist (oder sein kann)
* Inhaltsfragmente logisch organisieren, damit sie leicht zu finden und zu verwalten sind

Erstellen Sie in diesem Schritt einen neuen [!DNL Event] für `Punkrock Fest` im Ordner `/content/dam/wknd-mobile/en/events` Assets .

1. Navigieren Sie zu **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Dateien] > [!DNL WKND Mobile] >[!DNL English]** und erstellen Sie Asset-Ordner **[!DNL Events]**.
1. Erstellen Sie in **[!UICONTROL Assets] > [!UICONTROL Dateien] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** ein neues Inhaltsfragment des Typs **[!DNL Event]** mit dem Titel **[!DNL Punkrock Fest]**.
1. Erstellen Sie das neu erstellte Inhaltsfragment [!DNL Event] .

   * [!DNL Event Title]: **[!DNL Punkrock Fest]**
   * [!DNL Event Description] :  **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description=&quot;&quot;>**
   * [!DNL Event Date] :  **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] :  **Musik**
   * [!DNL Ticket Price] :  **10**
   * [!DNL Event Image] :  **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] :  **Das Reptilienhaus**
   * [!DNL Venue City] : **New York**

   Tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Speichern]** , um Änderungen zu speichern.

1. Installieren Sie mit [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp) das unten stehende Paket auf der AEM-Autoreninstanz. Dieses Paket enthält eine Reihe von Event-Inhaltsfragmenten.

   [Datei abrufen: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## Überprüfen der JCR-Struktur des Inhaltsfragments

*Dieser Abschnitt ist nur informativ und dient zum Verknüpfen der zugrunde liegenden JCR-Struktur von Inhaltsfragmenten, die aus Inhaltsfragmentmodellen erstellt wurden.*

1. Öffnen Sie **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** in der AEM-Autoreninstanz.
1. Navigieren Sie in CRXDE Lite im linken Hierarchiemenü zu [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) , dem Knoten, der das Inhaltsfragment [!DNL Punkrock Fest] [!DNL Event] im JCR darstellt.
1. Erweitern Sie den Knoten [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) .
Überprüfen Sie im Bereich **Eigenschaften** , ob es eine Eigenschaft `cq:model` hat, die auf die Definition des Inhaltsfragmentmodells [!DNL Event] verweist.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Wählen Sie unter dem Knoten `data` den Knoten [Übergeordnet](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) aus und überprüfen Sie die Eigenschaften. Dieser Knoten enthält den Inhalt, der beim Authoring eines [!DNL Event] Inhaltsfragmentmodells erfasst wurde. Die Namen der JCR-Eigenschaften entsprechen den Eigenschaftsnamen des Inhaltsfragmentmodells und die Werte entsprechen den verfassten Werten des Inhaltsfragments &quot;[!DNL Punkrock Fest]&quot; [!DNL Event].

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## Nächster Schritt

Es wird empfohlen, das Inhaltspaket [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) in der AEM-Autoreninstanz über [AEM [!UICONTROL Package Manager]](http://localhost:4502/crx/packmgr/index.jsp) zu installieren. Dieses Paket enthält die Konfigurationen und Inhalte, die in diesem und den vorherigen Kapiteln des Tutorials beschrieben werden.

* [Kapitel 4 - AEM Content Services-Vorlagen definieren](./chapter-4.md)

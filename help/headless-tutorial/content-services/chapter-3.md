---
title: Kapitel 3 - Inhaltsfragmente für Authoring von Ereignissen - Content Services
seo-title: Erste Schritte mit AEM Content Services - Kapitel 3 - Inhaltsfragmente im Authoring-Ereignis
description: Kapitel 3 des AEMHeadless-Lernprogramms umfasst das Erstellen und Authoring von Inhaltsfragmenten aus dem Inhaltsfragmentmodell, das in Kapitel 2 erstellt wurde.
seo-description: Kapitel 3 des AEMHeadless-Lernprogramms umfasst das Erstellen und Authoring von Inhaltsfragmenten aus dem Inhaltsfragmentmodell, das in Kapitel 2 erstellt wurde.
feature: Inhaltsfragmente, APIs
topic: Kopflos, Content-Management
role: Entwickler
level: Anfänger
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 4%

---


# Kapitel 3 - Inhaltsfragmente im Authoring-Ereignis

In Kapitel 3 des AEM Headless-Lernprogramms wird das Erstellen und Erstellen von Inhaltsfragmenten aus dem Inhaltsfragmentmodell beschrieben, das in [Kapitel 2](./chapter-2.md) erstellt wurde.

## Erstellen eines Inhaltsfragments für Ereignisse

Wenn ein Inhaltsfragmentmodell [!DNL Event] erstellt und die AEM-Konfiguration für WKND auf den Ordner `/content/dam/wknd-mobile` &quot;Asset&quot;angewendet wird (über die Eigenschaft `cq:conf`), kann ein Inhaltsfragment [!DNL Event] erstellt werden.

Inhaltsfragmente, bei denen es sich um eine Asset-Art handelt, sollten wie andere Assets in AEM Assets organisiert und verwaltet werden.

* Verwenden Sie Gebietsschemaordner in der Ordnerstruktur &quot;Assets&quot;, wenn eine Übersetzung erforderlich ist (oder sein könnte)
* Inhaltsfragmente logisch organisieren, damit sie leicht zu finden und zu verwalten sind

Erstellen Sie in diesem Schritt eine neue [!DNL Event] für `Punkrock Fest` im Ordner `/content/dam/wknd-mobile/en/events` assets.

1. Navigieren Sie zu **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Dateien] > [!DNL WKND Mobile] >[!DNL English]** und erstellen Sie Asset-Ordner **[!DNL Events]**.
1. Erstellen Sie in **[!UICONTROL Assets] > [!UICONTROL Dateien] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** ein neues Inhaltsfragment des Typs **[!DNL Event]** mit dem Titel **[!DNL Punkrock Fest]**.
1. Erstellen Sie das neu erstellte Inhaltsfragment [!DNL Event].

   * [!DNL Event Title]: **[!DNL Punkrock Fest]**
   * [!DNL Event Description] :  **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description=&quot;&quot;>**
   * [!DNL Event Date] :  **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] :  **Musik**
   * [!DNL Ticket Price] :  **10**
   * [!DNL Event Image] :  **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] :  **Reptilienhaus**
   * [!DNL Venue City] : **New York**

   Tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Speichern]**, um die Änderungen zu speichern.

1. Installieren Sie das folgende Paket mit [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp) auf AEM Author. Dieses Paket enthält eine Reihe von Ereignis-Inhaltsfragmenten.

   [Datei abrufen: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## Überprüfen der JCR-Struktur des Inhaltsfragments

*Dieser Abschnitt enthält nur informative Informationen und soll die zugrunde liegende JCR-Struktur von Inhaltsfragmenten aus Inhaltsfragmentmodellen sozialisieren.*

1. Öffnen Sie **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** in AEM Author.
1. In CRXDE Lite navigieren Sie im linken Hierarchiemenü zu [/content/dam/work-mobile/en/Ereignisses/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content), dem Knoten, der das Inhaltsfragment [!DNL Punkrock Fest] [!DNL Event] in der JCR-Datei darstellt.
1. Erweitern Sie den Knoten [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master).
Überprüfen Sie im Bereich **Eigenschaften**, ob eine Eigenschaft `cq:model` vorhanden ist, die auf die Definition des Inhaltsfragmentmodells [!DNL Event] verweist.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Wählen Sie unter dem Knoten `data` den Knoten [Übergeordnet](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) und überprüfen Sie die Eigenschaften. Dieser Knoten enthält den Inhalt, der beim Authoring eines [!DNL Event]-Inhaltsfragmentmodells erfasst wurde. Die Namen der JCR-Eigenschaften entsprechen den Eigenschaftsnamen des Inhaltsfragmentmodells und die Werte entsprechen den verfassten Werten des Inhaltsfragments &quot;[!DNL Punkrock Fest]&quot; [!DNL Event]&quot;.

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## Nächster Schritt

Es wird empfohlen, das Inhaltspaket [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) über [AEM [!UICONTROL Package Manager]](http://localhost:4502/crx/packmgr/index.jsp) zu installieren. Dieses Paket enthält die Konfigurationen und Inhalte, die in diesem und den vorhergehenden Kapiteln des Tutorials beschrieben werden.

* [Kapitel 4 - Definieren AEM Content Services-Vorlagen](./chapter-4.md)

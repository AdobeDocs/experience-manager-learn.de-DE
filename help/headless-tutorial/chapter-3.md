---
title: Kapitel 3 - Inhaltsfragmente im Authoring-Ereignis
seo-title: Erste Schritte mit AEM Content Services - Kapitel 3 - Inhaltsfragmente im Authoring-Ereignis
description: Kapitel 3 des AEMHeadless-Lernprogramms umfasst das Erstellen und Authoring von Inhaltsfragmenten aus dem Inhaltsfragmentmodell, das in Kapitel 2 erstellt wurde.
seo-description: Kapitel 3 des AEMHeadless-Lernprogramms umfasst das Erstellen und Authoring von Inhaltsfragmenten aus dem Inhaltsfragmentmodell, das in Kapitel 2 erstellt wurde.
translation-type: tm+mt
source-git-commit: d7258f8acf6df680795ce61cc8383e60b5b7d722
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 3%

---


# Kapitel 3 - Inhaltsfragmente im Authoring-Ereignis

Kapitel 3 des AEMHeadless-Lernprogramms umfasst das Erstellen und Authoring von Ereignissen Inhaltsfragmente aus dem Inhaltsfragmentmodell, das in [Kapitel 2](./chapter-2.md)erstellt wurde.

## Erstellen eines Inhaltsfragments für Ereignisse

Wenn ein [!DNL Event] Inhaltsfragmentmodell erstellt und die AEM-Konfiguration für WKND auf den `/content/dam/wknd-mobile` Asset-Ordner angewendet wird (über die `cq:conf` Eigenschaft), kann ein [!DNL Event] Inhaltsfragment erstellt werden.

Inhaltsfragmente, bei denen es sich um eine Asset-Art handelt, sollten wie andere Assets in AEM Assets organisiert und verwaltet werden.

* Verwenden Sie Gebietsschemaordner in der Ordnerstruktur &quot;Assets&quot;, wenn eine Übersetzung erforderlich ist (oder sein könnte)
* Inhaltsfragmente logisch organisieren, damit sie leicht zu finden und zu verwalten sind

Erstellen Sie in diesem Schritt eine neue [!DNL Event] für `Punkrock Fest` im `/content/dam/wknd-mobile/en/events` Ordner &quot;assets&quot;.

1. Navigieren Sie zu **[!UICONTROL AEM]>[!UICONTROL Assets]>[!UICONTROL Dateien]>[!DNL WKND Mobile]>[!DNL English]** und erstellen Sie Asset-Ordner **[!DNL Events]**.
1. In **[!UICONTROL Assets]>[!UICONTROL Files]>[!DNL WKND Mobile]>[!DNL English]>[!DNL Events]** Erstellen Sie ein neues Inhaltsfragment des Typs **[!DNL Event]** mit dem Titel **[!DNL Punkrock Fest]**.
1. Erstellen Sie das neu erstellte [!DNL Event] Inhaltsfragment.

   * [!DNL Event Title]: **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;Geben Sie ein paar Zeilen Beschreibung ein...>**
   * [!DNL Event Date] : **&lt;Datum in der Zukunft auswählen>**
   * [!DNL Event Type] : **Musik**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **Reptilienhaus**
   * [!DNL Venue City] : **New York**

   Tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Speichern]** , um die Änderungen zu speichern.

1. Installieren Sie mit [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)das unten stehende Paket auf AEM Author. Dieses Paket enthält eine Reihe von Ereignis-Inhaltsfragmenten.

   [Datei abrufen: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## Überprüfen der JCR-Struktur des Inhaltsfragments

*Dieser Abschnitt enthält nur informative Informationen und soll die zugrunde liegende JCR-Struktur von Inhaltsfragmenten aus Inhaltsfragmentmodellen sozialisieren.*

1. Öffnen Sie die **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** in AEM Author.
1. In CRXDE Lite navigieren Sie im linken Hierarchiemenü zu [/content/dam/work-mobile/en/Ereignisses/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) , dem Knoten, der das [!DNL Punkrock Fest] [!DNL Event] Inhaltsfragment in der JCR-Datei darstellt.
1. Erweitern Sie die [Daten](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) -Node.
Überprüfen Sie im Bereich **Eigenschaften, ob eine Eigenschaft vorhanden ist,** die auf die Definition des `cq:model` [!DNL Event] Inhaltsfragmentmodells verweist.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Wählen Sie unter dem `data` Knoten den Knoten [Übergeordnet](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) aus und überprüfen Sie die Eigenschaften. Dieser Knoten enthält den Inhalt, der beim Authoring eines [!DNL Event] Inhaltsfragmentmodells erfasst wurde. Die Namen der JCR-Eigenschaften entsprechen den Eigenschaftsnamen des Inhaltsfragmentmodells und die Werte entsprechen den verfassten Werten des &quot;[!DNL Punkrock Fest]&quot;- [!DNL Event] Inhaltsfragments.

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## Nächster Schritt

Es wird empfohlen, das [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) -Inhaltspaket über [AEM [!UICONTROL Package Manager]](http://localhost:4502/crx/packmgr/index.jsp)auf AEM Author zu installieren. Dieses Paket enthält die Konfigurationen und Inhalte, die in diesem und den vorhergehenden Kapiteln des Tutorials beschrieben werden.

* [Kapitel 4 - Definieren AEM Content Services-Vorlagen](./chapter-4.md)

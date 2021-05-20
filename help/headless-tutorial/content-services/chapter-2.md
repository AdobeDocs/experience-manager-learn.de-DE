---
title: Kapitel 2 - Definieren von Inhaltsfragmentmodellen für Ereignisse - Content Services
seo-title: Erste Schritte mit AEM Content Services - Kapitel 2 - Definieren von Ereignisinhaltsfragmentmodellen
description: In Kapitel 2 des Tutorials AEM Headless wird die Aktivierung und Definition von Inhaltsfragmentmodellen beschrieben, die zum Definieren einer normalisierten Datenstruktur und einer Authoring-Oberfläche zum Erstellen von Ereignissen verwendet werden.
seo-description: In Kapitel 2 des Tutorials AEM Headless wird die Aktivierung und Definition von Inhaltsfragmentmodellen beschrieben, die zum Definieren einer normalisierten Datenstruktur und einer Authoring-Oberfläche zum Erstellen von Ereignissen verwendet werden.
feature: Inhaltsfragmente, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1002'
ht-degree: 11%

---


# Kapitel 2 - Verwendung von Inhaltsfragmentmodellen

AEM Inhaltsfragmentmodelle definieren Inhaltsschemata, mit denen die Erstellung von Rohinhalten durch AEM-Autoren in Vorlagen erstellt werden kann. Dieser Ansatz ähnelt der Strukturvorlage oder dem formularbasierten Authoring. Das Schlüsselkonzept mit Inhaltsfragmenten besteht darin, dass der erstellte Inhalt von der Präsentation unabhängig ist. Das bedeutet, dass er für die Verwendung über mehrere Kanäle vorgesehen ist. Dabei wird über die aufnehmende Anwendung, AEM, eine Einzelseitenanwendung oder eine Mobile App gesteuert, wie der Inhalt dem Benutzer angezeigt wird.

Das Hauptanliegen des Inhaltsfragments besteht darin, Folgendes sicherzustellen:

1. Der richtige Inhalt wird vom Autor erfasst
2. Der Inhalt kann in einem strukturierten und gut verständlichen Format für anspruchsvolle Anwendungen verfügbar gemacht werden.

In diesem Kapitel wird die Aktivierung und Definition von Inhaltsfragmentmodellen beschrieben, die zum Definieren einer normalisierten Datenstruktur und einer Authoring-Oberfläche für die Modellierung und Erstellung von &quot;Ereignissen&quot;verwendet werden.

## Aktivieren von Inhaltsfragmentmodellen  

Inhaltsfragmentmodelle **müssen** über **[AEM [!UICONTROL Konfigurations-Browser]](https://docs.adobe.com/content/help/de-DE/experience-manager-cloud-service/implementing/developing/configurations.html)** aktiviert werden.

Wenn Inhaltsfragmentmodelle für eine Konfiguration **nicht** aktiviert sind, wird die Schaltfläche **[!UICONTROL Erstellen] > [!UICONTROL Inhaltsfragment]** nicht für die entsprechende AEM angezeigt.

>[!NOTE]
>
>AEM Konfigurationen stellen einen Satz von [kontextsensitiven Mandantenkonfigurationen](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) dar, die unter `/conf` gespeichert sind. Normalerweise korrelieren AEM Konfigurationen mit einer bestimmten Website, die in AEM Sites verwaltet wird, oder einer Geschäftseinheit, die für einen Inhaltsuntersatz (Assets, Seiten usw.) verantwortlich ist. in AEM.
>
>Damit sich eine Konfiguration auf eine Inhaltshierarchie auswirkt, muss über die Eigenschaft `cq:conf` in dieser Inhaltshierarchie auf die Konfiguration verwiesen werden. (Dies wird für die [!DNL WKND Mobile]-Konfiguration in **Schritt 5** unten erreicht.)
>
>Wenn die `global`-Konfiguration verwendet wird, gilt die Konfiguration für alle Inhalte und `cq:conf` muss nicht festgelegt werden.
>
>Weitere Informationen finden Sie in der [[!UICONTROL Dokumentation zum Konfigurations-Browser]](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html) .

1. Melden Sie sich bei der AEM-Autoreninstanz als Benutzer mit entsprechenden Berechtigungen an, um die entsprechende Konfiguration zu ändern.
   * Für dieses Tutorial kann der Benutzer **admin** verwendet werden.
1. Navigieren Sie zu **[!UICONTROL Tool] > [!UICONTROL Allgemein] > [!UICONTROL Konfigurationsbrowser]**
1. Tippen Sie auf das **Ordnersymbol** neben **[!DNL WKND Mobile]**, um auszuwählen, und tippen Sie dann oben links auf die Schaltfläche **[!UICONTROL Bearbeiten]** .
1. Wählen Sie **[!UICONTROL Inhaltsfragmentmodelle]** und tippen Sie oben rechts auf **[!UICONTROL Speichern und schließen]**.

   Dies ermöglicht Inhaltsfragmentmodelle in Inhaltsstrukturen von Asset-Ordnern, auf die die [!DNL WKND Mobile]-Konfiguration angewendet wurde.

   >[!NOTE]
   >
   >Diese Konfigurationsänderung kann nicht über die Webbenutzeroberfläche [!UICONTROL AEM Konfiguration] rückgängig gemacht werden. So machen Sie diese Konfiguration rückgängig:
   >    
   >    1. Öffnen Sie [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. Navigieren Sie zu `/conf/wknd-mobile/settings/dam/cfm`
   >    1. Löschen Sie den Knoten `models` .

   >    
   >Alle vorhandenen Inhaltsfragmentmodelle, die mit dieser Konfiguration erstellt wurden, werden gelöscht und ihre Definitionen werden unter `/conf/wknd-mobile/settings/dam/cfm/models` gespeichert.

1. Wenden Sie die **[!DNL WKND Mobile]**-Konfiguration auf den Ordner **[!DNL WKND Mobile]Assets** an, damit Inhaltsfragmente aus Inhaltsfragmentmodellen in dieser Assets-Ordnerhierarchie erstellt werden können:

   1. Navigieren Sie zu **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Dateien]**
   1. Wählen Sie den Ordner **[!UICONTROL WKND Mobile]** aus.
   1. Tippen Sie in der oberen Aktionsleiste auf die Schaltfläche **[!UICONTROL Eigenschaften]** , um [!UICONTROL Ordnereigenschaften] zu öffnen.
   1. Tippen Sie in [!UICONTROL Ordnereigenschaften] auf die Registerkarte **[!UICONTROL Cloud Services]** .
   1. Stellen Sie sicher, dass das Feld **[!UICONTROL Cloud-Konfiguration]** auf **/conf/wknd-mobile** gesetzt ist.
   1. Tippen Sie oben rechts auf **[!UICONTROL Speichern und schließen]**, um Änderungen beizubehalten.

>[!VIDEO](https://video.tv.adobe.com/v/28336/?quality=12&learn=on)

## Grundlagen zum Erstellen des Inhaltsfragmentmodells

Bevor wir das Inhaltsfragmentmodell definieren, sollten wir uns das Erlebnis ansehen, das wir durchführen werden, um sicherzustellen, dass wir alle erforderlichen Datenpunkte erfassen. Dazu werden wir das Design von Mobile Apps überprüfen und die Design-Elemente der Erfassung von Inhalten zuordnen.

Wir können die Datenpunkte, die ein Ereignis definieren, wie folgt aufschlüsseln:

![Erstellen des Inhaltsfragmentmodells](assets/chapter-2/design-to-model-mapping.png)

Mit dem Mapping bewaffnet können wir das Inhaltsfragment definieren, das zur Erfassung und letztendlich zur Offenlegung der Ereignisdaten verwendet wird.

## Erstellen des Inhaltsfragmentmodells

1. Navigieren Sie zu **[!UICONTROL Tools] > [!UICONTROL Assets] > [!UICONTROL Inhaltsfragmentmodelle]**.
1. Tippen Sie auf den Ordner **[!DNL WKND Mobile]** , um ihn zu öffnen.
1. Tippen Sie auf **[!UICONTROL Erstellen]** , um den Assistenten zur Erstellung von Inhaltsfragmentmodellen zu öffnen.
1. Geben Sie **[!DNL Event]** als **[!UICONTROL Modelltitel]** *(Beschreibung ist optional)* ein und tippen Sie zum Speichern auf **[!UICONTROL Erstellen]**.

>[!VIDEO](https://video.tv.adobe.com/v/28337/?quality=12&learn=on)

## Definieren der Struktur des Inhaltsfragmentmodells

1. Navigieren Sie zu **[!UICONTROL Tools] > [!UICONTROL Assets] > [!UICONTROL Inhaltsfragmentmodelle] >[!DNL WKND]**.
1. Wählen Sie das Inhaltsfragmentmodell **[!DNL Event]** aus und tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Bearbeiten]** .
1. Ziehen Sie vom Tab **[!UICONTROL Datentypen] auf der rechten Seite die**[!UICONTROL  Einzeilige Textanmeldung ]**in die linke Dropzone, um das Feld **[!DNL Question]**zu definieren.**
1. Stellen Sie sicher, dass links die neue Eingabe **[!UICONTROL Einzeiliger Text]** ausgewählt ist und rechts die Registerkarte **[!UICONTROL Eigenschaften]** ausgewählt ist. Füllen Sie die Felder Eigenschaften wie folgt aus:

   * [!UICONTROL Rendern als] : `textfield`
   * [!UICONTROL Feldbezeichnung] : `Event Title`
   * [!UICONTROL Eigenschaftsname] : `eventTitle`
   * [!UICONTROL Max. Länge] : 25
   * [!UICONTROL Erforderlich] : `Yes`

Wiederholen Sie diese Schritte mit den unten definierten Eingabedefinitionen, um den Rest des Ereignisinhaltsfragmentmodells zu erstellen.

>[!NOTE]
>
> Die Felder **Eigenschaftsname** MÜSSEN exakt übereinstimmen, da die Android-Anwendung so programmiert ist, dass diese Namen eingegeben werden.

### Ereignisbeschreibung

* [!UICONTROL Datentyp] : `Multi-line text`
* [!UICONTROL Feldbezeichnung] : `Event Description`
* [!UICONTROL Eigenschaftsname] : `eventDescription`
* [!UICONTROL Standardtyp] : `Rich text`

### Ereignisdatum und -zeit

* [!UICONTROL Datentyp] : `Date and time`
* [!UICONTROL Feldbezeichnung] : `Event Date and Time`
* [!UICONTROL Eigenschaftsname] : `eventDateAndTime`
* [!UICONTROL Erforderlich] : `Yes`

### Ereignistyp

* [!UICONTROL Datentyp] : `Enumeration`
* [!UICONTROL Feldbezeichnung] : `Event Type`
* [!UICONTROL Eigenschaftsname] : `eventType`
* [!UICONTROL Optionen] : `Art,Music,Performance,Photography`

### Ticketpreis

* [!UICONTROL Datentyp] : `Number`
* [!UICONTROL Rendern als] : `numberfield`
* [!UICONTROL Feldbezeichnung] : `Ticket Price`
* [!UICONTROL Eigenschaftsname] : `eventPrice`
* [!UICONTROL Typ] : `Integer`
* [!UICONTROL Erforderlich] : `Yes`

### Ereignisbild

* [!UICONTROL Datentyp] : `Content Reference`
* [!UICONTROL Rendern als] : `contentreference`
* [!UICONTROL Feldbezeichnung] : `Event Image`
* [!UICONTROL Eigenschaftsname] : `eventImage`
* [!UICONTROL Stammverzeichnis] : `/content/dam/wknd-mobile/images`
* [!UICONTROL Erforderlich] : `Yes`

### Name des Veranstaltungsorts

* [!UICONTROL Datentyp] : `Single-line text`
* [!UICONTROL Rendern als] : `textfield`
* [!UICONTROL Feldbezeichnung] : `Venue Name`
* [!UICONTROL Eigenschaftsname] : `venueName`
* [!UICONTROL Max. Länge] : 20
* [!UICONTROL Erforderlich] : `Yes`

### Stadt

* [!UICONTROL Datentyp] : `Enumeration`
* [!UICONTROL Feldbezeichnung] : `Venue City`
* [!UICONTROL Eigenschaftsname] : `venueCity`
* [!UICONTROL Optionen] : `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335/?quality=12&learn=on)

>[!NOTE]
>
>Der **[!UICONTROL Eigenschaftsname]** gibt den **beides** den JCR-Eigenschaftsnamen an, unter dem dieser Wert gespeichert wird, sowie den Schlüssel in der JSON-Datei . Dies sollte ein semantischer Name sein, der sich während der Lebensdauer des Inhaltsfragmentmodells nicht ändert.

Nachdem Sie die Erstellung des Inhaltsfragmentmodells abgeschlossen haben, sollten Sie am Ende eine Definition haben, die wie folgt aussieht:


![Ereignisinhaltsfragmentmodell](assets/chapter-2/event-content-fragment-model.png)

## Nächster Schritt

Optional können Sie das Inhaltspaket [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) in der AEM-Autoreninstanz über [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp) installieren. Dieses Paket enthält die Konfigurationen und Inhalte, die in diesem Teil des Tutorials beschrieben werden.

* [Kapitel 3 - Erstellen von Inhaltsfragmenten für Ereignisse](./chapter-3.md)

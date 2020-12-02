---
title: Kapitel 2 - Definieren von Ereignis Content Fragment-Modellen - Content Services
seo-title: Erste Schritte mit AEM Content Services - Kapitel 2 - Definieren von Ereignis Content Fragment-Modellen
description: Kapitel 2 des Lernprogramms "AEM ohne Kopf"umfasst die Aktivierung und Definition von Inhaltsfragmentmodellen, mit denen eine normalisierte Datenstruktur und eine Authoring-Oberfläche zum Erstellen von Ereignissen definiert werden.
seo-description: Kapitel 2 des Lernprogramms "AEM ohne Kopf"umfasst die Aktivierung und Definition von Inhaltsfragmentmodellen, mit denen eine normalisierte Datenstruktur und eine Authoring-Oberfläche zum Erstellen von Ereignissen definiert werden.
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '996'
ht-degree: 11%

---


# Kapitel 2 - Verwenden von Inhaltsfragmentmodellen

AEM Inhaltsfragmentmodelle definieren Content-Schema, die verwendet werden können, um die Erstellung von Rohinhalten durch AEM Autoren vorzubereiten. Dieser Ansatz ähnelt dem Gerüst- oder formularbasierten Authoring. Das Schlüsselkonzept bei Inhaltsfragmenten ist, dass der erstellte Inhalt präsentationsunabhängig ist. Das bedeutet, dass er für die Verwendung mit mehreren Kanälen vorgesehen ist, wobei die aufnehmende Anwendung, ob AEM, eine Einzelseitenanwendung oder eine mobile App, steuert, wie der Inhalt dem Benutzer angezeigt wird.

Das Hauptanliegen des Inhaltsfragments besteht darin, Folgendes sicherzustellen:

1. Der richtige Inhalt wird vom Autor erfasst
2. Der Inhalt kann in einem strukturierten und gut verständlichen Format für anspruchsvolle Anwendungen bereitgestellt werden.

Dieses Kapitel behandelt die Aktivierung und Definition von Inhaltsfragmentmodellen, die zur Definition einer normalisierten Datenstruktur und einer Authoring-Oberfläche für die Modellierung und Erstellung von &quot;Ereignissen&quot;verwendet werden.

## Aktivieren von Inhaltsfragmentmodellen  

Inhaltsfragmentmodelle **müssen über**[ AEM [!UICONTROL Konfigurationsbrowser]](https://docs.adobe.com/content/help/de-DE/experience-manager-cloud-service/implementing/developing/configurations.html)**&lt;a6/> aktiviert werden.**

Wenn Inhaltsfragmentmodelle für eine Konfiguration **nicht** aktiviert sind, wird die Schaltfläche **[!UICONTROL Erstellen] > [!UICONTROL Inhaltsfragment]** für die entsprechende AEM-Konfiguration nicht angezeigt.

>[!NOTE]
>
>AEM Konfigurationen stellen einen Satz von [kontextsensitiven Mandantenkonfigurationen](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) dar, die unter `/conf` gespeichert werden. Normalerweise korrelieren AEM Konfigurationen mit einer bestimmten Website, die in AEM Sites verwaltet wird, oder mit einer Geschäftseinheit, die für einen Teil des Inhalts (Assets, Seiten usw.) verantwortlich ist. aem
>
>Damit eine Konfiguration eine Inhaltshierarchie beeinflussen kann, muss über die `cq:conf`-Eigenschaft in dieser Inhaltshierarchie auf die Konfiguration verwiesen werden. (Dies wird für die [!DNL WKND Mobile]-Konfiguration unter **Schritt 5** erreicht).
>
>Wenn die `global`-Konfiguration verwendet wird, gilt die Konfiguration für alle Inhalte und `cq:conf` muss nicht eingestellt werden.
>
>Weitere Informationen finden Sie in der Dokumentation [[!UICONTROL Konfigurationsbrowser]](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html).

1. Melden Sie sich bei AEM Author als Benutzer mit den entsprechenden Berechtigungen an, um die entsprechende Konfiguration zu ändern.
   * Für dieses Lernprogramm kann der Benutzer **admin** verwendet werden.
1. Navigieren Sie zu **[!UICONTROL Tool] > [!UICONTROL Allgemein] > [!UICONTROL Konfigurationsbrowser]**
1. Tippen Sie zum Auswählen auf das **Ordnersymbol** neben **[!DNL WKND Mobile]** und dann auf die Schaltfläche **[!UICONTROL Bearbeiten] links oben.**
1. Wählen Sie **[!UICONTROL Inhaltsfragmentmodelle]** und tippen Sie oben rechts auf **[!UICONTROL Speichern und schließen]**.

   Dadurch werden Inhaltsfragmentmodelle für Inhaltsbäume aktiviert, auf die die [!DNL WKND Mobile]-Konfiguration angewendet wurde.

   >[!NOTE]
   >
   >Diese Konfigurationsänderung ist in der Webbenutzeroberfläche [!UICONTROL AEM Konfiguration] nicht rückgängig zu machen. So machen Sie diese Konfiguration rückgängig:
   >    
   >    1. Öffnen Sie [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. Navigieren Sie zu `/conf/wknd-mobile/settings/dam/cfm`
   >    1. Knoten `models` löschen

   >    
   >Alle vorhandenen Inhaltsfragmentmodelle, die mit dieser Konfiguration erstellt wurden, werden gelöscht und ihre Definitionen werden unter `/conf/wknd-mobile/settings/dam/cfm/models` gespeichert.

1. Wenden Sie die **[!DNL WKND Mobile]**-Konfiguration auf den **[!DNL WKND Mobile]-Asset-Ordner** an, damit Inhaltsfragmente aus Inhaltsfragmentmodellen in der Asset-Ordnerhierarchie erstellt werden können:

   1. Navigieren Sie zu **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Dateien]**
   1. Wählen Sie den Ordner **[!UICONTROL WKND Mobile]**
   1. Tippen Sie in der oberen Aktionsleiste auf die Schaltfläche **[!UICONTROL Eigenschaften]**, um [!UICONTROL Ordnereigenschaften] zu öffnen.
   1. Tippen Sie unter [!UICONTROL Ordnereigenschaften] auf die Registerkarte **[!UICONTROL Cloud Services]**
   1. Überprüfen Sie, ob das Feld **[!UICONTROL Cloud-Konfiguration]** auf **/conf/wknd-mobile** eingestellt ist.
   1. Tippen Sie oben rechts auf **[!UICONTROL Speichern &amp; Schließen]**, um die Änderungen beizubehalten.

>[!VIDEO](https://video.tv.adobe.com/v/28336/?quality=12&learn=on)

## Grundlagen zum Erstellen des Inhaltsfragmentmodells

Bevor wir das Inhaltsfragmentmodell definieren, sollten wir die Erfahrung, die wir machen werden, überprüfen, um sicherzustellen, dass wir alle erforderlichen Datenpunkte erfassen. Dazu überprüfen wir das Design von Mobile-Anwendungen und ordnen die Designelemente der zu erfassenden Inhalte zu.

Wir können die Datenpunkte, die ein Ereignis definieren, wie folgt ausblenden:

![Erstellen des Inhaltsfragmentmodells](assets/chapter-2/design-to-model-mapping.png)

Ausgerüstet mit der Zuordnung können wir Inhaltsfragment definieren, das zur Erfassung und letztendlich Offenlegung der Ereignis-Daten verwendet wird.

## Erstellen des Inhaltsfragmentmodells

1. Navigieren Sie zu **[!UICONTROL Tools] > [!UICONTROL Assets] > [!UICONTROL Inhaltsfragmentmodelle]**.
1. Tippen Sie zum Öffnen auf den Ordner **[!DNL WKND Mobile]**.
1. Tippen Sie auf **[!UICONTROL Create]**, um den Assistenten zum Erstellen von Inhaltsfragmenten zu öffnen.
1. Geben Sie **[!DNL Event]** als **[!UICONTROL Modelltitel]** *(Beschreibung ist optional)* ein und tippen Sie zum Speichern auf **[!UICONTROL Erstellen]**.

>[!VIDEO](https://video.tv.adobe.com/v/28337/?quality=12&learn=on)

## Definieren der Struktur des Inhaltsfragmentmodells

1. Navigieren Sie zu **[!UICONTROL Tools] > [!UICONTROL Assets] > [!UICONTROL Inhaltsfragmentmodelle] >[!DNL WKND]**.
1. Wählen Sie das Inhaltsfragmentmodell aus und tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Bearbeiten]**.**[!DNL Event]**
1. Ziehen Sie auf der Registerkarte **[!UICONTROL Datentypen] auf der rechten Seite die**[!UICONTROL  Einzeilige Texteingabe ]**in die linke Dropzone, um das **[!DNL Question]**-Feld zu definieren.**
1. Vergewissern Sie sich, dass links die neue Texteingabe **[!UICONTROL Einzeilig]** ausgewählt ist und auf der rechten Seite die Registerkarte **[!UICONTROL Eigenschaften] ausgewählt ist.** Füllen Sie die Felder Eigenschaften wie folgt aus:

   * [!UICONTROL Rendern als] : `textfield`
   * [!UICONTROL Feldbezeichnung] : `Event Title`
   * [!UICONTROL Eigenschaftsname] : `eventTitle`
   * [!UICONTROL Max. Länge] : 25
   * [!UICONTROL Erforderlich] : `Yes`

Wiederholen Sie diese Schritte mit den unten definierten Eingabedefinitionen, um den Rest des Ereignis Content Fragment Model zu erstellen.

>[!NOTE]
>
> Die Felder **Eigenschaftsname** MÜSSEN exakt übereinstimmen, da die Android-Anwendung so programmiert ist, dass diese Namen markiert werden.

### Ereignisbeschreibung

* [!UICONTROL Datentyp] : `Multi-line text`
* [!UICONTROL Feldbezeichnung] : `Event Description`
* [!UICONTROL Eigenschaftsname] : `eventDescription`
* [!UICONTROL Standardtyp] : `Rich text`

### Datum und Uhrzeit des Ereignisses

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

### Ereignis

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

### Ort

* [!UICONTROL Datentyp] : `Enumeration`
* [!UICONTROL Feldbezeichnung] : `Venue City`
* [!UICONTROL Eigenschaftsname] : `venueCity`
* [!UICONTROL Optionen] : `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335/?quality=12&learn=on)

>[!NOTE]
>
>Der **[!UICONTROL Eigenschaftsname]** gibt den **sowohl** als JCR-Eigenschaftsnamen an, in dem dieser Wert gespeichert wird, als auch den Schlüssel in der JSON-Datei. Dies sollte ein semantischer Name sein, der sich während der Lebensdauer des Inhaltsfragmentmodells nicht ändert.

Nachdem Sie das Erstellen des Inhaltsfragmentmodells abgeschlossen haben, sollten Sie eine Definition erhalten, die wie folgt aussieht:


![Ereignis Content Fragment Model](assets/chapter-2/event-content-fragment-model.png)

## Nächster Schritt

Optional können Sie das Inhaltspaket [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) auf AEM Author über [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp) installieren. Dieses Paket enthält die Konfigurationen und Inhalte, die in diesem Teil des Lernprogramms beschrieben werden.

* [Kapitel 3 - Inhaltsfragmente im Authoring-Ereignis](./chapter-3.md)

---
title: Kapitel 2 – Definieren von Inhaltsfragmentmodellen für Ereignisse – Content Services
seo-title: Getting Started with AEM Content Services - Chapter 2 - Defining Event Content Fragment Models
description: In Kapitel 2 des AEM Headless-Tutorials werden die Aktivierung und Definition von Inhaltsfragmentmodellen beschrieben, die zum Definieren einer normalisierten Datenstruktur und einer Authoring-Oberfläche für die Erstellung von Ereignissen verwendet werden.
seo-description: Chapter 2 of the AEM Headless tutorial covers enabling and defining Content Fragment Models used to define a normalized data structure and authoring interface for creating Events.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 100%

---

# Kapitel 2 – Verwendung von Inhaltsfragmentmodellen

AEM-Inhaltsfragmentmodelle definieren Inhaltsschemata, mit denen die Erstellung von Raw-Inhalt durch AEM-Autorinnen und -Autoren in Vorlagen vorkonfiguriert werden kann. Dieser Ansatz ähnelt der Strukturvorlage bzw. dem formularbasierten Authoring. Das Schlüsselkonzept bei Inhaltsfragmenten besteht darin, dass der erstellte Inhalt von der Präsentation unabhängig ist. Das bedeutet, dass er für die Verwendung über mehrere Kanäle vorgesehen ist. Dabei wird über die verarbeitende Anwendung, z. B. AEM, eine Single Page Application oder eine Mobile App, gesteuert, wie der Inhalt Benutzenden angezeigt wird.

Mit dem Inhaltsfragment soll vor allem Folgendes sichergstellt werden:

1. Der richtige Inhalt wird von der Autorin oder dem Autor erfasst
2. Der Inhalt kann in einem strukturierten und gut verständlichen Format für die verarbeitenden Anwendungen verfügbar gemacht werden.

In diesem Kapitel werden die Aktivierung und Definition von Inhaltsfragmentmodellen beschrieben, die zum Definieren einer normalisierten Datenstruktur und einer Authoring-Oberfläche für die Modellierung und Erstellung von „Ereignissen“ verwendet werden.

## Aktivieren von Inhaltsfragmentmodellen 

Inhaltsfragmentmodelle **müssen** über den **[AEM-[!UICONTROL Konfigurations-Browser]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=de)** aktiviert werden.

Wenn Inhaltsfragmentmodelle **nicht** für eine Konfiguration aktiviert sind, wird die Schaltfläche **[!UICONTROL Erstellen] > [!UICONTROL Inhaltsfragment]** für die entsprechende AEM-Konfiguration nicht angezeigt.

>[!NOTE]
>
>Bei AEM-Konfigurationen handelt es sich um [kontextsensitive Mandantenkonfigurationen](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html), die unter `/conf` gespeichert sind. Normalerweise korrelieren AEM-Konfigurationen mit einer bestimmten in AEM Sites verwalteten Website oder einer Geschäftseinheit, die für eine Untergruppe von Inhalten (Assets, Seiten usw.) in AEM verantwortlich ist.
>
>Damit sich eine Konfiguration auf eine Inhaltshierarchie auswirkt, muss die Konfiguration über die `cq:conf`-Eigenschaft in dieser Inhaltshierarchie referenziert werden (wie bei der [!DNL WKND Mobile]-Konfiguration in **Schritt 5** unten).
>
>Wenn die `global`-Konfiguration verwendet wird, gilt die Konfiguration für alle Inhalte, und `cq:conf` muss nicht festgelegt werden.
>
>Weitere Informationen finden Sie in der Dokumentation zum [[!UICONTROL Konfigurations-Browser].](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=de)

1. Melden Sie sich bei AEM Author mit entsprechenden Benutzerberechtigungen an, um die fragliche Konfiguration zu ändern.
   * In diesem Tutorial kann dafür **admin** verwendet werden.
1. Navigieren Sie zu **[!UICONTROL Tools] > [!UICONTROL Allgemein] > [!UICONTROL Konfigurations-Browser]**
1. Tippen Sie auf das **Ordnersymbol** neben **[!DNL WKND Mobile]**, um den Ordner auszuwählen, und tippen Sie dann oben links auf die Schaltfläche **[!UICONTROL Bearbeiten]**.
1. Wählen Sie **[!UICONTROL Inhaltsfragmentmodelle]** aus und tippen Sie oben rechts auf **[!UICONTROL Speichern und schließen]**.

   Dies ermöglicht Inhaltsfragmentmodelle in Inhaltsstrukturen von Asset-Ordnern, auf die die [!DNL WKND Mobile]-Konfiguration angewendet wurde.

   >[!NOTE]
   >
   >Diese Konfigurationsänderung kann nicht über die Web-Benutzeroberfläche [!UICONTROL AEM-Konfiguration] rückgängig gemacht werden. So machen Sie diese Konfiguration rückgängig:
   >    
   >    1. Öffnen Sie [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. Navigieren Sie zu `/conf/wknd-mobile/settings/dam/cfm`
   >    1. Löschen Sie den `models`-Knoten

   >    
   >Alle mit dieser Konfiguration erstellten vorhandenen Inhaltsfragmentmodelle werden gelöscht und ihre Definitionen werden unter `/conf/wknd-mobile/settings/dam/cfm/models` gespeichert.

1. Wenden Sie die **[!DNL WKND Mobile]**-Konfiguration auf den **[!DNL WKND Mobile]-Asset-Ordner** an, damit Inhaltsfragmente aus Inhaltsfragmentmodellen in dieser Assets-Ordnerhierarchie erstellt werden können:

   1. Navigieren Sie zu **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Dateien]**
   1. Wählen Sie den Ordner **[!UICONTROL WKND Mobile]** aus
   1. Tippen Sie in der oberen Aktionsleiste auf die Schaltfläche **[!UICONTROL Eigenschaften]**, um die [!UICONTROL Ordnereigenschaften] zu öffnen
   1. Tippen Sie unter [!UICONTROL Ordnereigenschaften] auf die Registerkarte **[!UICONTROL Cloud-Services]**
   1. Vergewissern Sie sich, dass das Feld **[!UICONTROL Cloud-Konfiguration]** auf **/conf/wknd-mobile** eingestellt ist
   1. Tippen Sie oben rechts auf **[!UICONTROL Speichern und schließen]**, um Änderungen beizubehalten

>[!VIDEO](https://video.tv.adobe.com/v/28336?quality=12&learn=on)

>[!WARNING]
>
> __Inhaltsfragmentmodelle__ wurden von __Tools > Assets__ nach __Tools > Allgemein__ verschoben.

## Grundlegendes zum Erstellen von Inhaltsfragmentmodellen

Vor der Definition des Inhaltsfragmentmodells sollten wir uns das vorgesehene Erlebnis ansehen, um sicherzustellen, dass wir alle erforderlichen Datenpunkte erfassen. Dazu werden wir das App-Design überprüfen und die Design-Elemente dem zu erfassenden Inhalt zuordnen.

Wir können die Datenpunkte, die ein Ereignis definieren, wie folgt aufschlüsseln:

![Erstellen des Inhaltsfragmentmodells](assets/chapter-2/design-to-model-mapping.png)

Mittels Zuordnung können wir Inhaltsfragmente definieren, die zur Erfassung und letztendlich zur Bereitstellung der Ereignisdaten verwendet werden.

## Erstellen des Inhaltsfragmentmodells

1. Navigieren Sie zu **[!UICONTROL Tools] > [!UICONTROL Allgemein] > [!UICONTROL Inhaltsfragmentmodelle]**.
1. Tippen Sie auf den Ordner **[!DNL WKND Mobile]**, um ihn zu öffnen.
1. Tippen Sie auf **[!UICONTROL Erstellen]**, um den Assistenten zur Erstellung von Inhaltsfragmentmodellen zu öffnen.
1. Geben Sie **[!DNL Event]** als **[!UICONTROL Modelltitel]** *ein (Beschreibung ist optional)* und tippen Sie auf **[!UICONTROL Erstellen]**, um ihn zu speichern.

>[!VIDEO](https://video.tv.adobe.com/v/28337?quality=12&learn=on)

## Definieren der Struktur des Inhaltsfragmentmodells

1. Navigieren Sie zu **[!UICONTROL Tools] > [!UICONTROL Allgemein] > [!UICONTROL Inhaltsfragmentmodelle] >[!DNL WKND]**.
1. Wählen Sie das Inhaltsfragmentmodell **[!DNL Event]** aus und tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Bearbeiten]**.
1. Ziehen Sie rechts aus der Registerkarte **[!UICONTROL Datentypen]** die **[!UICONTROL Einzelzeilentext-Eingabe]** in den linken Ablagebereich, um das Feld **[!DNL Question]** zu definieren.
1. Stellen Sie sicher, dass die neue **[!UICONTROL Einzelzeilentext-Eingabe]** auf der linken Seite und die Registerkarte **[!UICONTROL Eigenschaften]** auf der rechten Seite ausgewählt ist. Füllen Sie die Eigenschaftsfelder wie folgt aus:

   * [!UICONTROL Rendern als]: `textfield`
   * [!UICONTROL Feldbezeichnung]: `Event Title`
   * [!UICONTROL Eigenschaftsname]: `eventTitle`
   * [!UICONTROL Max. Länge]: 25
   * [!UICONTROL Erforderlich]: `Yes`

Wiederholen Sie diese Schritte entsprechend den unten stehenden Eingabedefinitionen, um den Rest des Ereignis-Inhaltsfragmentmodells zu erstellen.

>[!NOTE]
>
> Die Felder **Eigenschaftsname** MÜSSEN exakt übereinstimmen, da die Android-Anwendung so programmiert ist, dass sie diese Namen verwendet.

### Ereignisbeschreibung

* [!UICONTROL Datentyp]: `Multi-line text`
* [!UICONTROL Feldbezeichnung]: `Event Description`
* [!UICONTROL Eigenschaftsname]: `eventDescription`
* [!UICONTROL Standardtyp]: `Rich text`

### Ereignisdatum und -uhrzeit

* [!UICONTROL Datentyp]: `Date and time`
* [!UICONTROL Feldbezeichnung]: `Event Date and Time`
* [!UICONTROL Eigenschaftsname]: `eventDateAndTime`
* [!UICONTROL Erforderlich]: `Yes`

### Ereignistyp

* [!UICONTROL Datentyp]: `Enumeration`
* [!UICONTROL Feldbezeichnung]: `Event Type`
* [!UICONTROL Eigenschaftsname]: `eventType`
* [!UICONTROL Optionen]: `Art,Music,Performance,Photography`

### Ticketpreis

* [!UICONTROL Datentyp]: `Number`
* [!UICONTROL Rendern als]: `numberfield`
* [!UICONTROL Feldbezeichnung]: `Ticket Price`
* [!UICONTROL Eigenschaftsname]: `eventPrice`
* [!UICONTROL Typ]: `Integer`
* [!UICONTROL Erforderlich]: `Yes`

### Ereignisbild

* [!UICONTROL Datentyp]: `Content Reference`
* [!UICONTROL Rendern als]: `contentreference`
* [!UICONTROL Feldbezeichnung]: `Event Image`
* [!UICONTROL Eigenschaftsname]: `eventImage`
* [!UICONTROL Stammverzeichnis]: `/content/dam/wknd-mobile/images`
* [!UICONTROL Erforderlich]: `Yes`

### Ortsbezeichnung

* [!UICONTROL Datentyp]: `Single-line text`
* [!UICONTROL Rendern als]: `textfield`
* [!UICONTROL Feldbezeichnung]: `Venue Name`
* [!UICONTROL Eigenschaftsname]: `venueName`
* [!UICONTROL Max. Länge]: 20
* [!UICONTROL Erforderlich]: `Yes`

### Stadt

* [!UICONTROL Datentyp]: `Enumeration`
* [!UICONTROL Feldbezeichnung]: `Venue City`
* [!UICONTROL Eigenschaftsname]: `venueCity`
* [!UICONTROL Optionen]: `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335?quality=12&learn=on)

>[!NOTE]
>
>Der **[!UICONTROL Eigenschaftsname]** umfasst **sowohl** den JCR-Eigenschaftsnamen, in dem dieser Wert gespeichert wird, als auch den Schlüssel in der JSON-Datei. Dies sollte ein semantischer Name sein, der sich während der Lebensdauer des Inhaltsfragmentmodells nicht ändert.

Nachdem Sie die Erstellung des Inhaltsfragmentmodells abgeschlossen haben, sollten Sie eine Definition vorliegen haben, die in etwa wie folgt aussieht:


![Ereignis-Inhaltsfragmentmodell](assets/chapter-2/event-content-fragment-model.png)

## Nächster Schritt

Installieren Sie in AEM Author optional das Inhaltspaket [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) über den [Package Manager von AEM](http://localhost:4502/crx/packmgr/index.jsp). Dieses Paket enthält die Konfigurationen und Inhalte, die in diesem Teil des Tutorials beschrieben wurden.

* [Kapitel 3 – Erstellen von Inhaltsfragmenten für Ereignisse](./chapter-3.md)

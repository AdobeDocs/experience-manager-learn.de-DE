---
title: Kapitel 5 - Inhaltserstellung für Seiten in Content Services - Content Services
description: Kapitel 5 des Tutorials AEM Headless umfasst die Erstellung von Seiten aus den in Kapitel 4 definierten Vorlagen. Diese Seiten fungieren als JSON-HTTP-Endpunkte.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 873d8e69-5a05-44ac-8dae-bba21f82b823
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 1%

---

# Kapitel 5 - Inhaltserstellung für Seiten in Content Services

Kapitel 5 des Tutorials AEM Headless umfasst die Erstellung der Seite anhand der in Kapitel 4 definierten Vorlagen. Die in diesem Kapitel erstellte Seite dient als JSON-HTTP-Endpunkt für die Mobile App.

>[!NOTE]
>
> Die Seiteninhaltsarchitektur von `/content/wknd-mobile/en/api` wurde vorkonfiguriert. Die Basisseiten von `en` und `api` Sie erfüllen einen architektonischen und organisatorischen Zweck, sind jedoch nicht unbedingt erforderlich. Wenn API-Inhalte möglicherweise lokalisiert werden, empfiehlt es sich, die üblichen Best Practices für die Sprachkopie und die Multi-Site-Manager-Seitenorganisation zu befolgen, da die API-Seite wie jede andere AEM Sites-Seite lokalisiert werden kann.

## Erstellen der Ereignis-API-Seite

1. Navigieren Sie zu **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **Tippen Sie auf den Titel der API-Seite** und tippen Sie dann auf **Erstellen** in der oberen Aktionsleiste und erstellen Sie eine neue API-Seite für Ereignisse unter der API-Seite.
   1. Tippen **Erstellen** in der oberen Aktionsleiste
   1. Auswählen **Ereignis-API** template
   1. Im **Name** Feldeingabe **events**
   1. Im **Titel** Feldeingabe **Ereignis-API**
   1. Tippen **Erstellen** in der oberen Aktionsleiste, um die Seite zu erstellen
   1. Tippen **Fertig** , um zum AEM Sites-Administrator zurückzukehren

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## Bearbeiten der API-Seite für Ereignisse

>[!NOTE]
>
> Das Projekt bietet CSS, um einige grundlegende Stile für das Autorenerlebnis bereitzustellen.

1. Bearbeiten Sie die **Ereignis-API** Seite durch Navigieren zu **AEM > Sites > WKND Mobile > Englisch > API**, die **Ereignis-API** Seite und Tippen **Bearbeiten** in der oberen Aktionsleiste.
1. Hinzufügen einer **Logo image** , um sie in der App anzuzeigen, indem Sie sie per Drag-and-Drop aus der Asset-Suche auf den Bild-Komponenten-Platzhalter ziehen.
   * Verwenden Sie das bereitgestellte Logo unter `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. Hinzufügen **Tag-Zeile** , um über den Ereignissen anzuzeigen.
   1. Bearbeiten Sie die **Text** component
   1. Geben Sie den Text ein:
      * `The WKND is here.`

1. Wählen Sie die **events** zur Anzeige:
   1. Legen Sie die folgende Konfiguration auf der **Eigenschaften** tab:
      * Modell: **Ereignis**
      * Übergeordneter Pfad: **/content/dam/wknd-mobile/en/events**
      * Tags: **&lt;leave blank=&quot;&quot;>**
   1. Legen Sie die folgende Konfiguration auf der **Elemente** tab:
      * Entfernen Sie alle aufgelisteten Elemente, um sicherzustellen, dass ALLE Elemente der Event Content Fragments verfügbar gemacht werden.

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## Überprüfen der JSON-Ausgabe der API-Seite

Die JSON-Ausgabe und ihr Format können überprüft werden, indem die Seite mit der `.model.json` auswählen.

Diese JSON-Struktur (oder das Schema) muss von den Nutzern dieser API gut verstanden werden. Es ist wichtig, dass API-Kunden verstehen, welche Aspekte der Struktur fest sind (d. h. das Logo (Bild) und das Tag live (Text) der Ereignis-API, die fließend sind (d. h. die Ereignisse, die unter Inhaltsfragmentlisten-Komponente aufgeführt sind).

Wenn Sie diesen Vertrag auf eine veröffentlichte API unterbrechen, kann dies zu einem fehlerhaften Verhalten in den verbrauchenden Apps führen.

1. Rufen Sie in den neuen Browser-Registerkarten die API-Seiten für Ereignisse mit dem `.model.json` -Selektor, der den JSON-Exporter AEM Content Services aufruft und die Seite und Komponenten in eine normalisierte, klar definierte JSON-Struktur serialisiert.

   Die von diesen Seiten erzeugte JSON-Struktur ist die Struktur, an die Apps ausgerichtet werden müssen.

1. Anfordern der **Ereignis-API** Seite als **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   Das Ergebnis sollte in etwa wie folgt aussehen:

![JSON-Ausgabe AEM Content Services](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Diese JSON kann in einer **Aufräumen** (formatierte) Mode für die Lesbarkeit von Menschen mithilfe der `.tidy` selector:
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## Nächster Schritt

Installieren Sie optional die [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) Inhaltspaket in der AEM-Autoreninstanz über [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp). Dieses Paket enthält die Konfigurationen und Inhalte, die in diesem und den vorherigen Kapiteln des Tutorials beschrieben werden.

* [Kapitel 6 - Veröffentlichung des Inhalts in AEM Publish as JSON](./chapter-6.md)

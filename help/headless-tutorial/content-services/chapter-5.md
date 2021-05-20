---
title: Kapitel 5 - Inhaltserstellung für Seiten in Content Services - Content Services
description: Kapitel 5 des Tutorials AEM Headless umfasst die Erstellung von Seiten aus den in Kapitel 4 definierten Vorlagen. Diese Seiten fungieren als JSON-HTTP-Endpunkte.
feature: Inhaltsfragmente, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 1%

---


# Kapitel 5 - Inhaltserstellung für Seiten in Content Services

Kapitel 5 des Tutorials AEM Headless umfasst die Erstellung der Seite anhand der in Kapitel 4 definierten Vorlagen. Die in diesem Kapitel erstellte Seite dient als JSON-HTTP-Endpunkt für die Mobile App.

>[!NOTE]
>
> Die Seiteninhaltsarchitektur von `/content/wknd-mobile/en/api` wurde vorkonfiguriert. Die Basisseiten von `en` und `api` dienen architektonischen und organisatorischen Zwecken, sind jedoch nicht unbedingt erforderlich. Wenn API-Inhalte möglicherweise lokalisiert werden, empfiehlt es sich, die üblichen Best Practices für die Sprachkopie und die Multi-Site-Manager-Seitenorganisation zu befolgen, da die API-Seite wie jede andere AEM Sites-Seite lokalisiert werden kann.

## Erstellen der Ereignis-API-Seite

1. Navigieren Sie zu **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **Tippen Sie auf die Beschriftung der API-Seite**, tippen Sie in der oberen Aktionsleiste auf die Schaltfläche  **** Erstellen und erstellen Sie eine neue API-Seite für Ereignisse unter der API-Seite.
   1. Tippen Sie in der oberen Aktionsleiste auf **Erstellen** .
   1. Vorlage **Events API** auswählen
   1. Geben Sie im Feld **Name** **events** ein.
   1. Geben Sie im Feld **Title** **Events API** ein.
   1. Tippen Sie in der oberen Aktionsleiste auf **Erstellen** , um die Seite zu erstellen.
   1. Tippen Sie auf **Fertig** , um zum AEM Sites-Administrator zurückzukehren.

>[!VIDEO](https://video.tv.adobe.com/v/28340/?quality=12&learn=on)

## Bearbeiten der API-Seite für Ereignisse

>[!NOTE]
>
> Das Projekt bietet CSS, um einige grundlegende Stile für das Autorenerlebnis bereitzustellen.

1. Bearbeiten Sie die Seite **Events API**, indem Sie zu **AEM > Sites > WKND Mobile > Englisch > API** navigieren, die Seite **Events API** auswählen und in der oberen Aktionsleiste auf **Bearbeiten** tippen.
1. Fügen Sie ein **Logo-Bild** hinzu, das in der App angezeigt werden soll, indem Sie es per Drag-and-Drop aus der Asset-Suche auf den Bild-Komponenten-Platzhalter ziehen.
   * Verwenden Sie das unter `/content/dam/wknd-mobile/images/wknd-logo.png` bereitgestellte Logo.

1. Fügen Sie **Tag-Zeile** hinzu, um über den Ereignissen anzuzeigen.
   1. Bearbeiten Sie die Komponente **Text** .
   1. Geben Sie den Text ein:
      * `The WKND is here.`

1. Wählen Sie **events** aus, um Folgendes anzuzeigen:
   1. Legen Sie die folgende Konfiguration auf der Registerkarte **Eigenschaften** fest:
      * Modell: **Event**
      * Übergeordneter Pfad: **/content/dam/wknd-mobile/en/events**
      * Tags: **&lt;leer lassen>**
   1. Legen Sie die folgende Konfiguration auf der Registerkarte **Elemente** fest:
      * Entfernen Sie alle aufgelisteten Elemente, um sicherzustellen, dass ALLE Elemente der Event Content Fragments verfügbar gemacht werden.

>[!VIDEO](https://video.tv.adobe.com/v/28339/?quality=12&learn=on)

## Überprüfen der JSON-Ausgabe der API-Seite

Die JSON-Ausgabe und ihr Format können überprüft werden, indem die Seite mit dem Selektor `.model.json` angefordert wird.

Diese JSON-Struktur (oder das Schema) muss von den Nutzern dieser API gut verstanden werden. Es ist wichtig, dass API-Kunden verstehen, welche Aspekte der Struktur fest sind (d. h. das Logo (Bild) und das Tag live (Text) der Ereignis-API, die fließend sind (d. h. die Ereignisse, die unter Inhaltsfragmentlisten-Komponente aufgeführt sind).

Wenn Sie diesen Vertrag auf eine veröffentlichte API unterbrechen, kann dies zu einem fehlerhaften Verhalten in den verbrauchenden Apps führen.

1. Rufen Sie in neuen Browser-Registerkarten die Seiten der Events-API mit dem Selektor `.model.json` auf, der den JSON-Exporter AEM Content Services aufruft und die Seite und Komponenten in eine normalisierte, klar definierte JSON-Struktur serialisiert.

   Die von diesen Seiten erzeugte JSON-Struktur ist die Struktur, an die Apps ausgerichtet werden müssen.

1. Fordern Sie die Seite **Events API** als **JSON** an.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   Das Ergebnis sollte in etwa wie folgt aussehen:

![JSON-Ausgabe AEM Content Services](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Diese JSON kann in einer **tidy** (formatierten) Form ausgegeben werden, um für Menschen lesbar zu sein, indem die `.tidy` -Auswahl verwendet wird:
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## Nächster Schritt

Optional können Sie das Inhaltspaket [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) in der AEM-Autoreninstanz über [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp) installieren. Dieses Paket enthält die Konfigurationen und Inhalte, die in diesem und den vorherigen Kapiteln des Tutorials beschrieben werden.

* [Kapitel 6 - Veröffentlichung des Inhalts in AEM Publish as JSON](./chapter-6.md)

---
title: Kapitel 5 – Inhaltserstellung für Inhaltsdienstseiten – Inhaltsdienste
description: Kapitel 5 des AEM Headless-Tutorials behandelt die Erstellung der Seiten aus den in Kapitel 4 definierten Vorlagen. Diese Seiten fungieren als JSON-HTTP-Endpunkte.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 873d8e69-5a05-44ac-8dae-bba21f82b823
duration: 227
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '570'
ht-degree: 100%

---

# Kapitel 5 – Inhaltserstellung für Inhaltsdienstseiten

Kapitel 5 des AEM Headless-Tutorials behandelt die Erstellung der Seite aus den in Kapitel 4 definierten Vorlagen. Die in diesem Kapitel erstellte Seite dient als JSON-HTTP-Endpunkt für die Mobile App.

>[!NOTE]
>
> Die Seiteninhaltsarchitektur von `/content/wknd-mobile/en/api` wurde vorkonfiguriert. Die Basisseiten `en` und `api` dienen einem architektonischen und organisatorischen Zweck, sind aber nicht unbedingt erforderlich. Wenn API-Inhalte möglicherweise lokalisiert werden, empfiehlt es sich, die üblichen Best Practices für die Sprachkopie und die Multi-Site-Manager-Seitenorganisation zu befolgen, da die API-Seite wie jede andere AEM Sites-Seite lokalisiert werden kann.

## Erstellen der Ereignis-API-Seite

1. Navigieren Sie zu **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **Tippen Sie auf den Titel der API-Seite** und tippen Sie dann auf **Erstellen** in der oberen Aktionsleiste und erstellen Sie eine neue API-Seite für Ereignisse unter der API-Seite.
   1. Tippen Sie **Erstellen** in der oberen Aktionsleiste
   1. Wählen Sie die **Ereignis-API**-Vorlage aus
   1. In das Feld **Name** geben Sie **Ereignisse** ein.
   1. In das Feld **Titel** geben Sie **Ereignis-API** ein.
   1. Tippen Sie auf **Erstellen** in der oberen Aktionsleiste, um die Seite zu erstellen
   1. Tippen Sie auf **Fertig**, um zum AEM Sites-Administrator zurückzukehren

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## Bearbeiten der API-Seite für Ereignisse

>[!NOTE]
>
> Das Projekt bietet CSS, um einige grundlegende Stile für das Autorenerlebnis bereitzustellen.

1. Bearbeiten Sie die Seite **Ereignis-API**, indem Sie zu **AEM > Sites > WKND Mobile > Englisch > API** navigieren, die Seite **Ereignis-API** auswählen und in der oberen Aktionsleiste auf **Bearbeiten** tippen.
1. Fügen Sie ein **Logobild** hinzu, das in der Anwendung angezeigt werden soll, indem Sie es aus dem Asset-Finder auf den Platzhalter der Bildkomponente ziehen und ablegen.
   * Verwenden Sie das bereitgestellte Logo unter `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. Fügen Sie eine **Tag-Zeile** hinzu, die über den Ereignissen angezeigt wird.
   1. Bearbeiten Sie die **Text**-Komponente
   1. Geben Sie den Text ein:
      * `The WKND is here.`

1. Wählen Sie die **Ereignisse** aus, die angezeigt werden sollen:
   1. Stellen Sie die folgende Konfiguration auf der Registerkarte **Eigenschaften** ein:
      * Modell: **Ereignis**
      * Übergeordneter Pfad: **/content/dam/wknd-mobile/en/events**
      * Tags: **&lt;Leave blank>**
   1. Stellen Sie die folgende Konfiguration auf der Registerkarte **Elemente** ein:
      * Entfernen Sie alle aufgelisteten Elemente, um sicherzustellen, dass ALLE Elemente der Ereignis-Inhaltsfragmente verfügbar gemacht werden.

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## Überprüfen Sie die JSON-Ausgabe der API-Seite

Die JSON-Ausgabe und ihr Format können überprüft werden, indem die Seite mit dem Selektor `.model.json` angefordert wird.

Diese JSON-Struktur (oder das Schema) muss von den Nutzerinnen und Nutzern dieser API gut verstanden werden. Es ist wichtig, dass API-Nutzerinnen und -Nutzer verstehen, welche Aspekte der Struktur fest sind (d. h. das Logo (Bild) und das Tag live (Text) der Ereignis-API, die fließend sind (d. h. die Ereignisse, die unter Inhaltsfragmentlisten-Komponente aufgeführt sind).

Wenn Sie diesen Vertrag für eine veröffentlichte API brechen, kann dies zu einem fehlerhaften Verhalten in den konsumierenden Apps führen.

1. Fordern Sie in neuen Browser-Registerkarten die Seiten der Ereignis-API mit dem Selektor `.model.json` an, der den JSON-Exporter von AEM Content Services aufruft und die Seite und die Komponenten in eine normalisierte, klar definierte JSON-Struktur serialisiert.

   Die von diesen Seiten erzeugte JSON-Struktur ist die Struktur, an die Apps ausgerichtet werden müssen.

1. Anfordern der **Ereignis-API**-Seite als **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   Das Ergebnis sollte in etwa wie folgt aussehen:

![AEM Content Services JSON-Ausgabe](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Dieses JSON kann in einer **aufgeräumten** (formatierten) Weise für die menschliche Lesbarkeit ausgegeben werden, indem der Selektor `.tidy` verwendet wird:
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

## Nächster Schritt

Installieren Sie optional das Inhaltspaket [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) über [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp) in AEM Author. Dieses Paket enthält die in diesem und den vorangegangenen Kapiteln des Tutorials beschriebenen Konfigurationen und Inhalte.

* [Kapitel 6 – Bereitstellen des Inhalts in AEM Publish als JSON](./chapter-6.md)

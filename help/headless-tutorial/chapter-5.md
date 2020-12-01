---
title: Kapitel 5 - Authoring von Content Services-Seiten
description: In Kapitel 5 des AEM-Lernprogramms "Kopflos"werden die Seiten aus den in Kapitel 4 definierten Vorlagen erstellt. Diese Seiten fungieren als JSON HTTP-Endpunkte.
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '594'
ht-degree: 1%

---


# Kapitel 5 - Authoring von Content Services-Seiten

Kapitel 5 des AEMHeadless-Lernprogramms umfasst das Erstellen der Seite anhand der in Kapitel 4 definierten Vorlagen. Die in diesem Kapitel erstellte Seite fungiert als JSON-HTTP-Endpunkt für die mobile App.

>[!NOTE]
>
> Die Seiteninhaltsarchitektur von `/content/wknd-mobile/en/api` wurde vorab erstellt. Die Basisseiten von `en` und `api` dienen einem architektonischen und organisatorischen Zweck, sind jedoch nicht unbedingt erforderlich. Wenn API-Inhalte möglicherweise lokalisiert werden, sollten Sie sich an die üblichen Best Practices zur Sprachkopie und zum Multi-Site-Manager-Seitenaufbau halten, da die API-Seite wie jede andere AEM Sites-Seite lokalisiert werden kann.

## Erstellen der Ereignis-API-Seite

1. Navigieren Sie zu **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **Tippen Sie auf die Beschriftung der API-Seite** und dann auf die Schaltfläche  **** Erstellen in der oberen Aktionsleiste und erstellen Sie eine neue Ereignisses-API-Seite unter der API-Seite.
   1. Tippen Sie in der oberen Aktionsleiste auf **Erstellen**
   1. Vorlage für die Ereignisses-API **auswählen**
   1. Geben Sie im Feld **Name** **Ereignis** ein.
   1. Geben Sie im Feld **title** **Ereignisses API** ein.
   1. Tippen Sie auf **Erstellen** in der oberen Aktionsleiste, um die Seite zu erstellen.
   1. Tippen Sie auf **Fertig**, um zum AEM Sites-Administrator zurückzukehren.

>[!VIDEO](https://video.tv.adobe.com/v/28340/?quality=12&learn=on)

## Erstellen der API-Seite für Ereignis

>[!NOTE]
>
> Das Projekt stellt CSS bereit, um einige grundlegende Stile für die Autorenerfahrung bereitzustellen.

1. Bearbeiten Sie die Seite **Ereignisses-API**, indem Sie zu **AEM > Sites > WKND-Mobil > Englisch > API** navigieren, die Seite **Ereignisses-API** auswählen und auf **Bearbeiten** in der oberen Aktionsleiste tippen.
1. hinzufügen Sie ein **Logo-Bild**, das in der App angezeigt werden soll, indem Sie es per Drag &amp; Drop aus der Asset-Suche auf den Platzhalter für die Image-Komponente ziehen.
   * Verwenden Sie das angegebene Logo unter `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. hinzufügen **Tag-Zeile**, um über den Ereignissen anzuzeigen.
   1. Komponente **Text** bearbeiten
   1. Geben Sie den Text ein:
      * `The WKND is here.`

1. Wählen Sie die anzuzeigenden **Ereignis** aus:
   1. Legen Sie die folgende Konfiguration auf der Registerkarte **Eigenschaften** fest:
      * Modell: **Ereignis**
      * Übergeordneter Pfad: **/content/dam/wknd-mobile/en/Ereignisses**
      * Tags: **&lt;Leave blank>**
   1. Legen Sie die folgende Konfiguration auf der Registerkarte **Elemente** fest:
      * Entfernen Sie alle aufgelisteten Elemente, um sicherzustellen, dass ALLE Elemente der Inhaltsfragmente des Ereignisses verfügbar gemacht werden.

>[!VIDEO](https://video.tv.adobe.com/v/28339/?quality=12&learn=on)

## Überprüfen der JSON-Ausgabe der API-Seite

Die JSON-Ausgabe und ihr Format können überprüft werden, indem die Seite mit der Auswahl `.model.json` angefordert wird.

Diese JSON-Struktur (oder Schema) muss von den Nutzern dieser API gut verstanden werden. Es ist wichtig, dass Benutzer der API verstehen, welche Aspekte der Struktur fest sind (d.h. das Logo (Image) und das Tag live (Text) der Ereignis-APIs, die fließend sind (d.h. ereignis, die unter &quot;Inhaltsfragment-Liste&quot;aufgeführt sind).

Wenn Sie diesen Vertrag auf einer veröffentlichten API brechen, kann dies zu einem fehlerhaften Verhalten in den konsumierenden Apps führen.

1. Fordern Sie in neuen Browserregisterkarten die Ereignisses-API-Seiten mit der Auswahl `.model.json` an, die den JSON-Exporter AEM Content Services aufruft und die Seiten und Komponenten in eine normalisierte, genau definierte JSON-Struktur serialisiert.

   Die JSON-Struktur, die von diesen Seiten erzeugt wird, ist die Struktur, an die die Apps ausgerichtet werden müssen.

1. Fordern Sie die Seite **Ereignisses API** als **JSON** an.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   Das Ergebnis sollte wie folgt aussehen:

![AEM Content Services JSON-Ausgabe](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Dieses JSON kann in einer (formatierten) **Schreibweise**-Form für die Lesbarkeit von Menschen mithilfe der `.tidy`-Auswahl ausgegeben werden:
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## Nächster Schritt

Optional können Sie das Inhaltspaket [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) auf AEM Author über [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp) installieren. Dieses Paket enthält die Konfigurationen und Inhalte, die in diesem und den vorhergehenden Kapiteln des Tutorials beschrieben werden.

* [Kapitel 6 - Bereitstellen des Inhalts auf AEM Publish als JSON](./chapter-6.md)

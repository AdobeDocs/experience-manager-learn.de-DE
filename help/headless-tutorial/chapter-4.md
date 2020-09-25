---
title: Kapitel 4 - Definieren von Content Services-Vorlagen
seo-title: Erste Schritte mit AEM Kopflos - Kapitel 4 - Definieren von Content Services-Vorlagen
description: Kapitel 4 des AEM Headless-Lernprogramms behandelt die Rolle AEM bearbeitbarer Vorlagen im Kontext von AEM Content Services. Bearbeitbare Vorlagen werden verwendet, um die JSON-Inhaltsstruktur zu definieren, die Content Services letztendlich bereitstellt.
seo-description: Kapitel 4 des AEM Headless-Lernprogramms behandelt die Rolle AEM bearbeitbarer Vorlagen im Kontext von AEM Content Services. Bearbeitbare Vorlagen werden verwendet, um die JSON-Inhaltsstruktur zu definieren, die Content Services letztendlich bereitstellt.
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '835'
ht-degree: 1%

---


# Kapitel 4 - Definieren von Content Services-Vorlagen

Kapitel 4 des AEM Headless-Lernprogramms behandelt die Rolle AEM bearbeitbarer Vorlagen im Kontext von AEM Content Services. Bearbeitbare Vorlagen werden verwendet, um die JSON-Inhaltsstruktur zu definieren, die Content Services Clients über die Zusammensetzung der Content Services-aktivierten AEM bereitstellt.

## Die Rolle von Vorlagen in AEM Content Services

AEM bearbeitbare Vorlagen werden verwendet, um die HTTP-Endpunkte zu definieren, auf die zugegriffen wird, um den Ereignis-Inhalt als JSON verfügbar zu machen.

Traditionell AEM bearbeitbare Vorlagen werden zur Definition von Webseiten verwendet, aber diese Verwendung ist einfach eine Regel. Bearbeitbare Vorlagen können verwendet werden, um **beliebige** Inhaltsgruppen zu erstellen. wie auf diese Inhalte zugegriffen wird: als HTML in einem Browser, wie JSON von JavaScript (AEM SPA Editor) oder einer mobilen App genutzt wird, ist eine Funktion der Art und Weise, wie diese Seite angefordert wird.

In AEM Content Services werden bearbeitbare Vorlagen verwendet, um zu definieren, wie die JSON-Daten bereitgestellt werden.

Für die [!DNL WKND Mobile] App wird eine einzelne bearbeitbare Vorlage erstellt, die zur Steuerung eines einzelnen API-Endpunkts verwendet wird. Dieses Beispiel ist zwar einfach, um die Konzepte AEM Kopflosen zu illustrieren, Sie können jedoch mehrere Seiten (oder Endpunkte) erstellen, die jeweils unterschiedliche Inhaltssätze enthalten, um eine komplexere und besser organisierte API zu erstellen.

## Endpunkt der API

Um zu verstehen, wie Sie unseren API-Endpunkt zusammenstellen und zu verstehen, welche Inhalte unserer [!DNL WKND Mobile] App bereitgestellt werden sollten, sollten Sie das Design erneut aufrufen.

![API-Seitendekomposition von Ereignissen](./assets/chapter-4/design-to-component-mapping.png)

Wie wir sehen können, haben wir drei logische Inhaltssätze, die für die mobile App bereitgestellt werden sollen.

1. Das **Logo**
2. Die **Tag-Zeile**
3. Die Liste der **Ereignisse**

Dazu können wir diese Anforderungen AEM Komponenten zuordnen (und in unserem Fall WCM-Kernkomponenten AEM), um die erforderlichen Inhalte als JSON bereitzustellen.

1. Das **Logo** wird über eine **Bildkomponente angezeigt**
2. Die **Tag-Zeile** wird über eine **Textkomponente angezeigt**
3. Die Liste von **Ereignissen** wird über eine Komponente **der** Inhaltsfragment-Liste angezeigt, die wiederum einen Satz von Ereignis-Inhaltsfragmenten referenziert.

>[!NOTE]
>
>Zur Unterstützung AEM JSON-Exports von Seiten und Komponenten durch Content Service müssen die Seiten und Komponenten von AEM WCM-Kernkomponenten **abgeleitet werden**.
>
>[AEM WCM-Kernkomponenten](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) verfügen über integrierte Funktionen zur Unterstützung eines normalisierten JSON-Schemas exportierter Seiten und Komponenten. Alle in diesem Lernprogramm verwendeten WKND-Mobilkomponenten (Liste &quot;Seite&quot;, &quot;Bild&quot;, &quot;Text&quot;und &quot;Inhaltsfragment&quot;) stammen aus AEM WCM-Kernkomponenten.

## Definieren der Ereignisses-API-Vorlage

1. Öffnen Sie **[!UICONTROL Tools]>[!UICONTROL Allgemein]>[!UICONTROL Vorlagen]>[!DNL WKND Mobile]**.

1. Create the **[!DNL Events API]** template:

   1. Tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Erstellen]** .
   1. Select the **[!DNL WKND Mobile - Empty Page]** template
   1. Tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Weiter]** .
   1. Geben Sie **[!DNL Events API]** im Feld [!UICONTROL Vorlagentitel] ein
   1. Tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Erstellen]** .
   1. Tippen Sie auf Neue Vorlage zur Bearbeitung **[!UICONTROL öffnen]**

1. Zunächst erlauben wir die drei identifizierten AEM Komponenten, die wir zum Modellieren des Inhalts benötigen, indem wir die [!UICONTROL Richtlinien] des Root [!UICONTROL Layout Containers]bearbeiten. Vergewissern Sie sich, dass der **[!UICONTROL Strukturmodus]** aktiv ist, wählen Sie die Option aus **[!DNL Layout Container \[Root\]]** und klicken Sie auf die Schaltfläche &quot; **[!UICONTROL Richtlinie]** &quot;.
1. Suchen Sie unter **[!UICONTROL Eigenschaften]>[!UICONTROL Zulässige Komponenten]** nach **[!DNL WKND Mobile]**. Lassen Sie die folgenden Komponenten aus der [!DNL WKND Mobile] Komponentengruppe zu, damit sie auf der [!DNL Events] API-Seite verwendet werden können.

   * **[!DNL WKND Mobile > Image]**

      * Das Logo für die App
   * **[!DNL WKND Mobile > Text]**

      * Der einleitende Text der App
   * **[!DNL WKND Mobile > Content Fragment List]**

      * Die Liste der in der App verfügbaren Ereignis-Kategorien



1. Tippen Sie nach Abschluss auf das **[!UICONTROL Häkchen Fertig]** in der oberen rechten Ecke.
1. **Aktualisieren Sie** das Browserfenster, um die neue Liste [!UICONTROL Zulässige Komponenten] in der linken Leiste anzuzeigen.
1. Ziehen Sie aus der Komponentensuche in der linken Leiste die folgenden AEM Komponenten:
   1. **[!DNL Image]** für das Logo
   2. **[!DNL Text]** für die Tag-Zeile
   3. **[!DNL Content Fragment List]** für die Ereignisse
1. **Wählen Sie für jede der oben genannten Komponenten** diese aus und drücken Sie die **Entsperrungstaste** .
1. Vergewissern Sie sich jedoch, dass der **Layout-Container** **gesperrt** ist, um zu verhindern, dass andere Komponenten hinzugefügt oder diese drei Komponenten entfernt werden.
1. Tippen Sie in Admin **[!UICONTROL auf]Seiteninformationen[!UICONTROL >]** Ansicht, um zur Liste der [!DNL WKND Mobile] Vorlagen zurückzukehren. Wählen Sie die neu erstellte **[!DNL Events API]** Vorlage aus und tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Aktivieren]** .

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> Beachten Sie, dass die Komponenten, die zum Aufnehmen des Inhalts verwendet werden, der Vorlage selbst hinzugefügt und gesperrt werden. Dies soll Autoren die Möglichkeit geben, die vordefinierten Komponenten zu bearbeiten, jedoch nicht willkürlich Komponenten hinzuzufügen oder zu entfernen, da eine Änderung der API selbst die Annahmen um die JSON-Struktur umgehen und verbrauchende Apps umbrechen könnte. Alle APIs müssen stabil sein.

## Nächste Schritte

Optional können Sie das [Inhaltspaket com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) über [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)auf AEM Author installieren. Dieses Paket enthält die Konfigurationen und Inhalte, die in diesem und den vorhergehenden Kapiteln des Tutorials beschrieben werden.

* [Kapitel 5 - Authoring von Content Services-Seiten](./chapter-5.md)

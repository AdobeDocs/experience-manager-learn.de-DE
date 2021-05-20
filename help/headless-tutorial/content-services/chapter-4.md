---
title: Kapitel 4 - Definieren von Content Services-Vorlagen - Content Services
seo-title: Erste Schritte mit AEM Headless - Kapitel 4 - Definieren von Content Services-Vorlagen
description: Kapitel 4 des Tutorials AEM Headless behandelt die Rolle AEM bearbeitbaren Vorlagen im Kontext von AEM Content Services. Bearbeitbare Vorlagen werden verwendet, um die JSON-Inhaltsstruktur zu definieren, die Content Services letztendlich bereitstellen wird.
seo-description: Kapitel 4 des Tutorials AEM Headless behandelt die Rolle AEM bearbeitbaren Vorlagen im Kontext von AEM Content Services. Bearbeitbare Vorlagen werden verwendet, um die JSON-Inhaltsstruktur zu definieren, die Content Services letztendlich bereitstellen wird.
feature: Inhaltsfragmente, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 1%

---


# Kapitel 4 - Definieren von Content Services-Vorlagen

Kapitel 4 des Tutorials AEM Headless behandelt die Rolle AEM bearbeitbaren Vorlagen im Kontext von AEM Content Services. Bearbeitbare Vorlagen werden verwendet, um die JSON-Inhaltsstruktur zu definieren, AEM Content Services Clients über die Zusammensetzung von Content Services-aktivierten AEM Komponenten bereitstellt.

## Die Rolle von Vorlagen in AEM Content Services

AEM bearbeitbare Vorlagen werden verwendet, um die HTTP-Endpunkte zu definieren, auf die zugegriffen wird, um den Ereignisinhalt als JSON verfügbar zu machen.

Traditionell AEM bearbeitbare Vorlagen werden zur Definition von Webseiten verwendet, doch diese Verwendung ist einfach eine Konvention. Bearbeitbare Vorlagen können verwendet werden, um **beliebige** Inhaltssätze zu erstellen. wie auf diesen Inhalt zugegriffen wird: als HTML in einem Browser, da JSON von JavaScript (AEM-SPA-Editor) oder einer Mobile App genutzt wird, eine Funktion der Art und Weise ist, wie diese Seite angefordert wird.

In AEM Content Services werden bearbeitbare Vorlagen verwendet, um festzulegen, wie die JSON-Daten bereitgestellt werden.

Für die [!DNL WKND Mobile]-App erstellen wir eine einzelne bearbeitbare Vorlage, die zur Steuerung eines einzelnen API-Endpunkts verwendet wird. Dieses Beispiel ist zwar einfach, die Konzepte von Headless zu veranschaulichen, doch können Sie mehrere Seiten (oder Endpunkte) erstellen, die jeweils verschiedene Inhaltssätze enthalten, um eine komplexere und besser organisierte API zu erstellen.

## Grundlegendes zum API-Endpunkt

Um zu verstehen, wie Sie unseren API-Endpunkt erstellen und welche Inhalte für unsere [!DNL WKND Mobile]-App verfügbar gemacht werden sollen, sollten wir das Design erneut aufrufen.

![API-Seitenerkennung für Ereignisse](./assets/chapter-4/design-to-component-mapping.png)

Wie wir sehen können, müssen wir drei logische Inhaltssätze für die mobile App bereitstellen.

1. **Logo**
2. **Tag Line**
3. Die Liste der **Ereignisse**

Dazu können wir diese Anforderungen AEM Komponenten zuordnen (und in unserem Fall AEM WCM-Kernkomponenten), um den erforderlichen Inhalt als JSON verfügbar zu machen.

1. Das **Logo** wird über eine **Bildkomponente** angezeigt.
2. Die **Tag-Zeile** wird über eine **Text-Komponente** angezeigt.
3. Die Liste der **Ereignisse** wird über eine **Inhaltsfragmentlisten-Komponente** angezeigt, die wiederum auf eine Reihe von Ereignis-Inhaltsfragmenten verweist.

>[!NOTE]
>
>Um den JSON-Export von Seiten und Komponenten AEM Content Service zu unterstützen, müssen die Seiten und Komponenten **von AEM WCM-Kernkomponenten** abgeleitet werden.
>
>[AEM WCM-](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) Kernkomponenten verfügen über integrierte Funktionen zur Unterstützung eines normalisierten JSON-Schemas exportierter Seiten und Komponenten. Alle in diesem Tutorial verwendeten WKND Mobile-Komponenten (Seite, Bild, Text und Inhaltsfragmentliste) werden von AEM WCM-Kernkomponenten abgeleitet.

## Definieren der Ereignis-API-Vorlage

1. Öffnen Sie **[!UICONTROL Tools] > [!UICONTROL Allgemein] > [!UICONTROL Vorlagen] >[!DNL WKND Mobile]**.

1. Erstellen Sie die Vorlage **[!DNL Events API]** :

   1. Tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Erstellen]** .
   1. Wählen Sie die Vorlage **[!DNL WKND Mobile - Empty Page]** aus.
   1. Tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Weiter]** .
   1. Geben Sie **[!DNL Events API]** in das Feld [!UICONTROL Vorlagentitel] ein.
   1. Tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Erstellen]** .
   1. Tippen Sie auf **[!UICONTROL Öffnen]** Öffnen Sie die neue Vorlage zum Bearbeiten

1. Zuerst lassen wir die drei identifizierten AEM Komponenten zu, die wir zum Modellieren des Inhalts benötigen, indem wir die [!UICONTROL Richtlinie] des Stammordners [!UICONTROL Layout-Container] bearbeiten. Stellen Sie sicher, dass der Modus **[!UICONTROL Struktur]** aktiv ist, wählen Sie **[!DNL Layout Container \[Root\]]** und tippen Sie auf die Schaltfläche **[!UICONTROL Richtlinie]**.
1. Suchen Sie unter **[!UICONTROL Properties] > [!UICONTROL Allowed Components]** nach **[!DNL WKND Mobile]**. Lassen Sie die folgenden Komponenten aus der Komponentengruppe [!DNL WKND Mobile] zu, damit sie auf der API-Seite [!DNL Events] verwendet werden können.

   * **[!DNL WKND Mobile > Image]**

      * Das Logo für die App
   * **[!DNL WKND Mobile > Text]**

      * Der Einführungstext der App
   * **[!DNL WKND Mobile > Content Fragment List]**

      * Die Liste der Ereigniskategorien, die in der App angezeigt werden können



1. Tippen Sie auf das Häkchen **[!UICONTROL Fertig]** in der oberen rechten Ecke, wenn Sie fertig sind.
1. **** Aktualisieren Sie das Browser-Fenster, um in der linken Leiste die Liste der neu  [!UICONTROL zulässigen ] Komponenten anzuzeigen.
1. Ziehen Sie aus der Komponentensuche in der linken Leiste die folgenden AEM Komponenten:
   1. **[!DNL Image]** für das Logo
   2. **[!DNL Text]** für die Tag-Zeile
   3. **[!DNL Content Fragment List]** für die Ereignisse
1. **Wählen Sie für jede der oben genannten Komponenten** diese aus und drücken Sie die  **** Entsperrungstaste.
1. Stellen Sie jedoch sicher, dass der **Layout-Container** **locked** ist, um zu verhindern, dass andere Komponenten hinzugefügt oder diese drei Komponenten entfernt werden.
1. Tippen Sie auf **[!UICONTROL Seiteninformationen] > [!UICONTROL In Admin anzeigen]** , um zur Vorlagenliste [!DNL WKND Mobile] zurückzukehren. Wählen Sie die neu erstellte Vorlage **[!DNL Events API]** aus und tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Aktivieren]** .

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> Beachten Sie, dass die Komponenten, die zum Aufdecken des Inhalts verwendet werden, der Vorlage selbst hinzugefügt und gesperrt werden. Auf diese Weise können Autoren die vordefinierten Komponenten bearbeiten, jedoch nicht beliebig Komponenten hinzufügen oder entfernen, da eine Änderung der API selbst die Annahmen rund um die JSON-Struktur beschädigen und verbrauchende Apps beschädigen könnte. Alle APIs müssen stabil sein.

## Nächste Schritte

Optional können Sie das Inhaltspaket [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) in der AEM-Autoreninstanz über [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp) installieren. Dieses Paket enthält die Konfigurationen und Inhalte, die in diesem und den vorherigen Kapiteln des Tutorials beschrieben werden.

* [Kapitel 5 - Inhaltserstellung für Seiten in Content Services](./chapter-5.md)

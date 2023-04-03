---
title: Kapitel 4 - Definieren von Content Services-Vorlagen - Content Services
description: Kapitel 4 des Tutorials AEM Headless behandelt die Rolle AEM bearbeitbaren Vorlagen im Kontext von AEM Content Services. Bearbeitbare Vorlagen werden verwendet, um die JSON-Inhaltsstruktur zu definieren, AEM Content Services letztendlich verfügbar macht.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 2%

---

# Kapitel 4 - Definieren von Content Services-Vorlagen

Kapitel 4 des Tutorials AEM Headless behandelt die Rolle AEM bearbeitbaren Vorlagen im Kontext von AEM Content Services. Bearbeitbare Vorlagen werden verwendet, um die JSON-Inhaltsstruktur zu definieren, AEM Content Services Clients über die Zusammensetzung von Content Services-aktivierten AEM Komponenten bereitstellt.

## Die Rolle von Vorlagen in AEM Content Services

AEM bearbeitbare Vorlagen werden verwendet, um die HTTP-Endpunkte zu definieren, auf die zugegriffen wird, um den Ereignisinhalt als JSON verfügbar zu machen.

Traditionell AEM bearbeitbare Vorlagen werden zur Definition von Webseiten verwendet, doch diese Verwendung ist einfach eine Konvention. Bearbeitbare Vorlagen können zum Erstellen verwendet werden **any** Inhaltsmenge; wie auf diesen Inhalt zugegriffen wird: als HTML in einem Browser, da JSON von JavaScript (AEM-SPA-Editor) oder einer Mobile App genutzt wird, eine Funktion der Art und Weise ist, wie diese Seite angefordert wird.

In AEM Content Services werden bearbeitbare Vorlagen verwendet, um festzulegen, wie die JSON-Daten bereitgestellt werden.

Für [!DNL WKND Mobile] App erstellen wir eine einzelne bearbeitbare Vorlage, die zur Steuerung eines einzelnen API-Endpunkts verwendet wird. Dieses Beispiel ist zwar einfach, die Konzepte von Headless zu veranschaulichen, doch können Sie mehrere Seiten (oder Endpunkte) erstellen, die jeweils verschiedene Inhaltssätze enthalten, um eine komplexere und besser organisierte API zu erstellen.

## Grundlegendes zum API-Endpunkt

So erstellen Sie unseren API-Endpunkt und verstehen, welche Inhalte für unsere [!DNL WKND Mobile] App, lassen Sie uns das Design erneut besuchen.

![API-Seitenerkennung für Ereignisse](./assets/chapter-4/design-to-component-mapping.png)

Wie wir sehen können, müssen wir drei logische Inhaltssätze für die mobile App bereitstellen.

1. Die **Logo**
2. Die **Tag Line**
3. Die Liste der **Veranstaltungen**

Dazu können wir diese Anforderungen AEM Komponenten zuordnen (und in unserem Fall AEM WCM-Kernkomponenten), um den erforderlichen Inhalt als JSON verfügbar zu machen.

1. Die **Logo** über eine **Bildkomponente**
2. Die **Tag Line** über eine **Textkomponente**
3. Die Liste der **Veranstaltungen** über eine **Inhaltsfragmentlisten-Komponente** die wiederum einen Satz von Event-Inhaltsfragmenten referenziert.

>[!NOTE]
>
>Um den JSON-Export von Seiten und Komponenten durch AEM Content Service zu unterstützen, müssen die Seiten und Komponenten **Ableiten von AEM WCM-Kernkomponenten**.
>
>[AEM WCM-Kernkomponenten](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) verfügt über integrierte Funktionen zur Unterstützung eines normalisierten JSON-Schemas exportierter Seiten und Komponenten. Alle in diesem Tutorial verwendeten WKND Mobile-Komponenten (Seite, Bild, Text und Inhaltsfragmentliste) werden von AEM WCM-Kernkomponenten abgeleitet.

## Definieren der Ereignis-API-Vorlage

1. Öffnen Sie **[!UICONTROL Tools] > [!UICONTROL Allgemein] > [!UICONTROL Vorlagen] >[!DNL WKND Mobile]**.

1. Erstellen Sie die **[!DNL Events API]** template:

   1. Tippen **[!UICONTROL Erstellen]** in der oberen Aktionsleiste
   1. Wählen Sie die **[!DNL WKND Mobile - Empty Page]** template
   1. Tippen **[!UICONTROL Nächste]** in der oberen Aktionsleiste
   1. Eingabe **[!DNL Events API]** im [!UICONTROL Vorlagentitel] field
   1. Tippen **[!UICONTROL Erstellen]** in der oberen Aktionsleiste
   1. Tippen **[!UICONTROL Öffnen]** Öffnen Sie die neue Vorlage zur Bearbeitung

1. Zunächst erlauben wir es den drei identifizierten AEM Komponenten, die wir benötigen, den Inhalt zu modellieren, indem wir die [!UICONTROL Politik] des Stamms [!UICONTROL Layout-Container]. Stellen Sie sicher, dass **[!UICONTROL Struktur]** Modus aktiv ist, wählen Sie die **[!DNL Layout Container \[Root\]]** und tippen Sie auf **[!UICONTROL Politik]** Schaltfläche.
1. under **[!UICONTROL Eigenschaften] > [!UICONTROL Zugelassene Komponenten]** Suchen nach **[!DNL WKND Mobile]**. Zulassen der folgenden Komponenten aus der [!DNL WKND Mobile] Komponentengruppe, damit sie für die [!DNL Events] API-Seite.

   * **[!DNL WKND Mobile > Image]**

      * Das Logo für die App
   * **[!DNL WKND Mobile > Text]**

      * Der Einführungstext der App
   * **[!DNL WKND Mobile > Content Fragment List]**

      * Die Liste der Ereigniskategorien, die in der App angezeigt werden können



1. Tippen Sie auf **[!UICONTROL Fertig]** Häkchen in der oberen rechten Ecke nach Abschluss.
1. **Aktualisieren** das Browserfenster, um neu zu sehen [!UICONTROL Zugelassene Komponenten] in der linken Leiste.
1. Ziehen Sie aus der Komponentensuche in der linken Leiste die folgenden AEM Komponenten:
   1. **[!DNL Image]** für das Logo
   2. **[!DNL Text]** für die Tag-Zeile
   3. **[!DNL Content Fragment List]** für die Ereignisse
1. **Für jede der oben genannten Komponenten**, wählen Sie sie aus und drücken Sie die **unlock** Schaltfläche.
1. Stellen Sie jedoch sicher, dass die **Layout-Container** is **locked** um zu verhindern, dass andere Komponenten hinzugefügt oder diese drei Komponenten entfernt werden.
1. Tippen **[!UICONTROL Seiteninformationen] > [!UICONTROL In Admin anzeigen]** , um zur [!DNL WKND Mobile] Vorlagenliste. Wählen Sie die neu erstellte **[!DNL Events API]** Vorlage und tippen Sie auf **[!UICONTROL Aktivieren]** in der oberen Aktionsleiste.

>[!VIDEO](https://video.tv.adobe.com/v/28342?quality=12&learn=on)

>[!NOTE]
>
> Beachten Sie, dass die Komponenten, die zum Aufdecken des Inhalts verwendet werden, der Vorlage selbst hinzugefügt und gesperrt werden. Auf diese Weise können Autoren die vordefinierten Komponenten bearbeiten, jedoch nicht beliebig Komponenten hinzufügen oder entfernen, da eine Änderung der API selbst die Annahmen rund um die JSON-Struktur beschädigen und verbrauchende Apps beschädigen könnte. Alle APIs müssen stabil sein.

## Nächste Schritte

Installieren Sie optional die [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) Inhaltspaket in der AEM-Autoreninstanz über [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp). Dieses Paket enthält die Konfigurationen und Inhalte, die in diesem und den vorherigen Kapiteln des Tutorials beschrieben werden.

* [Kapitel 5 - Inhaltserstellung für Seiten in Content Services](./chapter-5.md)

---
title: Verfassen von Inhalten und Veröffentlichen von Änderungen
seo-title: Erste Schritte mit AEM Sites - Änderungen an Autoreninhalten und Veröffentlichungen
description: Verwenden Sie AEM den Seiteneditor in Adobe Experience Manager, um den Inhalt der Website zu aktualisieren. Erfahren Sie, wie Komponenten zur Erleichterung der Bearbeitung verwendet werden. Machen Sie sich mit dem Unterschied zwischen einer AEM-Autoren- und Veröffentlichungsumgebung vertraut und erfahren Sie, wie Sie Änderungen an der Live-Site veröffentlichen.
sub-product: Sites
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Kernkomponenten, Seiten-Editor
role: Developer
level: Beginner
kt: 7497
thumbnail: KT-7497.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '1306'
ht-degree: 3%

---


# Inhaltserstellung und Veröffentlichungsänderungen {#author-content-publish}

>[!CAUTION]
>
> Die hier vorgestellten Funktionen zur schnellen Site-Erstellung werden im zweiten Halbjahr 2021 veröffentlicht. Die zugehörige Dokumentation steht für Vorschauzwecke zur Verfügung.

Es ist wichtig zu verstehen, wie ein Benutzer Inhalte für die Website aktualisiert. In diesem Kapitel nehmen wir die Rolle eines **Inhaltsautors** an und nehmen einige redaktionelle Aktualisierungen an der im vorherigen Kapitel erstellten Site vor. Am Ende des Kapitels veröffentlichen wir die Änderungen, um zu verstehen, wie die Live-Site aktualisiert wird.

## Voraussetzungen {#prerequisites}

Dies ist ein mehrteiliges Tutorial, und es wird davon ausgegangen, dass die im Kapitel [Erstellen einer Site](./create-site.md) beschriebenen Schritte abgeschlossen sind.

## Vorgabe {#objective}

1. Machen Sie sich mit den Konzepten von **Seiten** und **Komponenten** in AEM Sites vertraut.
1. Erfahren Sie, wie Sie den Inhalt der Website aktualisieren.
1. Erfahren Sie, wie Sie Änderungen an der Live-Site veröffentlichen.

## Neue Seite {#create-page} erstellen

Eine Website ist in der Regel in Seiten unterteilt, um ein mehrseitiges Erlebnis zu schaffen. AEM strukturiert Inhalte auf die gleiche Weise. Als Nächstes erstellen Sie eine neue Seite für die Site.

1. Melden Sie sich beim AEM **Author** Service an, der im vorherigen Kapitel verwendet wurde.
1. Klicken Sie im AEM Startbildschirm auf **Sites** > **WKND Site** > **Englisch** > **Artikel**
1. Klicken Sie in der oberen rechten Ecke auf **Erstellen** > **Seite**.

   ![Seite erstellen](assets/author-content-publish/create-page-button.png)

   Dadurch wird der Assistent **Seite erstellen** angezeigt.

1. Wählen Sie die Vorlage **Artikelseite** aus und klicken Sie auf **Weiter**.

   Seiten in AEM werden basierend auf einer Seitenvorlage erstellt. Seitenvorlagen werden im Kapitel [Seitenvorlagen](page-templates.md) genauer untersucht.

1. Geben Sie unter **Properties** einen **Titel** von &quot;Hello World&quot;ein.
1. Legen Sie **Name** auf `hello-world` fest und klicken Sie auf **Erstellen**.

   ![Eigenschaften der Anfangsseite](assets/author-content-publish/initial-page-properties.png)

1. Klicken Sie im Popup-Dialogfeld auf **Öffnen** , um die neu erstellte Seite zu öffnen.

## Erstellen einer Komponente {#author-component}

AEM Komponenten können als kleine modulare Bausteine einer Webseite betrachtet werden. Indem die Benutzeroberfläche in logische Abschnitte oder Komponenten unterteilt wird, wird die Verwaltung viel einfacher. Um Komponenten wiederzuverwenden, müssen die Komponenten konfigurierbar sein. Dies erfolgt über das Dialogfeld &quot;Autor&quot;.

AEM stellt einen Satz von [Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de) bereit, die produktionsbereit sind. Die **Kernkomponenten** reichen von grundlegenden Elementen wie [Text](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) und [Bild](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) bis hin zu komplexeren Benutzeroberflächenelementen wie einem [Karussell](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html).

Als Nächstes erstellen wir einige Komponenten mit AEM Seiteneditor.

1. Navigieren Sie zur Seite **Hello World** , die in der vorherigen Übung erstellt wurde.
1. Stellen Sie sicher, dass Sie sich im Modus **Bearbeiten** befinden und klicken Sie in der linken Seitenleiste auf das Symbol **Komponenten** .

   ![Seitenleiste des Seiteneditors des Seiteneditors](assets/author-content-publish/page-editor-siderail.png)

   Dadurch wird die Komponentenbibliothek geöffnet und die verfügbaren Komponenten aufgelistet, die auf der Seite verwendet werden können.

1. Scrollen Sie nach unten und **Ziehen Sie die Komponente** a **Text (v2)** in den bearbeitbaren Hauptbereich der Seite.

   ![Drag-and-Drop-Textkomponente](assets/author-content-publish/drag-drop-text-cmp.png)

1. Klicken Sie auf die Komponente **Text**, um sie zu markieren, und klicken Sie dann auf das Symbol **Schraubenschlüssel** ![Schraubenschlüsselsymbol](assets/author-content-publish/wrench-icon.png), um das Dialogfeld der Komponente zu öffnen. Geben Sie Text ein und speichern Sie die Änderungen im Dialogfeld.

   ![Rich-Text-Komponente](assets/author-content-publish/rich-text-populated-component.png)

   Die Komponente **Text** sollte jetzt den Rich-Text auf der Seite anzeigen.

1. Wiederholen Sie die obigen Schritte, bis auf eine Instanz der Komponente **Image(v2)** auf die Seite ziehen. Öffnen Sie das Dialogfeld der Komponente **Bild** .

1. Wechseln Sie in der linken Leiste zum **Asset Finder**, indem Sie auf das Symbol **Assets** ![Asset-Symbol](assets/author-content-publish/asset-icon.png) klicken.
1. **Ziehen Sie** ein Bild per Drag-and-Drop in das Dialogfeld der Komponente und klicken Sie auf  **** Weiter , um die Änderungen zu speichern.

   ![Asset zum Dialogfeld hinzufügen](assets/author-content-publish/add-asset-dialog.png)

1. Beachten Sie, dass auf der Seite Komponenten wie **Titel**, **Navigation**, **Suche** vorhanden sind, die korrigiert sind. Diese Bereiche werden als Teil der Seitenvorlage konfiguriert und können nicht auf einer einzelnen Seite geändert werden. Dies wird im nächsten Kapitel näher erläutert.

Experimentieren Sie mit einigen der anderen Komponenten. Die Dokumentation zu den einzelnen [Kernkomponenten finden Sie hier](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html). Eine detaillierte Videoserie zu [Seitenbearbeitung finden Sie hier](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html).

## Veröffentlichungsaktualisierungen {#publish-updates}

AEM Umgebungen sind auf einen **Autorendienst** und einen **Veröffentlichungsdienst** aufgeteilt. In diesem Kapitel haben wir mehrere Änderungen an der Site auf der **Autorendienst** vorgenommen. Damit Besucher der Site die Änderungen anzeigen können, müssen wir sie im **Veröffentlichungsdienst** veröffentlichen.

![Allgemeine Abbildung](assets/author-content-publish/author-publish-high-level-flow.png)

*Übergeordneter Inhaltsfluss von der Autoren- zur Veröffentlichungsinstanz*

**1.** Inhaltsautoren nehmen Aktualisierungen am Site-Inhalt vor. Die Updates können als Vorschau angezeigt, geprüft und genehmigt werden, um live geschaltet zu werden.

**2.** Der Inhalt ist veröffentlicht. Die Veröffentlichung kann bei Bedarf erfolgen oder für ein künftiges Datum geplant werden.

**3.** Besucher der Site sehen die Änderungen, die im Veröffentlichungsdienst übernommen werden.

### Veröffentlichen der Änderungen

Als Nächstes veröffentlichen wir die Änderungen.

1. Navigieren Sie im Bildschirm AEM Start zu **Sites** und wählen Sie die **WKND-Site** aus.
1. Klicken Sie in der Menüleiste auf **Veröffentlichung verwalten** .

   ![Veröffentlichung verwalten](assets/author-content-publish/click-manage-publiciation.png)

   Da es sich um eine brandneue Site handelt, möchten wir alle Seiten veröffentlichen und können den Assistenten &quot;Veröffentlichung verwalten&quot;verwenden, um genau zu definieren, was veröffentlicht werden muss.

1. Lassen Sie unter **Options** die Standardeinstellungen zu **Publish** und planen Sie sie für **Now**. Klicken Sie auf **Weiter**.
1. Wählen Sie unter **Scope** die **WKND-Site** aus und klicken Sie auf **Untergeordnete Elemente einschließen**. Deaktivieren Sie im Dialogfeld alle Kontrollkästchen. Wir möchten die vollständige Website veröffentlichen.

   ![Veröffentlichungsumfang aktualisieren](assets/author-content-publish/update-scope-publish.png)

1. Klicken Sie auf die Schaltfläche **Veröffentlichte Verweise** . Überprüfen Sie im Dialogfeld, ob alles aktiviert ist. Dazu gehören die **Grundlegende AEM Site-Vorlage** und mehrere Konfigurationen, die von der Site-Vorlage generiert wurden. Klicken Sie zum Aktualisieren auf **Fertig** .

   ![Veröffentlichen von Verweisen](assets/author-content-publish/publish-references.png)

1. Klicken Sie abschließend in der oberen rechten Ecke auf **Publish** , um den Inhalt zu veröffentlichen.

## Veröffentlichte Inhalte anzeigen {#publish}

Navigieren Sie anschließend zum Veröffentlichungsdienst , um die Änderungen anzuzeigen.

1. Eine einfache Möglichkeit, die URL des Veröffentlichungsdienstes abzurufen, besteht darin, die Autoren-URL zu kopieren und das `author`-Wort durch `publish` zu ersetzen. Beispiel:

   * **Autoren-URL** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **Veröffentlichungs-URL**  -  `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. Fügen Sie `/content/wknd.html` zur Veröffentlichungs-URL hinzu, damit die endgültige URL wie folgt aussieht: `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`.

   >[!NOTE]
   >
   > Ändern Sie `wknd.html` so, dass es mit dem Namen Ihrer Site übereinstimmt, wenn Sie bei der [Site-Erstellung](create-site.md) einen eindeutigen Namen angegeben haben.

1. Wenn Sie zur Veröffentlichungs-URL navigieren, sollte die Site ohne AEM Authoring-Funktion angezeigt werden.

   ![Veröffentlichte Site](assets/author-content-publish/publish-url-update.png)

1. Klicken Sie im Menü **Navigation** auf **Artikel** > **Hallo Welt** , um zur zuvor erstellten Seite &quot;Hello World&quot;zu navigieren.
1. Kehren Sie zum **AEM-Autorendienst** zurück und nehmen Sie einige zusätzliche Inhaltsänderungen im Seiteneditor vor.
1. Veröffentlichen Sie diese Änderungen direkt im Seiteneditor, indem Sie auf das Symbol **Seiteneigenschaften** > **Seite veröffentlichen** klicken.

   ![Publish direct](assets/author-content-publish/page-editor-publish.png)

1. Kehren Sie zum **AEM Publish Service** zurück, um die Änderungen anzuzeigen. Wahrscheinlich werden **nicht** sofort die Updates sehen. Dies liegt daran, dass der **AEM Publish Service** [Caching über einen Apache-Webserver und CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html) umfasst. Standardmäßig wird HTML-Inhalt ca. 5 Minuten lang zwischengespeichert.

1. Um den Cache zum Testen/Debuggen zu umgehen, fügen Sie einfach einen Abfrageparameter wie `?nocache=true` hinzu. Die URL würde wie `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true` aussehen. Weitere Informationen zur Cachestrategie und den verfügbaren Konfigurationen [finden Sie hier](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html).

1. Sie finden auch die URL zum Veröffentlichungsdienst in Cloud Manager. Navigieren Sie zum **Cloud Manager-Programm** > **Umgebungen** > **Umgebung**.

   ![Veröffentlichungsdienst anzeigen](assets/author-content-publish/view-environment-segments.png)

   Unter **Umgebungssegmente** finden Sie Links zu den Diensten **Autor** und **Veröffentlichen**.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gerade Änderungen an Ihrer AEM Site verfasst und veröffentlicht!

### Nächste Schritte {#next-steps}

Erfahren Sie, wie Sie [Seitenvorlagen](./page-templates.md) erstellen und ändern. Machen Sie sich mit der Beziehung zwischen einer Seitenvorlage und einer Seite vertraut. Erfahren Sie, wie Sie Richtlinien einer Seitenvorlage konfigurieren, um granulare Governance und Markenkonsistenz für Inhalte bereitzustellen.  Eine gut strukturierte Zeitschriftenartikelvorlage wird auf der Grundlage eines Mockups aus Adobe XD erstellt.

---
title: Einführung in das Authoring und Publishing | AEM SchnellSite-Erstellung
description: Verwenden Sie AEM den Seiteneditor in Adobe Experience Manager, um den Inhalt der Website zu aktualisieren. Erfahren Sie, wie Komponenten zur Erleichterung der Bearbeitung verwendet werden. Machen Sie sich mit dem Unterschied zwischen einer AEM-Autoren- und Veröffentlichungsumgebung vertraut und erfahren Sie, wie Sie Änderungen an der Live-Site veröffentlichen.
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
kt: 7497
thumbnail: KT-7497.jpg
exl-id: 17ca57d1-2b9a-409c-b083-398d38cd6a19
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1330'
ht-degree: 4%

---

# Einführung in das Authoring und Publishing {#author-content-publish}

Es ist wichtig zu verstehen, wie ein Benutzer Inhalte für die Website aktualisiert. In diesem Kapitel werden wir die Rolle eines **Inhaltsautor** und nehmen einige redaktionelle Aktualisierungen an der im vorherigen Kapitel erstellten Site vor. Am Ende des Kapitels veröffentlichen wir die Änderungen, um zu verstehen, wie die Live-Site aktualisiert wird.

## Voraussetzungen {#prerequisites}

Dies ist ein mehrteiliges Tutorial, und es wird davon ausgegangen, dass die im [Erstellen einer Site](./create-site.md) -Kapitel abgeschlossen.

## Ziel {#objective}

1. Grundlegendes zu den Konzepten von **Seiten** und **Komponenten** in AEM Sites.
1. Erfahren Sie, wie Sie den Inhalt der Website aktualisieren.
1. Erfahren Sie, wie Sie Änderungen an der Live-Site veröffentlichen.

## Neue Seite erstellen {#create-page}

Eine Website ist in der Regel in Seiten unterteilt, um ein mehrseitiges Erlebnis zu schaffen. AEM strukturiert Inhalte auf die gleiche Weise. Als Nächstes erstellen Sie eine neue Seite für die Site.

1. Bei der AEM anmelden **Autor** Im vorherigen Kapitel verwendeter Dienst.
1. Klicken Sie im Bildschirm AEM Start auf **Sites** > **WKND-Site** > **englisch** > **Artikel**
1. Klicken Sie in der oberen rechten Ecke auf **Erstellen** > **Seite**.

   ![Seite erstellen](assets/author-content-publish/create-page-button.png)

   Daraus ergeben sich die **Seite erstellen** Assistent.

1. Wählen Sie die **Artikelseite** Vorlage und klicken Sie auf **Nächste**.

   Seiten in AEM werden basierend auf einer Seitenvorlage erstellt. Seitenvorlagen werden im Abschnitt [Seitenvorlagen](page-templates.md) Kapitel.

1. under **Eigenschaften** einen **Titel** von &quot;Hello World&quot;.
1. Legen Sie die **Name** zu `hello-world` und klicken Sie auf **Erstellen**.

   ![Eigenschaften der Anfangsseite](assets/author-content-publish/initial-page-properties.png)

1. Klicken Sie im Popup-Dialogfeld auf **Öffnen** , um die neu erstellte Seite zu öffnen.

## Erstellen einer Komponente {#author-component}

AEM Komponenten können als kleine modulare Bausteine einer Webseite betrachtet werden. Indem die Benutzeroberfläche in logische Abschnitte oder Komponenten unterteilt wird, wird die Verwaltung viel einfacher. Um Komponenten wiederzuverwenden, müssen die Komponenten konfigurierbar sein. Dies erfolgt über das Dialogfeld &quot;Autor&quot;.

AEM bietet eine Reihe von [Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de) die einsatzbereit sind. Die **Kernkomponenten** aus grundlegenden Elementen wie [Text](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html?lang=de) und [Bild](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=de) zu komplexeren UI-Elementen wie [Karussell](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html?lang=de).

Als Nächstes erstellen Sie einige Komponenten mit dem AEM Seiteneditor.

1. Navigieren Sie zum **Hello World** Seite, die in der vorherigen Übung erstellt wurde.
1. Stellen Sie sicher, dass Sie sich in **Bearbeiten** und klicken Sie in der linken Seitenleiste auf die **Komponenten** Symbol.

   ![Seitenleiste des Seiteneditors des Seiteneditors](assets/author-content-publish/page-editor-siderail.png)

   Dadurch wird die Komponentenbibliothek geöffnet und die verfügbaren Komponenten aufgelistet, die auf der Seite verwendet werden können.

1. Scrollen Sie nach unten und **Drag &amp; Drop** a **Text (v2)** -Komponente in den Haupt-bearbeitbaren Bereich der Seite.

   ![Drag-and-Drop-Textkomponente](assets/author-content-publish/drag-drop-text-cmp.png)

1. Klicken Sie auf **Text** -Komponente markieren und dann auf **Schraubenschlüssel** icon ![Schraubensymbol](assets/author-content-publish/wrench-icon.png) , um das Dialogfeld der Komponente zu öffnen. Geben Sie Text ein und speichern Sie die Änderungen im Dialogfeld.

   ![Rich-Text-Komponente](assets/author-content-publish/rich-text-populated-component.png)

   Die **Text** -Komponente sollte nun den Rich-Text auf der Seite anzeigen.

1. Wiederholen Sie die obigen Schritte, ziehen Sie jedoch eine Instanz der **Image(v2)** -Komponente auf der Seite. Öffnen Sie die **Bild** Dialogfeld der Komponente.

1. Wechseln Sie in der linken Leiste zum **Asset-Suche** durch Klicken auf **Assets** icon ![Asset-Symbol](assets/author-content-publish/asset-icon.png).
1. **Drag &amp; Drop** ein Bild im Dialogfeld &quot;Komponente&quot;anzeigen und auf **Fertig** , um die Änderungen zu speichern.

   ![Asset zum Dialogfeld hinzufügen](assets/author-content-publish/add-asset-dialog.png)

1. Beachten Sie, dass sich Komponenten auf der Seite befinden, wie z. B. die **Titel**, **Navigation**, **Suche** die behoben sind. Diese Bereiche werden als Teil der Seitenvorlage konfiguriert und können nicht auf einer einzelnen Seite geändert werden. Dies wird im nächsten Kapitel näher erläutert.

Experimentieren Sie mit anderen Komponenten. Dokumentation zu den einzelnen [Die Kernkomponente finden Sie hier .](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html). Eine detaillierte Videoserie zu [Die Seitenbearbeitung finden Sie hier .](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html).

## Veröffentlichungsaktualisierungen {#publish-updates}

AEM Umgebungen auf eine **Autorendienst** und **Veröffentlichungsdienst**. In diesem Kapitel haben wir mehrere Änderungen an der Site auf der **Autorendienst**. Damit Besucher der Site die Änderungen anzeigen können, müssen wir sie in der **Veröffentlichungsdienst**.

![Allgemeine Abbildung](assets/author-content-publish/author-publish-high-level-flow.png)

*Übergeordneter Inhaltsfluss von der Autoren- zur Veröffentlichungsinstanz*

**1.** Inhaltsautoren nehmen Aktualisierungen am Site-Inhalt vor. Die Updates können als Vorschau angezeigt, geprüft und genehmigt werden, um live geschaltet zu werden.

**2.** Der Inhalt ist veröffentlicht. Die Veröffentlichung kann bei Bedarf erfolgen oder für ein künftiges Datum geplant werden.

**3.** Besucher der Site sehen die Änderungen, die im Veröffentlichungsdienst übernommen werden.

### Veröffentlichen der Änderungen

Als Nächstes veröffentlichen wir die Änderungen.

1. Navigieren Sie im Bildschirm AEM Start zu **Sites** und wählen Sie die **WKND-Site**.
1. Klicken Sie auf **Veröffentlichung verwalten** in der Menüleiste.

   ![Veröffentlichung verwalten](assets/author-content-publish/click-manage-publiciation.png)

   Da es sich um eine brandneue Site handelt, möchten wir alle Seiten veröffentlichen und können den Assistenten &quot;Veröffentlichung verwalten&quot;verwenden, um genau zu definieren, was veröffentlicht werden muss.

1. under **Optionen** Lassen Sie die Standardeinstellungen auf **Veröffentlichen** und planen Sie sie für **Jetzt**. Klicken Sie auf **Weiter**.
1. under **Anwendungsbereich**, wählen Sie die **WKND-Site** und klicken Sie auf **Untergeordnete Einstellungen einschließen**. Aktivieren Sie im Dialogfeld die Option **Untergeordnete Elemente einschließen**. Deaktivieren Sie die übrigen Felder, um sicherzustellen, dass die gesamte Site veröffentlicht wird.

   ![Veröffentlichungsumfang aktualisieren](assets/author-content-publish/update-scope-publish.png)

1. Klicken Sie auf **Veröffentlichte Verweise** Schaltfläche. Überprüfen Sie im Dialogfeld, ob alles aktiviert ist. Dies umfasst die **Standardsite-Vorlage** und mehrere Konfigurationen, die von der Site-Vorlage generiert wurden. Klicken **Fertig** zu aktualisieren.

   ![Veröffentlichen von Verweisen](assets/author-content-publish/publish-references.png)

1. Markieren Sie abschließend das Kästchen neben **WKND-Site** und klicken Sie auf **Nächste** in der oberen rechten Ecke.
1. Im **Workflows** Schritt, geben Sie eine **Workflow-Titel**. Dies kann ein beliebiger Text sein und später als Teil eines Audit-Protokolls nützlich sein. Geben Sie &quot;Erstveröffentlichung&quot;ein und klicken Sie auf **Veröffentlichen**.

![Erstveröffentlichung des Workflow-Schritts](assets/author-content-publish/workflow-step-publish.png)

## Veröffentlichte Inhalte anzeigen {#publish}

Navigieren Sie anschließend zum Veröffentlichungsdienst , um die Änderungen anzuzeigen.

1. Eine einfache Möglichkeit, die URL des Veröffentlichungsdienstes abzurufen, besteht darin, die Autoren-URL zu kopieren und die `author` Wort mit `publish`. Beispiel:

   * **Autoren-URL** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **Veröffentlichungs-URL** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. Hinzufügen `/content/wknd.html` zur Veröffentlichungs-URL hinzu, sodass die endgültige URL wie folgt aussieht: `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`.

   >[!NOTE]
   >
   > Änderung `wknd.html` , um den Namen Ihrer Site anzugeben, wenn Sie einen eindeutigen Namen für [Site-Erstellung](create-site.md).

1. Wenn Sie zur Veröffentlichungs-URL navigieren, sollte die Site ohne AEM Authoring-Funktion angezeigt werden.

   ![Veröffentlichte Site](assets/author-content-publish/publish-url-update.png)

1. Verwenden der **Navigation** Menüklick **Artikel** > **Hello World** um zur zuvor erstellten Seite &quot;Hello World&quot;zu navigieren.
1. Kehren Sie zu **AEM-Autorendienst** und nehmen einige zusätzliche Inhaltsänderungen im Seiteneditor vor.
1. Veröffentlichen Sie diese Änderungen direkt im Seiteneditor, indem Sie auf die **Seiteneigenschaften** Symbol > **Seite veröffentlichen**

   ![Publish direct](assets/author-content-publish/page-editor-publish.png)

1. Kehren Sie zu **AEM Publish Service** um die Änderungen anzuzeigen. Wahrscheinlich werden Sie **not** sofort die Aktualisierungen anzeigen. Dies liegt daran, dass die Variable **AEM Publish Service** include [Zwischenspeicherung über einen Apache-Webserver und ein CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html). Standardmäßig werden HTML-Inhalte ca. 5 Minuten lang zwischengespeichert.

1. Um den Cache für Test-/Debugging-Zwecke zu umgehen, fügen Sie einfach einen Abfrageparameter hinzu wie `?nocache=true`. Die URL würde wie folgt aussehen: `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`. Weitere Informationen zur Cachestrategie und den verfügbaren Konfigurationen [finden Sie hier .](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html).

1. Sie finden auch die URL zum Veröffentlichungsdienst in Cloud Manager. Navigieren Sie zum **Cloud Manager-Programm** > **Umgebungen** > **Umgebung**.

   ![Veröffentlichungsdienst anzeigen](assets/author-content-publish/view-environment-segments.png)

   under **Umgebungssegmente** finden Sie Links zu **Autor** und **Veröffentlichen** Dienste.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gerade Änderungen an Ihrer AEM Site verfasst und veröffentlicht!

### Nächste Schritte {#next-steps}

In einer realen Implementierung, die eine Site mit Stichproben und Benutzeroberflächen-Designs plant, geht in der Regel der Erstellung von Sites voraus. Erfahren Sie, wie Sie mit Adobe XD UI Kits Ihre Adobe Experience Manager Sites-Implementierung entwerfen und beschleunigen können in [Benutzeroberflächenplanung mit Adobe XD](./ui-planning-adobe-xd.md).

Möchten Sie weiterhin die Funktionen von AEM Sites erkunden? Sie können direkt in das Kapitel springen auf [Seitenvorlagen](./page-templates.md) um die Beziehung zwischen einer Seitenvorlage und einer Seite zu verstehen.



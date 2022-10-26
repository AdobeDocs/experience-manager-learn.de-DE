---
title: Erste Schritte mit AEM Sites - Seiten und Vorlagen
description: Erfahren Sie mehr über die Beziehung zwischen einer Basisseitenkomponente und bearbeitbaren Vorlagen. Erfahren Sie, wie Kernkomponenten in das Projekt integriert werden. Erfahren Sie mehr über erweiterte Richtlinienkonfigurationen bearbeitbarer Vorlagen, um eine gut strukturierte Artikelseitenvorlage zu erstellen, die auf einem Modell von Adobe XD basiert.
feature: Core Components, Editable Templates, Page Editor
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
exl-id: e9d06dc2-ac3b-48c5-ae00-fdaf5bb45b54
recommendations: noDisplay, noCatalog
source-git-commit: de2fa2e4c29ce6db31233ddb1abc66a48d2397a6
workflow-type: tm+mt
source-wordcount: '3066'
ht-degree: 1%

---

# Seiten und Vorlagen {#pages-and-template}

In diesem Kapitel werden wir die Beziehung zwischen einer Basisseitenkomponente und bearbeitbaren Vorlagen untersuchen. Wir werden eine nicht formatierte Artikelvorlage erstellen, die auf einigen von [AdobeXD](https://www.adobe.com/products/xd.html). Beim Erstellen der Vorlage werden Kernkomponenten und erweiterte Richtlinienkonfigurationen der bearbeitbaren Vorlagen behandelt.

## Voraussetzungen {#prerequisites}

Überprüfen Sie die erforderlichen Tools und Anweisungen zum Einrichten eines [lokale Entwicklungsumgebung](overview.md#local-dev-environment).

### Starterprojekt

>[!NOTE]
>
> Wenn Sie das vorherige Kapitel erfolgreich abgeschlossen haben, können Sie das Projekt wiederverwenden und die Schritte zum Auschecken des Starterprojekts überspringen.

Sehen Sie sich den Basis-Code an, auf dem das Tutorial aufbaut:

1. Sehen Sie sich die `tutorial/pages-templates-start` Verzweigung aus [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
   ```

1. Stellen Sie die Codebasis mithilfe Ihrer Maven-Kenntnisse in einer lokalen AEM-Instanz bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Wenn Sie AEM 6.5 oder 6.4 verwenden, hängen Sie die `classic` Profile zu beliebigen Maven-Befehlen hinzufügen.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Sie können den fertigen Code immer in [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution) oder den Code lokal auszuchecken, indem Sie zu der Verzweigung wechseln `tutorial/pages-templates-solution`.

## Ziel

1. Inspect ist ein in Adobe XD erstellter Seitenentwurf und ordnet ihn Kernkomponenten zu.
1. Machen Sie sich mit den Details bearbeitbarer Vorlagen vertraut und erfahren Sie, wie Richtlinien verwendet werden können, um eine granulare Steuerung des Seiteninhalts zu erzwingen.
1. Erfahren Sie, wie Vorlagen und Seiten verknüpft werden

## Was Sie erstellen werden {#what-you-will-build}

In diesem Teil des Tutorials erstellen Sie eine neue Artikelseitenvorlage, die verwendet werden kann, um neue Artikelseiten zu erstellen und an einer gemeinsamen Struktur auszurichten. Die Artikelseitenvorlage basiert auf Designs und einem UI-Kit, das in Adobe XD erstellt wurde. Dieses Kapitel konzentriert sich ausschließlich auf die Erstellung der Struktur oder des Skeletts der Vorlage. Es werden keine Stile implementiert, aber die Vorlage und die Seiten sind funktionsfähig.

![Artikelseitendesign und nicht formatierte Version](assets/pages-templates/what-you-will-build.png)

## Benutzeroberflächenplanung mit Adobe XD {#adobexd}

In den meisten Fällen beginnt die Planung für eine neue Website mit Stichproben und statischen Designs. [Adobe XD](https://www.adobe.com/products/xd.html) ist ein Design-Tool zum Erstellen von Benutzererlebnissen. Als Nächstes werden wir ein UI-Kit und Modelle untersuchen, um die Struktur der Artikelseitenvorlage zu planen.

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**Laden Sie die [WKND Article Design File](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

>[!NOTE]
>
> Ein generisches [AEM UI-Kit der Kernkomponenten ist ebenfalls verfügbar](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd) als Ausgangspunkt für benutzerdefinierte Projekte.

## Erstellen der Artikelseitenvorlage

Beim Erstellen einer Seite müssen Sie eine Vorlage auswählen, die als Grundlage für die Erstellung der neuen Seite verwendet wird. Die Vorlage definiert die Struktur der resultierenden Seite, den anfänglichen Inhalt und die zulässigen Komponenten.

Es gibt 3 Hauptbereiche von [Bearbeitbare Vorlagen](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=de):

1. **Struktur** - definiert Komponenten, die Teil der Vorlage sind. Diese können von Inhaltsautoren nicht bearbeitet werden.
1. **Anfänglicher Inhalt** - definiert Komponenten, mit denen die Vorlage beginnt. Diese können von Inhaltsautoren bearbeitet und/oder gelöscht werden.
1. **Richtlinien** - definiert Konfigurationen dazu, wie sich Komponenten verhalten und welche Optionen Autoren zur Verfügung stehen.

Erstellen Sie anschließend eine neue Vorlage in AEM, die der Struktur der Sicherungen entspricht. Dies geschieht in einer lokalen Instanz von AEM. Führen Sie die folgenden Schritte aus:

>[!VIDEO](https://video.tv.adobe.com/v/330991/?quality=12&learn=on)

Allgemeine Schritte für das obige Video:

### Strukturkonfigurationen

1. Erstellen Sie eine neue Vorlage mit dem **Seitenvorlagentyp**, benannt **Artikelseite**.
1. Alles in **Struktur** -Modus.
1. Hinzufügen einer **Experience Fragment** -Komponente, die als **Kopfzeile** oben in der Vorlage.
   * Konfigurieren der Komponente, auf die verwiesen werden soll `/content/experience-fragments/wknd/us/en/site/header/master`.
   * Legen Sie die Richtlinie auf **Seitenkopf** und stellen sicher, dass **Standardelement** auf `header`. Die `header`-Element im nächsten Kapitel mit CSS angesprochen wird.
1. Hinzufügen einer **Experience Fragment** -Komponente, die als **Fußzeile** unten in der Vorlage.
   * Konfigurieren der Komponente, auf die verwiesen werden soll `/content/experience-fragments/wknd/us/en/site/footer/master`.
   * Legen Sie die Richtlinie auf **Fußzeile der Seite** und stellen sicher, dass **Standardelement** auf `footer`. Die `footer` -Element im nächsten Kapitel mit CSS angesprochen wird.
1. Sperren Sie die **main** Container, der bei der anfänglichen Erstellung der Vorlage enthalten war.
   * Legen Sie die Richtlinie auf **Page Main** und stellen sicher, dass **Standardelement** auf `main`. Die `main` -Element im nächsten Kapitel mit CSS angesprochen wird.
1. Hinzufügen einer **Bild** -Komponente **main** Container.
   * Entsperren Sie die **Bild** -Komponente.
1. Hinzufügen einer **Breadcrumb** Komponente unter **Bild** -Komponente im Hauptbehälter.
   * Erstellen Sie eine neue Richtlinie für die **Breadcrumb** Komponente namens **Artikelseite - Breadcrumb**. Legen Sie die **Navigationsstartstufe** nach **4**.
1. Hinzufügen einer **Container** Komponente unter **Breadcrumb** und innerhalb der **main** Container. Dies dient als **Inhaltscontainer** für die Vorlage.
   * Entsperren Sie die **Inhalt** Container.
   * Legen Sie die Richtlinie auf **Seiteninhalt**.
1. Weitere hinzufügen **Container** Komponente unter **Inhaltscontainer**. Dies dient als **Seitenleiste** Container für die Vorlage.
   * Entsperren Sie die **Seitenleiste** Container.
   * Erstellen Sie eine neue Richtlinie mit dem Namen **Artikelseite - Seitenleiste**.
   * Konfigurieren Sie die **Zugelassene Komponenten** under **WKND Sites-Projekt - Inhalt** um Folgendes einzuschließen: **Schaltfläche**, **Download**, **Bild**, **Liste**, **Trennzeichen**, **Freigabe in Social Media**, **Text** und **Titel**.
1. Aktualisieren Sie die Richtlinie des Seitenstamm-Containers. Dies ist der äußere Behälter der Vorlage. Legen Sie die Richtlinie auf **Seitenstamm**.
   * under **Container-Einstellungen**, legen Sie die **Layout** nach **Responsives Raster**.
1. Layout-Modus für die **Inhaltscontainer**. Ziehen Sie den Ziehpunkt von rechts nach links und verkleinern Sie den Container auf 8 Spalten breit.
1. Layout-Modus für die **Seitenleisten-Container**. Ziehen Sie den Ziehpunkt von rechts nach links und verkleinern Sie den Container auf 4 Spalten breit. Ziehen Sie dann den linken Ziehpunkt von links nach rechts 1 Spalte, um den Container mit den 3 Spalten breit zu machen und eine 1-Spalten-Lücke zwischen der **Inhaltscontainer**.
1. Öffnen Sie den mobilen Emulator und wechseln Sie zu einem mobilen Breakpoint. Layout-Modus erneut einbinden und die **Inhaltscontainer** und **Seitenleisten-Container** die gesamte Breite der Seite. Dadurch werden die Container vertikal im mobilen Breakpoint gestapelt.
1. Aktualisieren Sie die Richtlinie des **Text** -Komponente in **Inhaltscontainer**.
   * Legen Sie die Richtlinie auf **Inhaltstext**.
   * under **Plugins** > **Absatzformate**, check **Absatzstile aktivieren** und stellen sicher, dass **Anführungsblock** aktiviert ist.

### Erstmalige Inhaltskonfigurationen

1. Wechseln zu **Anfänglicher Inhalt** -Modus.
1. Hinzufügen einer **Titel** -Komponente **Inhaltscontainer**. Dies dient als Artikeltitel. Wenn er leer gelassen wird, wird automatisch der Titel der aktuellen Seite angezeigt.
1. Sekunde hinzufügen **Titel** -Komponente unter der ersten Titelkomponente.
   * Konfigurieren Sie die Komponente mit dem Text: &quot;Nach Autor&quot;. Dies ist ein Textplatzhalter.
   * Festlegen des Typs `H4`.
1. Hinzufügen einer **Text** Komponente unter **Nach Autor** Titelkomponente.
1. Hinzufügen einer **Titel** -Komponente **Seitenleisten-Container**.
   * Konfigurieren Sie die Komponente mit dem Text: &quot;Diese Geschichte teilen&quot;.
   * Festlegen des Typs `H5`.
1. Hinzufügen einer **Freigabe in Social Media** Komponente unter **Diese Meldung teilen** Titelkomponente.
1. Hinzufügen einer **Trennzeichen** Komponente unter **Freigabe in Social Media** -Komponente.
1. Hinzufügen einer **Download** Komponente unter **Trennzeichen** -Komponente.
1. Hinzufügen einer **Liste** Komponente unter **Download** -Komponente.
1. Aktualisieren Sie die **Anfängliche Seiteneigenschaften** für die Vorlage.
   * under **Social Media** > **Freigabe in Social Media**, check **Facebook** und **Pinterest**

### Aktivieren Sie die Vorlage und fügen Sie eine Miniatur hinzu

1. Zeigen Sie die Vorlage in der Vorlagenkonsole an, indem Sie zu [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)
1. **Aktivieren** die Vorlage &quot;Artikelseite&quot;.
1. Bearbeiten Sie die Eigenschaften der Vorlage &quot;Artikelseite&quot;und laden Sie die folgende Miniaturansicht hoch, um die mit der Vorlage &quot;Artikelseite&quot;erstellten Seiten schnell zu identifizieren:

   ![Miniaturansicht der Artikelseitenvorlage](assets/pages-templates/article-page-template-thumbnail.png)

## Kopf- und Fußzeile mit Experience Fragments aktualisieren {#experience-fragments}

Eine gängige Praxis bei der Erstellung globaler Inhalte, wie Kopf- oder Fußzeilen, besteht darin, eine [Experience Fragment](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Experience Fragments ermöglicht es Benutzern, mehrere Komponenten zu kombinieren, um eine einzelne, referenzierbare Komponente zu erstellen. Experience Fragments bieten die Vorteile der Unterstützung von Multi-Site-Management und [Lokalisierung](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

Der AEM Projektarchetyp generiert eine Kopf- und Fußzeile. Aktualisieren Sie anschließend die Experience Fragments, um sie an die Modelle anzupassen. Führen Sie die folgenden Schritte aus:

>[!VIDEO](https://video.tv.adobe.com/v/330992/?quality=12&learn=on)

Allgemeine Schritte für das obige Video:

1. Beispielinhaltspaket herunterladen **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)**.
1. Laden Sie das Inhaltspaket mit Package Manager hoch und installieren Sie es unter [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Aktualisieren Sie die Vorlage Webvariante , die die Vorlage für Experience Fragments unter [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)
   * Aktualisieren Sie die Richtlinie **Container** -Komponente in der Vorlage.
   * Legen Sie die Richtlinie auf **XF-Stamm**.
   * under **Zugelassene Komponenten** Komponentengruppe auswählen **WKND Sites-Projekt - Struktur** um **Sprachnavigation**, **Navigation** und **Schnellsuche** Komponenten.

### Header-Experience Fragment aktualisieren

1. Öffnen Sie das Experience Fragment, das die Kopfzeile unter [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)
1. Konfigurieren des Stamms **Container** des Fragments. Dies ist der äußere **Container**.
   * Legen Sie die **Layout** nach **Responsives Raster**
1. Fügen Sie die **WKND Dark Logo** als Bild oben im **Container**. Das Logo war im Paket enthalten, das in einem vorherigen Schritt installiert wurde.
   * Layout der **WKND Dark Logo** zu **2** Spalten breit. Ziehen Sie die Griffe von rechts nach links.
   * Konfigurieren Sie das Logo mit **Alternativtext** des &quot;WKND-Logos&quot;.
   * Konfigurieren Sie das Logo für **Link** nach `/content/wknd/us/en` die Startseite.
1. Konfigurieren Sie die **Navigation** -Komponente, die bereits auf der Seite platziert ist.
   * Legen Sie die **Ausschließen von Stammebenen** nach **1**.
   * Legen Sie die **Navigationsstrukturtiefe** nach **1**.
   * Layout der **Navigation** Komponente **8** Spalten breit. Ziehen Sie die Griffe von rechts nach links.
1. Entfernen Sie die **Sprachnavigation** -Komponente.
1. Layout der **Suche** Komponente **2** Spalten breit. Ziehen Sie die Griffe von rechts nach links. Alle Komponenten sollten jetzt horizontal auf einer einzigen Zeile ausgerichtet werden.

### Fußzeile für Experience Fragment aktualisieren

1. Öffnen Sie das Experience Fragment, das die Fußzeile am [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)
1. Konfigurieren des Stamms **Container** des Fragments. Dies ist der äußere **Container**.
   * Legen Sie die **Layout** nach **Responsives Raster**
1. Fügen Sie die **WKND-Light-Logo** als Bild oben im **Container**. Das Logo war im Paket enthalten, das in einem vorherigen Schritt installiert wurde.
   * Layout der **WKND-Light-Logo** zu **2** Spalten breit. Ziehen Sie die Griffe von rechts nach links.
   * Konfigurieren Sie das Logo mit **Alternativtext** des &quot;WKND Logo Light&quot;.
   * Konfigurieren Sie das Logo für **Link** nach `/content/wknd/us/en` die Startseite.
1. Hinzufügen einer **Navigation** -Komponente unter dem -Logo. Konfigurieren Sie die **Navigation** component:
   * Legen Sie die **Ausschließen von Stammebenen** nach **1**.
   * Deaktivieren **Sammlung aller untergeordneten Seiten**.
   * Legen Sie die **Navigationsstrukturtiefe** nach **1**.
   * Layout der **Navigation** Komponente **8** Spalten breit. Ziehen Sie die Griffe von rechts nach links.

## Artikelseite erstellen

Erstellen Sie anschließend eine neue Seite mithilfe der Vorlage &quot;Artikelseite&quot;. Verfassen Sie den Inhalt der Seite so, dass er mit den Site-Stichproben übereinstimmt. Führen Sie die folgenden Schritte aus:

>[!VIDEO](https://video.tv.adobe.com/v/330993/?quality=12&learn=on)

Allgemeine Schritte für das obige Video:

1. Navigieren Sie zur Sites-Konsole unter [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine).
1. Erstellen Sie eine neue Seite unter **WKND** > **USA** > **DE** > **Magazin**.
   * Wählen Sie die **Artikelseite** Vorlage.
   * under **Eigenschaften** legen Sie die **Titel** zu &quot;Ultimate Guide to LA Skateparks&quot;
   * Legen Sie die **Name** &quot;guide-la-skateparks&quot;
1. Ersetzen **Nach Autor** Titel mit dem Text &quot;By Stacey Roswells&quot;.
1. Aktualisieren Sie die **Text** -Komponente, um einen Absatz zum Ausfüllen des Artikels einzuschließen. Sie können die folgende Textdatei als Kopie verwenden: [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt).
1. Weitere hinzufügen **Text** -Komponente.
   * Aktualisieren Sie die Komponente, um das Anführungszeichen einzuschließen: &quot;Es gibt keinen besseren Ort, um ihn zu teilen, als Los Angeles.&quot;
   * Bearbeiten Sie den Rich-Text-Editor im Vollbildmodus und ändern Sie das obige Anführungszeichen, um die **Anführungsblock** -Element.
1. Fahren Sie mit dem Füllen des Hauptteils des Artikels fort, um mit den Sicherungen zu übereinstimmen.
1. Konfigurieren Sie die **Download** -Komponente verwenden, um eine PDF-Version des Artikels zu verwenden.
   * under **Download** > **Eigenschaften**, klicken Sie auf das Kontrollkästchen **Abrufen des Titels aus dem DAM-Asset**.
   * Legen Sie die **Beschreibung** an: &quot;Get the Full Story&quot;.
   * Legen Sie die **Aktionstext** an: &quot;PDF herunterladen&quot;.
1. Konfigurieren Sie die **Liste** -Komponente.
   * under **Listeneinstellungen** > **Erstellen einer Liste mithilfe von** auswählen **Untergeordnete Seiten**.
   * Legen Sie die **Übergeordnete Seite** nach `/content/wknd/us/en/magazine`.
   * under **Elementeinstellungen** check **Verknüpfungselemente** und überprüfen **Datum anzeigen**.

## Inspect der Knotenstruktur {#node-structure}

An dieser Stelle ist die Artikelseite eindeutig unformatiert. Die Grundstruktur ist jedoch vorhanden. Überprüfen Sie anschließend die Knotenstruktur der Artikelseite, um ein besseres Verständnis der Rolle der Vorlage, Seite und Komponenten zu erhalten.

Verwenden Sie das CRXDE-Lite-Tool auf einer lokalen AEM-Instanz, um die zugrunde liegende Knotenstruktur anzuzeigen.

1. Öffnen [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) und navigieren Sie mithilfe der Navigationsstruktur zu `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Klicken Sie auf `jcr:content` Knoten unter `la-skateparks` und zeigen Sie die Eigenschaften an:

   ![JCR-Inhaltseigenschaften](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Beachten Sie den Wert für `cq:template`, der auf `/conf/wknd/settings/wcm/templates/article-page`, die zuvor erstellte Artikelseitenvorlage.

   Beachten Sie außerdem den Wert von `sling:resourceType`, der auf `wknd/components/page`. Dies ist die Seitenkomponente, die vom AEM Projektarchetyp erstellt wurde und für das Rendern der Seite basierend auf der Vorlage verantwortlich ist.

1. Erweitern Sie die `jcr:content` Knoten unter `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` und zeigen Sie die Knotenhierarchie an:

   ![JCR Content LA Skateparks](assets/pages-templates/page-jcr-structure.png)

   Sie sollten in der Lage sein, jeden Knoten lose Komponenten zuzuordnen, die erstellt wurden. Ermitteln Sie, ob Sie die verschiedenen Layout-Container identifizieren können, indem Sie die Knoten überprüfen, denen das Präfix `container`.

1. Überprüfen Sie die Seitenkomponente als Nächstes unter `/apps/wknd/components/page`. Zeigen Sie die Komponenteneigenschaften in CRXDE Lite an:

   ![Eigenschaften der Seitenkomponenten](assets/pages-templates/page-component-properties.png)

   Beachten Sie, dass es nur 2 HTL-Skripte gibt. `customfooterlibs.html` und `customheaderlibs.html` unterhalb der Seitenkomponente. *Wie rendert diese Komponente die Seite?*

   Die `sling:resourceSuperType` -Eigenschaft verweist auf `core/wcm/components/page/v2/page`. Mit dieser Eigenschaft kann die Seitenkomponente des WKND erben **all** der Funktionalität der Kernkomponente-Seitenkomponente. Dies ist das erste Beispiel einer so genannten [Proxy-Komponentenmuster](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern). Weitere Informationen finden Sie [hier](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html).

1. Inspect eine weitere Komponente innerhalb der WKND-Komponenten, die `Breadcrumb` Komponente unter: `/apps/wknd/components/breadcrumb`. Beachten Sie Folgendes: `sling:resourceSuperType` -Eigenschaft gefunden werden, aber diesmal verweist sie auf `core/wcm/components/breadcrumb/v2/breadcrumb`. Dies ist ein weiteres Beispiel für die Verwendung des Proxy-Komponentenmusters zum Einschließen einer Kernkomponente. Tatsächlich sind alle Komponenten in der WKND-Codebasis Proxys von AEM Kernkomponenten (mit Ausnahme unserer berühmten Komponente &quot;HelloWorld&quot;). Es empfiehlt sich, möglichst viele Funktionen von Kernkomponenten wiederzuverwenden *before* Schreiben von benutzerdefiniertem Code.

1. Überprüfen Sie als Nächstes die Kernkomponentenseite unter `/libs/core/wcm/components/page/v2/page` mithilfe von CRXDE Lite:

   >[!NOTE]
   >
   > In AEM 6.5/6.4 befinden sich die Kernkomponenten unter `/apps/core/wcm/components`. In AEM as a Cloud Service befinden sich die Kernkomponenten unter `/libs` und werden automatisch aktualisiert.

   ![Kernkomponentenseite](assets/pages-templates/core-page-component-properties.png)

   Beachten Sie, dass unter dieser Seite viele weitere Skripte enthalten sind. Die Seite &quot;Kernkomponente&quot;enthält viele Funktionen. Diese Funktion ist in mehrere Skripte unterteilt, um die Wartung und Lesbarkeit zu erleichtern. Sie können die Einbeziehung der HTL-Skripte verfolgen, indem Sie die `page.html` und suchen `data-sly-include`:

   ```html
   <!--/* /libs/core/wcm/components/page/v2/page/page.html */-->
   <!DOCTYPE HTML>
   <html data-sly-use.page="com.adobe.cq.wcm.core.components.models.Page" lang="${page.language}"
       data-sly-use.head="head.html"
       data-sly-use.footer="footer.html"
       data-sly-use.redirect="redirect.html">
       <head data-sly-call="${head.head @ page = page}"></head>
       <body class="${page.cssClassNames}"
           id="${page.id}"
           data-cmp-data-layer-enabled="${page.data ? true : false}">
           <script data-sly-test.dataLayerEnabled="${page.data}">
           window.adobeDataLayer = window.adobeDataLayer || [];
           adobeDataLayer.push({
               page: JSON.parse("${page.data.json @ context='scriptString'}"),
               event:'cmp:show',
               eventInfo: {
                   path: 'page.${page.id @ context="scriptString"}'
               }
           });
           </script>
           <sly data-sly-test.isRedirectPage="${page.redirectTarget && (wcmmode.edit || wcmmode.preview)}"
               data-sly-call="${redirect.redirect @ redirectTarget = page.redirectTarget}"></sly>
           <sly data-sly-test="${!isRedirectPage}">
               <sly data-sly-include="body.skiptomaincontent.html"></sly>
               <sly data-sly-include="body.socialmedia_begin.html"></sly>
               <sly data-sly-include="body.html"></sly>
               <sly data-sly-call="${footer.footer @ page = page}"></sly>
               <sly data-sly-include="body.socialmedia_end.html"></sly>
           </sly>
       </body>
   </html>
   ```

   Der andere Grund für das Ausschlüsseln der HTL in mehrere Skripte besteht darin, den Proxy-Komponenten zu ermöglichen, einzelne Skripte zu überschreiben, um benutzerdefinierte Geschäftslogik zu implementieren. HTL-Skripte, `customfooterlibs.html` und `customheaderlibs.html`, werden für den expliziten Zweck erstellt, der durch die Implementierung von Projekten außer Kraft gesetzt werden soll.

   Sie können mehr darüber erfahren, wie die bearbeitbare Vorlage in das Rendering der [Inhaltsseite durch Lesen dieses Artikels](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html).

1. Inspect die andere Kernkomponente, wie die Breadcrumb-Leiste unter `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`. Anzeigen der `breadcrumb.html` -Skript, um zu verstehen, wie das Markup für die Breadcrumb-Komponente letztendlich generiert wird.

## Speichern von Konfigurationen in der Quell-Code-Verwaltung {#configuration-persistence}

In vielen Fällen ist es insbesondere zu Beginn eines AEM-Projekts nützlich, Konfigurationen wie Vorlagen und zugehörige Inhaltsrichtlinien zur Quell-Code-Verwaltung beizubehalten. Dadurch wird sichergestellt, dass alle Entwickler mit demselben Inhalt und denselben Konfigurationen arbeiten und zusätzliche Konsistenz zwischen Umgebungen sichergestellt wird. Sobald ein Projekt einen gewissen Reifegrad erreicht hat, kann die Verwaltung von Vorlagen einer speziellen Gruppe von Power-Benutzern übertragen werden.

Zunächst werden wir die Vorlagen wie andere Codeabschnitte behandeln und die **Artikelseitenvorlage** als Teil des Projekts. Bis jetzt haben wir **pusht** Code aus unserem AEM-Projekt in eine lokale Instanz von AEM. Die **Artikelseitenvorlage** wurde direkt auf einer lokalen Instanz von AEM erstellt. Daher müssen wir **importieren** die Vorlage in unser AEM Projekt. Die **ui.content** -Modul ist zu diesem Zweck im AEM Projekt enthalten.

Die nächsten Schritte werden mit der VSCode IDE mit der [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview) -Plug-in, aber möglicherweise mit einer beliebigen IDE arbeiten, für die Sie **importieren** oder importieren Sie Inhalte aus einer lokalen Instanz von AEM.

1. Öffnen Sie im VSCode die `aem-guides-wknd` Projekt.

1. Erweitern Sie die **ui.content** -Modul im Projekt-Explorer. Erweitern Sie die `src` Ordner und navigieren Sie zu `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Rechts+Klicken] die `templates` Ordner und wählen Sie **Import von AEM Server**:

   ![VSCode-Importvorlage](assets/pages-templates/vscode-import-templates.png)

   Die `article-page` importiert werden und die `page-content`, `xf-web-variation` -Vorlagen sollten ebenfalls aktualisiert werden.

   ![Aktualisierte Vorlagen](assets/pages-templates/updated-templates.png)

1. Wiederholen Sie die Schritte zum Importieren von Inhalten, wählen Sie jedoch die **policies** Ordner unter `/conf/wknd/settings/wcm/policies`.

   ![VSCode-Importrichtlinien](assets/pages-templates/policies-article-page-template.png)

1. Inspect `filter.xml` Datei unter `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
   <workspaceFilter version="1.0">
       <filter root="/conf/wknd" mode="merge"/>
       <filter root="/content/wknd" mode="merge"/>
       <filter root="/content/dam/wknd" mode="merge"/>
       <filter root="/content/experience-fragments/wknd" mode="merge"/>
   </workspaceFilter>
   ```

   Die `filter.xml` -Datei ist für die Identifizierung der Pfade von Knoten verantwortlich, die mit dem Paket installiert werden. Beachten Sie die `mode="merge"` bei jedem Filter, der anzeigt, dass existierender Inhalt nicht geändert wird, werden nur neue Inhalte hinzugefügt. Da Inhaltsautoren diese Pfade möglicherweise aktualisieren, ist es wichtig, dass eine Codebereitstellung **not** Inhalt überschreiben. Siehe [FileVault-Dokumentation](https://jackrabbit.apache.org/filevault/filter.html) Weitere Informationen zum Arbeiten mit Filterelementen.

   Vergleichen `ui.content/src/main/content/META-INF/vault/filter.xml` und `ui.apps/src/main/content/META-INF/vault/filter.xml` um die verschiedenen Knoten zu verstehen, die von den einzelnen Modulen verwaltet werden.

   >[!WARNING]
   >
   > Um eine konsistente Bereitstellung der WKND-Referenz-Site sicherzustellen, werden einige Verzweigungen des Projekts so eingerichtet, dass `ui.content` überschreibt alle Änderungen im JCR. Dies erfolgt standardmäßig, d. h. für Lösungsverzweigungen, da Code/Stile für bestimmte Richtlinien geschrieben werden.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben soeben eine neue Vorlage und Seite mit Adobe Experience Manager Sites erstellt.

### Nächste Schritte {#next-steps}

An dieser Stelle ist die Artikelseite eindeutig unformatiert. Befolgen Sie die [Client-seitige Bibliotheken und Frontend-Workflow](client-side-libraries.md) Tutorial zu den Best Practices zum Einschließen von CSS und JavaScript, um globale Stile auf die Site anzuwenden und einen dedizierten Front-End-Build zu integrieren.

Anzeigen des fertigen Codes unter [GitHub](https://github.com/adobe/aem-guides-wknd) oder den Code lokal in der Git-Klammer überprüfen und bereitstellen `tutorial/pages-templates-solution`.

1. Klonen Sie die [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) Repository.
1. Sehen Sie sich die `tutorial/pages-templates-solution` -Verzweigung.

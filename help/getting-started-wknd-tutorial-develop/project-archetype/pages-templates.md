---
title: Erste Schritte mit AEM Sites – Seiten und Vorlagen
description: Erfahren Sie mehr über die Beziehung zwischen einer Basisseitenkomponente und bearbeitbaren Vorlagen. Erfahren Sie, wie Kernkomponenten in das Projekt integriert werden. Erfahren Sie mehr über erweiterte Richtlinienkonfigurationen bearbeitbarer Vorlagen, um eine gut strukturierte Vorlage für Artikelseiten zu erstellen, die auf einem Modell von Adobe XD basiert.
feature: Core Components, Editable Templates, Page Editor
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
exl-id: e9d06dc2-ac3b-48c5-ae00-fdaf5bb45b54
recommendations: noDisplay, noCatalog
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '3040'
ht-degree: 99%

---

# Seiten und Vorlagen {#pages-and-template}

In diesem Kapitel untersuchen wir die Beziehung zwischen einer Basisseitenkomponente und bearbeitbaren Vorlagen. Erfahren Sie, wie Sie eine nicht formatierte Artikelvorlage auf der Grundlage einiger Mockups aus [Adobe XD](https://helpx.adobe.com/de/support/xd.html) erstellen. Beim Erstellen der Vorlage werden Kernkomponenten und erweiterte Richtlinienkonfigurationen der bearbeitbaren Vorlagen behandelt.

## Voraussetzungen {#prerequisites}

Vergegenwärtigen Sie sich die erforderlichen Tools und Anweisungen zum Einrichten einer [lokalen Entwicklungsumgebung](overview.md#local-dev-environment).

### Ausgangsprojekt

>[!NOTE]
>
> Wenn Sie das vorherige Kapitel erfolgreich abgeschlossen haben, können Sie das Projekt wiederverwenden und die Schritte zum Ausprobieren des Ausgangsprojekts überspringen.

Checken Sie den Basis-Code aus, auf dem das Tutorial aufbaut:

1. Checken Sie die Verzweigung `tutorial/pages-templates-start` aus [GitHub](https://github.com/adobe/aem-guides-wknd) aus.

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
   ```

1. Stellen Sie die Code-Basis mithilfe Ihrer Maven-Kenntnisse in einer lokalen AEM-Instanz bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Wenn Sie AEM 6.5 oder 6.4 verwenden, fügen Sie das `classic`-Profil an beliebige Maven-Befehle an.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Sie können den fertigen Code jederzeit auf [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution) ansehen oder den Code lokal herunterladen, indem Sie zu der Verzweigung `tutorial/pages-templates-solution` wechseln.

## Ziel

1. Prüfen Sie ein in Adobe XD erstelltes Seiten-Design und ordnen Sie es Kernkomponenten zu.
1. Machen Sie sich mit den Details bearbeitbarer Vorlagen vertraut und erfahren Sie, wie Richtlinien verwendet werden können, um eine granulare Steuerung des Seiteninhalts zu ermöglichen.
1. Erfahren Sie, wie Vorlagen und Seiten verknüpft werden

## Was Sie erstellen werden {#what-build}

In diesem Teil des Tutorials erstellen Sie eine neue Artikelseitenvorlage, die verwendet werden kann, um Artikelseiten zu erstellen und an einer gemeinsamen Struktur auszurichten. Die Artikelseitenvorlage basiert auf Designs und einem in Adobe XD erstellten UI-Kit. Dieses Kapitel konzentriert sich ausschließlich auf die Erstellung der Struktur oder des Skeletts der Vorlage. Es werden keine Stile implementiert, aber die Vorlage und die Seiten sind funktionsfähig.

![Design von Artikelseiten und nicht formatierte Version](assets/pages-templates/what-you-will-build.png)

## Benutzeroberflächenplanung mit Adobe XD {#adobexd}

Normalerweise beginnt die Planung für eine neue Website mit Mockups und statischen Designs. [Adobe XD](https://helpx.adobe.com/de/support/xd.html) ist ein Design-Tool zum Erstellen von Anwendererlebnissen. Als Nächstes sehen wir uns ein UI-Kit und Mockups an, um die Struktur der Artikelseitenvorlage zu planen.

>[!VIDEO](https://video.tv.adobe.com/v/30214?quality=12&learn=on)

**Laden Sie die [Design-Datei für WKND-Artikel](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)** herunter.

>[!NOTE]
>
> Ein generisches [UI-Kit der AEM-Kernkomponenten ist ebenfalls verfügbar](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd) und dient als Ausgangspunkt für benutzerdefinierte Projekte.

## Erstellen der Artikelseitenvorlage

Wenn Sie eine Seite erstellen, müssen Sie eine Vorlage auswählen. Diese wird als Grundlage für die Erstellung der Seite verwendet. Die Vorlage definiert die Struktur der resultierenden Seite, den anfänglichen Inhalt und die zulässigen Komponenten.

Für [bearbeitbare Vorlagen](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=de) sind drei Hauptbereiche vorhanden:

1. **Struktur** – definiert Komponenten, die Teil der Vorlage sind. Diese können von Inhaltsautorinnen und -autoren nicht bearbeitet werden.
1. **Anfänglicher Inhalt** – definiert Komponenten, mit denen die Vorlage startet. Diese können von Inhaltsautorinnen und -autoren bearbeitet und/oder gelöscht werden.
1. **Richtlinien** – definiert Konfigurationen zum Verhalten von Komponenten und zu den Optionen, die Autorinnen und Autoren haben.

Erstellen Sie anschließend eine Vorlage in AEM, die der Struktur der Mockups entspricht. Dies geschieht in einer lokalen Instanz von AEM. Führen Sie die folgenden Schritte aus:

>[!VIDEO](https://video.tv.adobe.com/v/330991?quality=12&learn=on)

Allgemeine Schritte für das obige Video:

### Strukturkonfigurationen

1. Erstellen Sie eine Vorlage mit dem **Seitenvorlagentyp** namens **Artikelseite**.
1. Wechseln Sie in den **Struktur**-Modus.
1. Fügen Sie eine **Experience Fragment**-Komponente hinzu, die als **Header** oben in der Vorlage agiert.
   * Konfigurieren Sie die Komponente, die auf `/content/experience-fragments/wknd/us/en/site/header/master` verweisen soll.
   * Legen Sie die Richtlinie auf **Seitenkopf** fest und stellen sicher, dass **Standardelement** auf `header` gesetzt ist. Das `header`-Element wird im nächsten Kapitel mit CSS angesprochen.
1. Fügen Sie eine **Experience Fragment**-Komponente hinzu, die als **Fußzeile** unten in der Vorlage agiert.
   * Konfigurieren Sie die Komponente, die auf `/content/experience-fragments/wknd/us/en/site/footer/master` verweisen soll.
   * Legen Sie die Richtlinie auf **Seitenfußzeile** fest und stellen Sie sicher, dass das **Standardelement** auf `footer` gesetzt ist. Das `footer`-Element wird im nächsten Kapitel mit CSS angesprochen.
1. Sperren Sie den **Haupt**-Container, der bei der anfänglichen Erstellung der Vorlage enthalten war.
   * Legen Sie die Richtlinie auf **Seitenhauptteil** fest und stellen sicher, dass das **Standardelement** auf `main` gesetzt ist. Das `main`-Element wird im nächsten Kapitel mit CSS angesprochen.
1. Fügen Sie eine **Bildkomponente** zum **Haupt**-Container hinzu.
   * Entsperren Sie die **Bildkomponente**.
1. Fügen Sie eine **Breadcrumb**-Komponente unter der **Bildkomponente** im Haupt-Container hinzu.
   * Erstellen Sie eine Richtlinie für die **Breadcrumb**-Komponente namens **Artikelseite – Breadcrumb**. Setzen Sie die **Navigationsstartstufe** auf **4**.
1. Fügen Sie eine **Container**-Komponente unter der **Breadcrumb**-Komponente und innerhalb des **Haupt**-Containers hinzu. Dies dient als **Inhalts-Container** für die Vorlage.
   * Entsperren Sie den **Inhalts**-Container.
   * Legen Sie die Richtlinie auf **Seiteninhalt** fest.
1. Fügen Sie eine weitere **Container**-Komponente unter dem **Inhalts-Container** hinzu. Dies dient als **Seitenleisten**-Container für die Vorlage.
   * Entsperren Sie den **Seitenleisten**-Container.
   * Erstellen Sie eine Richtlinie mit dem Namen **Artikelseite – Seitenleiste**.
   * Konfigurieren Sie die **zugelassenen Komponenten** unter **WKND Sites-Projekt – Inhalt**, um Folgendes einzuschließen: **Schaltfläche**, **Download**, **Bild**, **Liste**, **Trennzeichen**, **Freigabe in Social Media**, **Text** und **Titel**.
1. Aktualisieren Sie die Richtlinie des Seitenstamm-Containers. Dies ist der äußerste Container der Vorlage. Legen Sie die Richtlinie auf **Seitenstamm** fest.
   * Setzen Sie unter **Container-Einstellungen** das **Layout** auf **Responsives Raster**.
1. Schalten Sie den Layout-Modus für den **Inhalts-Container** ein. Ziehen Sie den Ziehgriff von rechts nach links und verkleinern Sie den Container auf eine Breite von acht Spalten.
1. Schalten Sie den Layout-Modus für den **Seitenleisten-Container** ein. Ziehen Sie den Ziehgriff von rechts nach links und verkleinern Sie den Container auf eine Breite von vier Spalten. Ziehen Sie dann den linken Ziehgriff um eine Spalte von links nach rechts, damit der Container 3 Spalten breit wird und eine Lücke von 1 Spalte zum **Inhalts-Container** lässt.
1. Öffnen Sie den Mobile-Emulator und wechseln Sie zu einem Mobile-Breakpoint. Schalten Sie erneut den Layout-Modus ein und ziehen Sie den **Inhalts-Container** und den **Seitenleisten-Container** auf die gesamte Seitenbreite auf. Dadurch werden die Container vertikal im Mobile-Breakpoint gestapelt.
1. Aktualisieren Sie die Richtlinie der **Textkomponente** auf **Inhalts-Container**.
   * Legen Sie die Richtlinie auf **Inhaltstext** fest.
   * Aktivieren Sie unter **Plug-ins** > **Absatzformate** das Kontrollkästchen **Absatzformate aktivieren** und stellen Sie sicher, dass die Option **Zitatblock** aktiviert ist.

### Konfigurieren von anfänglichen Inhalten

1. Wechseln Sie in den Modus **Anfänglicher Inhalt**.
1. Fügen Sie eine **Titelkomponente** zum **Inhalts-Container** hinzu. Dies dient als Artikeltitel. Ohne Angabe wird automatisch der Titel der aktuellen Seite angezeigt.
1. Fügen Sie eine zweite **Titelkomponente** unter der ersten Titelkomponente hinzu.
   * Konfigurieren Sie die Komponente mit dem Text „By Author“. Dies ist ein Textplatzhalter.
   * Legen Sie `H4` als Typ fest.
1. Fügen Sie eine **Textkomponente** unter der Titelkomponente **By Author** hinzu.
1. Fügen Sie eine **Titelkomponente** zum **Seitenleisten-Container** hinzu.
   * Konfigurieren Sie die Komponente mit dem Text „Share this Story“.
   * Legen Sie `H5` als Typ fest.
1. Fügen Sie eine **Social Media Sharing**-Komponente unter der Titelkomponente **Share this Story** hinzu.
1. Fügen Sie eine **Trennzeichen**-Komponente unter der **Social Media Sharing**-Komponente hinzu.
1. Fügen Sie eine **Download**-Komponente unter der **Trennzeichen**-Komponente hinzu.
1. Fügen Sie eine **Listenkomponente** unter der **Download**-Komponente hinzu.
1. Aktualisieren Sie die **anfänglichen Seiteneigenschaften** für die Vorlage.
   * Aktivieren Sie unter **Social Media** > **Social Media Sharing** die Kontrollkästchen **Facebook** und **Pinterest**.

### Aktivieren der Vorlage und Hinzufügen eine Miniaturansicht

1. Zeigen Sie die Vorlage in der Vorlagenkonsole an, indem Sie zu [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd) navigieren.
1. **Aktivieren** Sie die Artikelseitenvorlage.
1. Bearbeiten Sie die Eigenschaften der Artikelseitenvorlage und laden Sie die folgende Miniaturansicht hoch, um die mit der Artikelseitenvorlage erstellten Seiten schnell zu identifizieren:

   ![Miniaturansicht der Artikelseitenvorlage](assets/pages-templates/article-page-template-thumbnail.png)

## Aktualisieren von Kopf- und Fußzeile mit Experience Fragments {#experience-fragments}

Eine gängige Praxis bei der Erstellung globaler Inhalte, wie Kopf- oder Fußzeilen, ist die Verwendung eines [Experience Fragments](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=de). Experience Fragments ermöglichen es Benutzenden, mehrere Komponenten zu kombinieren, um eine einzelne, referenzierbare Komponente zu erstellen. Experience Fragments haben den Vorteil, dass sie die Verwaltung mehrerer Websites und die [lokale Anpassung](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html?lang=de) unterstützen.

Der AEM-Projektarchetyp generiert eine Kopf- und Fußzeile. Aktualisieren Sie anschließend die Experience Fragments, um sie an die Mockups anzupassen. Führen Sie die folgenden Schritte aus:

>[!VIDEO](https://video.tv.adobe.com/v/330992?quality=12&learn=on)

Allgemeine Schritte für das obige Video:

1. Laden Sie das Paket **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)** herunter, das Beispielinhalte enthält.
1. Laden Sie mithilfe von Package Manager das Inhaltspaket hoch und installieren Sie es unter [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).
1. Aktualisieren Sie die Vorlage „Web-Variante“, also die Vorlage für Experience Fragments, unter [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html).
   * Aktualisieren Sie die Richtlinie der **Container**-Komponente in der Vorlage.
   * Legen Sie für die Richtlinie die Option **XF-Stamm** fest.
   * Wählen Sie unter **Zugelassene Komponenten** die Komponentengruppe **WKND Sites-Projekt – Struktur** aus, um die Komponenten **Sprachnavigation**, **Navigation** und **Schnellsuche** einzuschließen.

### Aktualisieren des Experience Fragments für die Kopfzeile

1. Öffnen Sie das Experience Fragment, das die Kopfzeile rendert, unter [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html).
1. Konfigurieren Sie den **Stamm-Container** des Fragments. Dies ist der **Container**, der sich ganz außen befindet.
   * Legen Sie für das **Layout** die Option **Responsives Raster** fest.
1. Fügen Sie das **dunkle WKND-Logo** als Bild oben im **Container** hinzu. Das Logo war im Paket enthalten, das in einem vorherigen Schritt installiert wurde.
   * Ändern Sie das Layout des **dunklen WKND-Logos** so, dass es **zwei** Spalten breit ist. Ziehen Sie die Griffe von rechts nach links.
   * Konfigurieren Sie das Logo so, dass „WKND Logo“ als **Alternativtext** verwendet wird.
   * Konfigurieren Sie das Logo so, dass es auf der Startseite mit `/content/wknd/us/en` **verknüpft** ist.
1. Konfigurieren Sie die **Navigationskomponente**, die bereits auf der Seite platziert ist.
   * Legen Sie **Ausschließen von Stammebenen** auf **1** fest.
   * Legen Sie die **Navigationsstrukturtiefe** auf **1** fest.
   * Passen Sie das Layout der **Navigationskomponente** so an, dass sie **acht** Spalten breit ist. Ziehen Sie die Griffe von rechts nach links.
1. Entfernen Sie die **Sprachnavigationskomponente**.
1. Passen Sie das Layout der **Suchkomponente** so an, dass sie **zwei** Spalten breit ist. Ziehen Sie die Griffe von rechts nach links. Alle Komponenten sollten jetzt horizontal auf einer einzelnen Zeile ausgerichtet sein.

### Aktualisieren der Fußzeile für Experience Fragment

1. Öffnen Sie das Experience Fragment, das die Fußzeile unter [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html) rendert.
1. Konfigurieren Sie den **Stamm-Container** des Fragments. Dies ist der **Container**, der sich ganz außen befindet.
   * Legen Sie das **Layout** auf **Responsives Raster** fest
1. Fügen Sie das **WKND-Light-Logo** als Bild oben zum **Container** hinzu. Das Logo war im Paket enthalten, das in einem vorherigen Schritt installiert wurde.
   * Passen Sie das Layout des **WKND-Light-Logos** so an, dass es **zwei** Spalten breit ist. Ziehen Sie die Griffe von rechts nach links.
   * Konfigurieren Sie das Logo mit **Alternativtext** von „WKND Logo Light“.
   * Konfigurieren Sie das Logo so, dass es `/content/wknd/us/en` auf die Startseite **verlinkt**.
1. Fügen Sie eine **Navigationskomponente** unter dem Logo hinzu. Konfigurieren Sie die **Navigationskomponente**:
   * Legen Sie **Ausschließen von Stammebenen** auf **1** fest.
   * Deaktivieren Sie **Alle untergeordneten Seiten erfassen**.
   * Legen Sie die **Navigationsstrukturtiefe** auf **1** fest.
   * Passen Sie das Layout der **Navigationskomponente** so an, dass sie **acht** Spalten breit ist. Ziehen Sie die Griffe von rechts nach links.

## Erstellen einer Artikelseite 

Erstellen Sie anschließend eine Seite mithilfe der Artikelseiten-Vorlage. Verfassen Sie den Inhalt der Seite so, dass er mit den Mockups der Site übereinstimmt. Führen Sie die folgenden Schritte aus:

>[!VIDEO](https://video.tv.adobe.com/v/330993?quality=12&learn=on)

Allgemeine Schritte für das obige Video:

1. Navigieren Sie zur Sites-Konsole unter [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine).
1. Erstellen Sie eine Seite unter **WKND** > **US** > **EN** > **Magazine**.
   * Wählen Sie die **Artikelseite**-Vorlage.
   * Setzen Sie den **Titel** unter **Eigenschaften** auf „Ultimate Guide to LA Skateparks“ fest
   * Definieren Sie den **Namen** als „guide-la-skateparks“
1. Ersetzen Sie den Titel **By Author** durch den Text „By Stacey Roswells“.
1. Aktualisieren Sie die **Textkomponente**, um zum Ausfüllen des Artikels einen Absatz einzuschließen. Sie können die folgende Textdatei als Kopie verwenden: [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt).
1. Fügen Sie eine weitere **Textkomponente** hinzu.
   * Aktualisieren Sie die Komponente, um folgendes Zitat einzuschließen: „There is no better place to shred than Los Angeles.“
   * Bearbeiten Sie den Rich-Texteditor im Vollbildmodus und ändern Sie das obige Zitat, um das **Zitatblock**-Element zu verwenden.
1. Füllen Sie den Hauptteil des Artikels weiter aus, um die Modelle aufeinander abzustimmen.
1. Konfigurieren Sie die **Download**-Komponente so, dass sie eine PDF-Version des Artikels verwendet.
   * Kreuzen Sie unter **Download** > **Eigenschaften** das Kontrollkästchen **Titel aus dem DAM-Asset abrufen** an.
   * Definieren Sie die **Beschreibung** als: „Get the Full Story.“
   * Legen Sie den **Aktionstext** als „Download PDF“ fest.
1. Konfigurieren Sie die **Listenkomponente**.
   * Wählen Sie unter **Listeneinstellungen** > **Erstellen einer Liste mittels**: **Untergeordnete Seiten**.
   * Legen Sie die **übergeordnete Seite** auf `/content/wknd/us/en/magazine` fest.
   * Markieren Sie **Elemente verknüpfen** und **Datum anzeigen** unter **Einstellungen für Elemente**.

## Überprüfen der Knotenstruktur {#node-structure}

An diesem Punkt ist die Artikelseite offensichtlich unformatiert. Die Grundstruktur ist jedoch vorhanden. Überprüfen Sie anschließend die Knotenstruktur der Artikelseite, um ein besseres Verständnis der Rolle der Vorlage, Seite und der Komponenten zu erhalten.

Verwenden Sie das Tool CRXDE-Lite auf einer lokalen AEM-Instanz, um die zugrunde liegende Knotenstruktur anzuzeigen.

1. Öffnen Sie [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) und navigieren Sie mithilfe der Navigationsstruktur zu `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Klicken Sie auf den Knoten `jcr:content` unter `la-skateparks` und zeigen Sie die Eigenschaften an:

   ![JCR-Inhaltseigenschaften](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Beachten Sie den Wert für `cq:template`, der auf `/conf/wknd/settings/wcm/templates/article-page`, die zuvor erstellte Artikelseitenvorlage, verweist.

   Beachten Sie außerdem den Wert für `sling:resourceType`, der auf `wknd/components/page` verweist. Dies ist die Seitenkomponente, die vom AEM-Projektarchetyp erstellt wurde und für das Rendern der Seite basierend auf der Vorlage verantwortlich ist.

1. Erweitern Sie den Knoten `jcr:content` unter `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` und zeigen Sie die Knotenhierarchie an:

   ![JCR-Inhalt LA Skateparks](assets/pages-templates/page-jcr-structure.png)

   Sie sollten in der Lage sein, jedem Knoten erstellte Komponenten lose zuzuordnen. Finden Sie heraus, ob Sie die verschiedenen Layout-Container identifizieren können, indem Sie die Knoten mit dem Präfix `container` überprüfen.

1. Überprüfen Sie als Nächstes die Seitenkomponente unter `/apps/wknd/components/page`. So zeigen Sie die Komponenteneigenschaften in CRXDE Lite an:

   ![Eigenschaften der Seitenkomponenten](assets/pages-templates/page-component-properties.png)

   Es gibt nur zwei HTL-Skripte – `customfooterlibs.html` und `customheaderlibs.html` unterhalb der Seitenkomponente. *Wie rendert diese Komponente die Seite?*

   Die Eigenschaft `sling:resourceSuperType` verweist auf `core/wcm/components/page/v2/page`. Mithilfe dieser Eigenschaft erbt die WKND-Seitenkomponente **alle** Funktionen der Seitenkomponente der Kernkomponente. Dies ist das erste Beispiel eines [Proxy-Komponentenmusters](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=de#ProxyComponentPattern). Weitere Informationen finden Sie [hier](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=de).

1. Sehen Sie sich eine weitere Komponente innerhalb der WKND-Komponenten an, und zwar die `Breadcrumb`-Komponente aus `/apps/wknd/components/breadcrumb`. Beachten Sie, dass sich hier dieselbe `sling:resourceSuperType`-Eigenschaft findet, aber dieses Mal verweist sie auf `core/wcm/components/breadcrumb/v2/breadcrumb`. Dies ist ein weiteres Beispiel für die Verwendung des Proxy-Komponentenmusters zum Einschließen einer Kernkomponente. Tatsächlich sind alle Komponenten in der WKND-Code-Basis Proxys von AEM-Kernkomponenten (mit Ausnahme der benutzerdefinierten Demo-Komponente HelloWorld). Es empfiehlt sich, die Funktionalität der Kernkomponenten so weit wie möglich wiederzuverwenden, *bevor* benutzerdefinierter Code erstellt wird.

1. Sehen Sie sich als Nächstes die Kernkomponentenseite unter `/libs/core/wcm/components/page/v2/page` mithilfe von CRXDE Lite an:

   >[!NOTE]
   >
   > In AEM 6.5/6.4 befinden sich die Kernkomponenten unter `/apps/core/wcm/components`. In AEM as a Cloud Service befinden sich die Kernkomponenten unter `/libs` und werden automatisch aktualisiert.

   ![Kernkomponentenseite](assets/pages-templates/core-page-component-properties.png)

   Beachten Sie, dass unter dieser Seite viele Skriptdateien enthalten sind. Die Kernkomponentenseite enthält zahlreiche Funktionen. Diese Funktion ist in mehrere Skripte unterteilt, um die Wartung und Lesbarkeit zu erleichtern. Sie können die Einbindung der HTL-Skripte verfolgen, indem Sie die Datei `page.html` öffnen und nach `data-sly-include` suchen:

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

   Der andere Grund für das Aufschlüsseln des HTL-Codes in mehrere Skripte besteht darin, den Proxy-Komponenten das Überschreiben einzelner Skripte zu ermöglichen, um benutzerdefinierte Geschäftslogiken zu implementieren. Die HTL-Skripte `customfooterlibs.html` und `customheaderlibs.html` werden für den expliziten Zweck erstellt, durch die Implementierung von Projekten überschrieben zu werden.

   ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=de)In diesem Artikel[ erhalten Sie weitere Informationen darüber, wie die bearbeitbare Vorlage in das Rendering der Inhaltsseite einfließt.

1. Untersuchen Sie eine andere Kernkomponente, z. B. den Breadcrumb unter `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`. Sehen Sie sich das Skript `breadcrumb.html` an, um zu verstehen, wie das Markup für die Breadcrumb-Komponente letztendlich generiert wird.

## Speichern von Konfigurationen in der Quell-Code-Verwaltung {#configuration-persistence}

Häufig ist es insbesondere zu Beginn eines AEM-Projekts hilfreich, Konfigurationen wie Vorlagen und zugehörige Inhaltsrichtlinien zur Quell-Code-Verwaltung beizubehalten. Dadurch wird sichergestellt, dass alle Entwicklerinnen und Entwickler mit demselben Inhaltssatz und denselben Konfigurationen arbeiten, und zusätzliche Konsistenz zwischen Umgebungen gewährleistet. Sobald ein Projekt einen gewissen Reifegrad erreicht hat, kann die Verwaltung von Vorlagen einer speziellen Gruppe von Power-Benutzenden übertragen werden.


Im Moment werden Vorlagen wie andere Code-Abschnitte behandelt und die **Vorlage für Artikelseiten** als Teil des Projekts synchronisiert.
Bisher wird Code vom AEM-Projekt an eine lokale Instanz von AEM gesendet. Die **Artikelseitenvorlage** wurde direkt in einer lokalen Instanz von AEM erstellt, daher muss die Vorlage in das AEM-Projekt **importiert** werden. Das **ui.content**-Modul ist zu diesem Zweck im AEM-Projekt enthalten.

Die nächsten Schritte werden in der VSCode IDE mithilfe des Plug-ins [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview) ausgeführt. Sie können jedoch jede IDE dafür verwenden, die gemäß Ihrer Konfiguration **importieren** oder Inhalte aus einer lokalen Instanz von AEM importieren soll.

1. Öffnen Sie in VSCode das Projekt `aem-guides-wknd`.

1. Erweitern Sie das **ui.content**-Modul im Projekt-Explorer. Erweitern Sie den Ordner `src` und navigieren Sie zu `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Klicken Sie mit der rechten Maustaste] auf den Ordner `templates` und wählen Sie **Importieren aus AEM-Server**:

   ![VSCode-Importvorlage](assets/pages-templates/vscode-import-templates.png)

   Die `article-page` sollte importiert werden und die Vorlagen `page-content` sowie `xf-web-variation` sollten ebenfalls aktualisiert werden.

   ![Aktualisierte Vorlagen](assets/pages-templates/updated-templates.png)

1. Wiederholen Sie die Schritte, um Inhalte zu importieren, wählen Sie jedoch den Ordner **Richtlinien** aus `/conf/wknd/settings/wcm/policies`.

   ![VSCode-Importrichtlinien](assets/pages-templates/policies-article-page-template.png)

1. Sehen Sie sich die Datei `filter.xml` aus `ui.content/src/main/content/META-INF/vault/filter.xml` an.

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

   Die Datei `filter.xml` ist für die Identifizierung der Knotenpfade verantwortlich, die mit dem Paket installiert werden. Beachten Sie das `mode="merge"` bei jedem Filter, das anzeigt, dass der vorhandene Inhalt nicht geändert werden soll, sondern nur neue Inhalte hinzugefügt werden. Da Inhaltsautorinnen und -autoren diese Pfade möglicherweise aktualisieren, ist es wichtig, dass eine Code-Implementierung den Inhalt **nicht** überschreibt. Weitere Informationen zum Arbeiten mit Filterelementen finden Sie in der [FileVault-Dokumentation](https://jackrabbit.apache.org/filevault/filter.html).

   Vergleichen Sie `ui.content/src/main/content/META-INF/vault/filter.xml` und `ui.apps/src/main/content/META-INF/vault/filter.xml`, um die verschiedenen Knoten zu verstehen, die von den einzelnen Modulen verwaltet werden.

   >[!WARNING]
   >
   > Um konsistente Implementierungen der WKND-Referenz-Site sicherzustellen, werden einige Verzweigungen des Projekts so eingerichtet, dass `ui.content` alle Änderungen im JCR überschreibt. Dies erfolgt standardmäßig, d. h. für Lösungszweige, da Codes/Stile unter bestimmten Richtlinien geschrieben werden.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben eine Vorlage und eine Seite mit Adobe Experience Manager Sites erstellt!

### Nächste Schritte {#next-steps}

An diesem Punkt ist die Artikelseite offensichtlich unformatiert. Absolvieren Sie das Tutorial [Client-seitige Bibliotheken und Frontend-Workflow](client-side-libraries.md), um die Best Practices zum Einschließen von CSS und JavaScript zur Anwendung globaler Stile auf die Site und zur Integration eines dedizierten Frontend-Builds zu erlernen.

Sehen Sie sich den fertigen Code unter [GitHub](https://github.com/adobe/aem-guides-wknd) an oder prüfen und implementieren Sie den Code lokal in der Git-Verzweigung `tutorial/pages-templates-solution`.

1. Klonen Sie das Repository [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Checken Sie die Verzweigung `tutorial/pages-templates-solution` aus.

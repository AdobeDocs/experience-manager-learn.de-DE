---
title: Erste Schritte mit AEM Sites - Seiten und Vorlagen
seo-title: Erste Schritte mit AEM Sites - Seiten und Vorlagen
description: Erfahren Sie mehr über die Beziehung zwischen einer Basisseitenkomponente und bearbeitbaren Vorlagen. Erfahren Sie, wie die Kernkomponenten in das Projekt integriert werden, und lernen Sie erweiterte Richtlinienkonfigurationen bearbeitbarer Vorlagen kennen, um eine gut strukturierte Artikelseitenvorlage basierend auf einem Mock-up von Adobe XD zu erstellen.
sub-product: Sites
feature: template-editor, core-components
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
translation-type: tm+mt
source-git-commit: 836ef9b7f6a9dcb2ac78f5d1320797897931ef8c
workflow-type: tm+mt
source-wordcount: '2226'
ht-degree: 3%

---


# Seiten und Vorlagen {#pages-and-template}

In diesem Kapitel werden wir die Beziehung zwischen einer Basisseitenkomponente und bearbeitbaren Vorlagen untersuchen. Wir werden eine nicht formatierte Artikelvorlage basierend auf einigen Modellen von [AdobeXD](https://www.adobe.com/products/xd.html) erstellen. Beim Erstellen der Vorlage werden die Kernkomponenten und die erweiterten Richtlinienkonfigurationen der bearbeitbaren Vorlagen behandelt.

## Voraussetzungen {#prerequisites}

Überprüfen Sie die erforderlichen Werkzeuge und Anweisungen zum Einrichten einer [lokalen Entwicklungs-Umgebung](overview.md#local-dev-environment).

### Starterprojekt

Sehen Sie sich den Basiscode an, auf dem das Lernprogramm basiert:

1. Klonen Sie das [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd)-Repository.
1. Sehen Sie sich die Verzweigung `pages-templates/start` an.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
   $ cd ~/code/aem-guides-wknd
   $ git checkout pages-templates/start
   ```

1. Stellen Sie mithilfe Ihrer Maven-Fähigkeiten eine Codebasis für eine lokale AEM bereit:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd/tree/pages-templates/solution) oder lokal prüfen, indem Sie zur Verzweigung `pages-templates/solution` wechseln.

## Vorgabe

1. Inspect ein in Adobe XD erstelltes Seitendesign und ordnen Sie es Kernkomponenten zu.
1. Machen Sie sich mit den Details bearbeitbarer Vorlagen vertraut und erfahren Sie, wie Richtlinien verwendet werden können, um eine granulare Steuerung des Seiteninhalts zu erzwingen.
1. Erfahren Sie, wie Vorlagen und Seiten verknüpft werden

## Was Sie erstellen werden {#what-you-will-build}

In diesem Teil des Tutorials erstellen Sie eine neue Artikelseitenvorlage, mit der neue Artikelseiten erstellt und an einer gemeinsamen Struktur ausgerichtet werden können. Die Artikelseitenvorlage basiert auf Designs und einem UI-Kit, das in AdobeXD erstellt wurde. Dieses Kapitel konzentriert sich nur auf das Erstellen der Struktur oder des Skeletts der Vorlage. Es werden keine Stile implementiert, aber die Vorlage und die Seiten funktionieren.

![Artikelseitendesign und nicht formatierte Version](assets/pages-templates/what-you-will-build.png)

## Benutzeroberfläche mit Adobe XD {#adobexd}

In den meisten Fällen ist die Planung für einen neuen Website-Beginn mit Mockups und statischen Designs. [Adobe ](https://www.adobe.com/products/xd.html) XDist ein Design-Tool zum Erstellen von Benutzererlebnissen. Anschließend werden wir ein UI-Kit und -Modelle prüfen, um die Struktur der Artikelseitenvorlage zu planen.

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

Laden Sie die [WKND Article Design File](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd) herunter.

## Erstellen einer Kopf- und Fußzeile mit Erlebnisfragmenten {#experience-fragments}

Eine gängige Praxis bei der Erstellung globaler Inhalte, wie Kopf- oder Fußzeilen, ist die Verwendung eines [Erlebnisfragments](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Erlebnisfragmente ermöglichen es uns, mehrere Komponenten zu kombinieren, um eine einzelne, referenzierbare Komponente zu erstellen. Erlebnisfragmente haben den Vorteil, dass sie mehrseitige Verwaltung unterstützen, und ermöglichen es uns, verschiedene Kopf- und Fußzeilen pro Gebietsschema zu verwalten.

Als Nächstes aktualisieren wir das Erlebnisfragment, das als Kopf- und Fußzeile verwendet werden soll, um das WKND-Logo hinzuzufügen.

>[!VIDEO](https://video.tv.adobe.com/v/30215/?quality=12&learn=on)

>[!NOTE]
>
> Sehen Ihre Erlebnisfragmente anders aus als im Video? Versuchen Sie, sie zu löschen und die Code-Basis des Startprojekts erneut zu installieren.

Im Folgenden finden Sie die Schritte auf hoher Ebene, die im obigen Video ausgeführt werden.

1. Aktualisieren Sie die Kopfzeile &quot;Erlebnisfragment&quot;unter [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html), um das dunkle WKND-Logo einzuschließen.

   ![WKND Dunkles Logo](assets/pages-templates/wknd-logo-dk.png)

   *WKND Dark Logo*

1. Aktualisieren Sie die Experience Fragment-Kopfzeile unter [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html), um das WKND Light-Logo einzuschließen.

   ![WKND-Light-Logo](assets/pages-templates/wknd-logo-light.png)

   *WKND Light-Logo*

## Erstellen der Artikelseitenvorlage

Wenn Sie eine Seite erstellen, müssen Sie eine Vorlage auswählen. Diese wird als Grundlage für die Erstellung der neuen Seite verwendet. Die Vorlage definiert die Struktur der Zielseite, den anfänglichen Inhalt und die zulässigen Komponenten.

Es gibt 3 Hauptbereiche von [Bearbeitbare Vorlagen](https://docs.adobe.com/content/help/de/experience-manager-65/developing/platform/templates/page-templates-editable.html):

1. **Struktur** : Definiert Komponenten, die Teil der Vorlage sind. Diese können von Inhaltserstellern nicht bearbeitet werden.
1. **Anfänglicher Inhalt** : Definiert Komponenten, mit denen die Vorlage Beginn wird. Diese können von Inhaltserstellern bearbeitet und/oder gelöscht werden
1. **Richtlinien**  - legt Konfigurationen fest, wie sich Komponenten verhalten und welche Optionen Autoren zur Verfügung stehen.

Als Nächstes erstellen wir die Artikelseitenvorlage. Dies geschieht in einer lokalen Instanz von AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30217/?quality=12&learn=on)

Im Folgenden finden Sie die Schritte auf hoher Ebene, die im obigen Video ausgeführt werden.

1. Navigieren Sie zum Ordner &quot;WKND Sites Template&quot;: **Tools** > **Allgemein** > **Vorlagen** > **WKND-Site**
1. Erstellen Sie eine neue Vorlage mithilfe der **WKND Site Leere Seite** Vorlagentyp mit dem Titel **Artikelseitenvorlage**
1. Konfigurieren Sie die Vorlage im Modus **Struktur** so, dass sie die folgenden Elemente enthält:

   * Erlebnisfragment-Kopfzeile
   * Bild
   * Breadcrumb
   * Container - 8 Spalten breit Desktop, 12 Spalten breit Tablet, Mobil
   * Container - 4 Spalten breit, 12 Spalten breit Tablet, Mobil
   * Erlebnisfragment-Fußzeile

   ![Strukturmodus - Artikelseitenvorlage](assets/pages-templates/article-page-template-structure.png)

   *Struktur - Artikelseitenvorlage*

1. Wechseln Sie zum Modus **Anfänglicher Inhalt** und fügen Sie die folgenden Komponenten als Starterinhalt hinzu:

   * **Wichtigster Container**
      * Titel - Standardgröße von H1
      * Titel - *&quot;Nach Autorenname&quot;* mit einer Größe von H4
      * Text - leer
   * **Container**
      * Titel - *&quot;Diese Meldung teilen&quot;* mit einer Größe von H5
      * Freigabe in Social Media
      * Trennzeichen
      * Download
      * Liste

   ![Artikelseitenvorlage für den anfänglichen Inhaltsmodus](assets/pages-templates/article-page-template-initialcontent.png)

   *Anfänglicher Inhalt - Artikelseitenvorlage*

1. Aktualisieren Sie die **Eigenschaften der Anfangsseite**, um die Benutzerfreigabe für **Facebook** und **Pinterest** zu aktivieren.
1. Laden Sie ein Bild in die **Eigenschaften der Artikelseitenvorlage** hoch, um es leicht zu identifizieren:

   ![Miniaturansicht der Artikelvorlage](assets/pages-templates/article-page-template-thumbnail.png)

   *Miniaturansicht der Artikelvorlage*

1. Aktivieren Sie die Option **Artikelseitenvorlage** im Ordner [WKND-Site-Vorlagen](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd/settings/wcm/templates).

## Artikelseite erstellen

Nachdem wir nun eine Vorlage haben, erstellen wir eine neue Seite mit dieser Vorlage.

1. Laden Sie das folgende ZIP-Paket herunter: [WKND-PagesTemplates-DAM-Assets.zip](assets/pages-templates/WKND-PagesTemplates-DAM-Assets.zip) und installieren Sie es über [CRX Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   Mit dem obigen Paket werden unter `/content/dam/wknd/en/magazine/la-skateparks` mehrere Bilder und Assets installiert, um eine Artikelseite in späteren Schritten zu füllen.

   *Bilder und Assets im obigen Paket sind lizenzfrei mit der Gratifikation von  [Unsplash.com](https://unsplash.com/).*

   ![Beispiel-DAM-Assets](assets/pages-templates/sample-assets-la-skatepark.png)

1. Erstellen Sie unter **WKND** > **US** > **en** eine neue Seite mit dem Namen **Magazine**. Verwenden Sie die Vorlage **Inhaltsseite**.

   Auf dieser Seite wird eine Struktur zu unserer Site hinzugefügt, die es uns ermöglicht, die Breadcrumb-Komponente in Aktion zu sehen.

1. Anschließend erstellen Sie eine neue Seite unter **WKND** > **US** > **en** > **Zeitschrift**. Verwenden Sie die Vorlage **Artikelseite**. Verwenden Sie den Titel **Ultimate Guide to LA Skateparks** und den Namen **guide-la-skateparks**.

   ![Ursprünglich erstellte Artikelseite](assets/pages-templates/create-article-page-nocontent.png)

1. Füllen Sie die Seite mit Inhalten, um mit den unter [UI-Planung untersuchten Mustern mit dem AdobeXD](#adobexd)-Abschnitt übereinzustimmen. Beispielartikeltext kann [hier heruntergeladen werden](assets/pages-templates/la-skateparks-copy.txt). Sie sollten Folgendes erstellen können:

   ![Seite mit ausgefüllten Artikeln](assets/pages-templates/article-page-unstyled.png)

   >[!NOTE]
   >
   > Die Bildkomponente oben auf der Seite kann bearbeitet, aber nicht entfernt werden. Die Breadcrumb-Komponente wird auf der Seite angezeigt, kann jedoch nicht bearbeitet oder entfernt werden.

## Inspect der Knotenstruktur {#node-structure}

An dieser Stelle ist die Artikelseite eindeutig nicht formatiert. Die Grundstruktur ist jedoch vorhanden. Als Nächstes betrachten wir die Knotenstruktur der Artikelseite, um ein besseres Verständnis der Rolle der Vorlage und der Seitenkomponente, die für die Wiedergabe des Inhalts verantwortlich ist, zu erhalten.

Wir können dies mit dem CRXDE-Lite-Tool auf einer lokalen AEM tun.

1. Öffnen Sie [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) und navigieren Sie mit der Strukturnavigation zu `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Klicken Sie auf den Knoten `jcr:content` unter der Seite `la-skateparks` und Ansicht der Eigenschaften:

   ![JCR-Inhaltseigenschaften](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Beachten Sie den Wert für `cq:template`, der auf `/conf/wknd/settings/wcm/templates/article-page` verweist, die zuvor erstellte Artikelseitenvorlage.

   Beachten Sie auch den Wert von `sling:resourceType`, der auf `wknd/components/structure/page` verweist. Dies ist die vom AEM Projektarchiv erstellte Seitenkomponente, die für die Wiedergabe der Seite auf Basis der Vorlage verantwortlich ist.

1. Erweitern Sie den Knoten `jcr:content` unter `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` und Ansicht der Node-Hierarchie:

   ![JCR Content LA Skateparks](assets/pages-templates/page-jcr-structure.png)

   Sie sollten in der Lage sein, jeden Knoten lose Komponenten zuzuordnen, die erstellt wurden. Überprüfen Sie, ob Sie die verschiedenen Layout-Container identifizieren können, indem Sie die mit `responsivegrid` präfixierten Knoten überprüfen.

1. Überprüfen Sie anschließend die Seitenkomponente bei `/apps/wknd/components/structure/page`. Ansicht der Komponenteneigenschaften in der CRXDE Lite:

   ![Eigenschaften von Seitenkomponenten](assets/pages-templates/page-component-properties.png)

   Beachten Sie, dass sich die Seitenkomponente unter dem Ordner **structure** befindet. Diese Regel entspricht dem Strukturmodus des Vorlageneditors und wird verwendet, um anzugeben, dass die Seitenkomponente nicht direkt mit Autoren interagiert.

   Beachten Sie, dass unter der Seitenkomponente nur 2 HTML-Skripte, `customfooterlibs.html` und `customheaderlibs.html` stehen. Wie also rendert diese Komponente die Seite?

   Beachten Sie die Eigenschaft `sling:resourceSuperType` und den Wert von `core/wcm/components/page/v2/page`. Mit dieser Eigenschaft kann die Seitenkomponente des WKND alle Funktionen der Komponente &quot;Core Component&quot;übernehmen. Dies ist das erste Beispiel für das [Proxy Component Pattern](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern). Weitere Informationen finden Sie [hier.](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html).

1. Inspect eine weitere Komponente innerhalb der WKND-Komponenten, die `Breadcrumb`-Komponente, die sich unter: `/apps/wknd/components/content/breadcrumb`. Beachten Sie, dass dieselbe `sling:resourceSuperType`-Eigenschaft gefunden werden kann, diesmal jedoch auf `core/wcm/components/breadcrumb/v2/breadcrumb` verweist. Dies ist ein weiteres Beispiel für die Verwendung des Musters der Proxy-Komponente, um eine Core-Komponente einzuschließen. Tatsächlich sind alle Komponenten in der WKND-Codebasis Stellvertreter von AEM Core Components (mit Ausnahme unserer berühmten HelloWorld-Komponente). Es empfiehlt sich, möglichst viele Funktionen von Kernkomponenten erneut zu verwenden, bevor *benutzerdefinierter Code geschrieben wird.*

1. Überprüfen Sie anschließend die Core Component Page bei `/apps/core/wcm/components/page/v2/page` mithilfe der CRXDE Lite:

   ![Core-Komponentenseite](assets/pages-templates/core-page-component-properties.png)

   Beachten Sie, dass viele weitere Skripte unter dieser Seite enthalten sind. Die Seite &quot;Core-Komponenten&quot;enthält viele Funktionen. Diese Funktion ist in mehrere Skripten unterteilt, um die Wartung und Lesbarkeit zu erleichtern. Sie können die Einbindung der HTML-Skripte verfolgen, indem Sie `page.html` öffnen und nach `data-sly-include` suchen:

   ```html
   <!--/* /apps/core/wcm/components/page/v2/page/page.html */-->
   <!DOCTYPE HTML>
   <html data-sly-use.page="com.adobe.cq.wcm.core.components.models.Page" lang="${page.language}"
           data-sly-use.head="head.html"
           data-sly-use.footer="footer.html"
           data-sly-use.redirect="redirect.html">
       <head data-sly-call="${head.head @ page = page}"></head>
       <body class="${page.cssClassNames}">
           <sly data-sly-test.isRedirectPage="${page.redirectTarget && (wcmmode.edit || wcmmode.preview)}"
               data-sly-call="${redirect.redirect @ redirectTarget = page.redirectTarget}"></sly>
           <sly data-sly-test="${!isRedirectPage}">
               <sly data-sly-include="body.socialmedia_begin.html"></sly>
               <sly data-sly-include="body.html"></sly>
               <sly data-sly-call="${footer.footer @ page = page}"></sly>
               <sly data-sly-include="body.socialmedia_end.html"></sly>
           </sly>
       </body>
   </html>
   ```

   Der andere Grund für das Ausbrechen der HTML in mehrere Skripten besteht darin, dass die Proxy-Komponenten einzelne Skripte überschreiben können, um benutzerdefinierte Geschäftslogik zu implementieren. Die HTML-Skripten `customfooterlibs.html` und `customheaderlibs.html` werden für den expliziten Zweck erstellt, durch die Implementierung von Projekten außer Kraft gesetzt zu werden.

   Weitere Informationen dazu, wie die bearbeitbare Vorlage in die Darstellung der [Inhaltsseite eingebunden wird, finden Sie in diesem Artikel](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/templates/page-templates-editable.html#resultant-content-pages).

1. Inspect die andere Core-Komponente, wie die Breadcrumb bei `/apps/core/wcm/components/breadcrumb/v2/breadcrumb`. Ansicht des Skripts `breadcrumb.html`, um zu verstehen, wie das Markup für die Breadcrumb-Komponente letztendlich generiert wird.

## Speichern von Konfigurationen in der Quellcodeverwaltung {#configuration-persistence}

In vielen Fällen, besonders zu Beginn eines AEM Projekts, ist es nützlich, Konfigurationen wie Vorlagen und zugehörige Inhaltsrichtlinien zur Quellcodeverwaltung beizubehalten. Dadurch wird sichergestellt, dass alle Entwickler mit demselben Inhaltssatz und denselben Konfigurationen arbeiten und zusätzliche Konsistenz zwischen den Umgebung sicherstellen. Sobald ein Projekt eine gewisse Reife erreicht hat, kann die Verwaltung von Vorlagen einer speziellen Gruppe von Stromverbrauchern überlassen werden.

Vorerst behandeln wir die Vorlagen wie andere Codeabschnitte und synchronisieren die **Article Page Template** als Teil des Projekts. Bisher haben wir **Code** aus unserem AEM Projekt in eine lokale Instanz von AEM verschoben. Die **Artikelseitenvorlage** wurde direkt auf einer lokalen Instanz von AEM erstellt. Daher müssen wir **ziehen** oder die Vorlage in unser AEM Projekt importieren. Das Modul **ui.content** ist zu diesem Zweck im AEM Projekt enthalten.

Die nächsten Schritte werden mit der Eclipse-IDE ausgeführt. Sie können jedoch jede IDE verwenden, die Sie für **ull** konfiguriert haben, oder Inhalte aus einer lokalen Instanz von AEM importieren.

1. Stellen Sie in der Eclipse-IDE sicher, dass ein Server mit dem AEM Developer Tool-Plug-In für die Verbindung zur lokalen Instanz von AEM gestartet wurde und dass das **ui.content**-Modul zur Serverkonfiguration hinzugefügt wurde.

   ![Eclipse-Serververbindung](assets/pages-templates/eclipse-server-started.png)

1. Erweitern Sie das Modul **ui.content** im Project Explorer. Erweitern Sie den Ordner `src` (den Ordner mit dem kleinen Globussymbol) und navigieren Sie zu `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Klicken Sie mit der rechten Maustaste ] auf den  `templates` Knoten und wählen Sie  **Aus Server importieren...**:

   ![Eclipse-Importvorlage](assets/pages-templates/eclipse-import-templates.png)

   Bestätigen Sie das Dialogfeld **Aus Repository importieren** und klicken Sie auf **Fertig stellen**. Sie sollten nun den Ordner `article-page-template` unter dem Ordner `templates` sehen.

1. Wiederholen Sie die Schritte zum Importieren von Inhalten, wählen Sie jedoch den Knoten **policies** unter `/conf/wknd/settings/wcm/policies` aus.

   ![Eclipse-Importrichtlinien](assets/pages-templates/policies-article-page-template.png)

1. Inspect Sie die Datei `filter.xml` unter `src/main/content/META-INF/vault/filter.xml`.

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

   Die `filter.xml`-Datei ist dafür verantwortlich, die Pfade von Knoten zu identifizieren, die mit dem Paket installiert werden. Beachten Sie, dass `mode="merge"` auf jedem der Filter angezeigt wird, dass vorhandene Inhalte nicht geändert werden, sondern nur neue Inhalte hinzugefügt werden. Da Inhaltsersteller diese Pfade möglicherweise aktualisieren, ist es wichtig, dass bei einer Codebereitstellung **kein** Inhalt überschrieben wird. Weitere Informationen zum Arbeiten mit Filterelementen finden Sie in der [FileVault-Dokumentation](https://jackrabbit.apache.org/filevault/filter.html).

   Vergleichen Sie `ui.content/src/main/content/META-INF/vault/filter.xml` und `ui.apps/src/main/content/META-INF/vault/filter.xml`, um die verschiedenen Knoten zu verstehen, die von den einzelnen Modulen verwaltet werden.

   >[!WARNING]
   >
   > Um eine konsistente Bereitstellung für die WKND-Referenz-Website sicherzustellen, werden einige Zweige des Projekts so eingerichtet, dass `ui.content` alle Änderungen in der JCR überschreiben. Dies erfolgt standardmäßig, d. h. für Lösungsverzweigungen, da Code/Stile für bestimmte Richtlinien geschrieben werden.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gerade eine neue Vorlage und Seite mit Adobe Experience Manager Sites erstellt.

### Nächste Schritte {#next-steps}

An dieser Stelle ist die Artikelseite eindeutig nicht formatiert. Folgen Sie dem Lernprogramm [Clientseitige Bibliotheken und Front-End-Workflow](client-side-libraries.md), um die Best Practices für die Einbeziehung von CSS und JavaScript zu lernen, um globale Stile auf die Site anzuwenden und einen dedizierten Front-End-Build zu integrieren.

Ansicht des fertigen Codes auf [GitHub](https://github.com/adobe/aem-guides-wknd) oder lokale Überprüfung und Bereitstellung des Codes in der Git-Klammer `pages-templates/solution`.

1. Klonen Sie das [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)-Repository.
1. Sehen Sie sich die Verzweigung `pages-templates/solution` an.

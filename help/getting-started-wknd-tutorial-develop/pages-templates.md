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
source-git-commit: e03d84f92be11623704602fb448273e461c70b4e
workflow-type: tm+mt
source-wordcount: '1724'
ht-degree: 2%

---


# Seiten und Vorlagen {#pages-and-template}

In diesem Kapitel werden wir die Beziehung zwischen einer Basisseitenkomponente und bearbeitbaren Vorlagen untersuchen. Wir werden eine nicht formatierte Artikelvorlage basierend auf einigen Modellen von [AdobeXD](https://www.adobe.com/products/xd.html) erstellen. Beim Erstellen der Vorlage werden die Kernkomponenten und die erweiterten Richtlinienkonfigurationen der bearbeitbaren Vorlagen behandelt.

## Voraussetzungen {#prerequisites}

Überprüfen Sie die erforderlichen Werkzeuge und Anweisungen zum Einrichten einer [lokalen Entwicklungs-Umgebung](overview.md#local-dev-environment).

### Starterprojekt

>[!NOTE]
>
> Wenn Sie das vorherige Kapitel erfolgreich abgeschlossen haben, können Sie das Projekt erneut verwenden und die Schritte zum Auschecken des Startprojekts überspringen.

Sehen Sie sich den Basiscode an, auf dem das Lernprogramm basiert:

1. Sehen Sie sich die Verzweigung `tutorial/pages-templates-start` von [GitHub](https://github.com/adobe/aem-guides-wknd) an.

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
   ```

1. Stellen Sie mithilfe Ihrer Maven-Fähigkeiten eine Codebasis für eine lokale AEM bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Wenn Sie AEM 6.5 oder 6.4 verwenden, hängen Sie das `classic`-Profil an beliebige Maven-Befehle an.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd/tree/pages-templates/solution) oder lokal prüfen, indem Sie zur Verzweigung `tutorial/pages-templates-solution` wechseln.

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

**Laden Sie die  [WKND-Artikelentwurfsdatei](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)** herunter.

## Erstellen der Artikelseitenvorlage

Wenn Sie eine Seite erstellen, müssen Sie eine Vorlage auswählen. Diese wird als Grundlage für die Erstellung der neuen Seite verwendet. Die Vorlage definiert die Struktur der Zielseite, den anfänglichen Inhalt und die zulässigen Komponenten.

Es gibt 3 Hauptbereiche von [Bearbeitbare Vorlagen](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html):

1. **Struktur** : Definiert Komponenten, die Teil der Vorlage sind. Diese können von Inhaltserstellern nicht bearbeitet werden.
1. **Anfänglicher Inhalt** : Definiert Komponenten, mit denen die Vorlage Beginn wird. Diese können von Inhaltserstellern bearbeitet und/oder gelöscht werden
1. **Richtlinien**  - legt Konfigurationen fest, wie sich Komponenten verhalten und welche Optionen Autoren zur Verfügung stehen.

Erstellen Sie anschließend eine neue Vorlage in AEM, die der Struktur der Sicherungen entspricht. Dies geschieht in einer lokalen Instanz von AEM. Gehen Sie wie folgt vor:

>[!VIDEO](https://video.tv.adobe.com/v/330991/?quality=12&learn=on)

## Kopf- und Fußzeile mit Erlebnisfragmenten aktualisieren {#experience-fragments}

Eine gängige Praxis bei der Erstellung globaler Inhalte, wie Kopf- oder Fußzeilen, ist die Verwendung eines [Erlebnisfragments](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Erlebnisfragmente ermöglichen es Benutzern, mehrere Komponenten zu kombinieren, um eine einzelne, referenzierbare Komponente zu erstellen. Erlebnisfragmente haben den Vorteil, dass die Verwaltung mehrerer Sites und [lokale Anpassung](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure) unterstützt werden.

Der AEM-Projektarchiv generiert eine Kopf- und Fußzeile. Aktualisieren Sie anschließend die Erlebnisfragmente, um sie mit den Sicherungen abzustimmen. Gehen Sie wie folgt vor:

>[!VIDEO](https://video.tv.adobe.com/v/330992/?quality=12&learn=on)

Laden Sie das Beispielinhaltspaket **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets.zip)** herunter und installieren Sie es.

## Artikelseite erstellen

Erstellen Sie anschließend eine neue Seite mit der Vorlage &quot;Artikelseite&quot;. Verfassen Sie den Inhalt der Seite so, dass er mit den Site-Modellen übereinstimmt. Gehen Sie wie folgt vor:

>[!VIDEO](https://video.tv.adobe.com/v/330993/?quality=12&learn=on)

## Inspect der Knotenstruktur {#node-structure}

An dieser Stelle ist die Artikelseite eindeutig nicht formatiert. Die Grundstruktur ist jedoch vorhanden. Überprüfen Sie anschließend die Knotenstruktur der Artikelseite, um ein besseres Verständnis der Rolle der Vorlage, Seite und Komponenten zu erhalten.

Verwenden Sie das CRXDE-Lite-Tool auf einer lokalen AEM, um die zugrunde liegende Knotenstruktur Ansicht.

1. Öffnen Sie [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) und navigieren Sie mit der Strukturnavigation zu `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Klicken Sie auf den Knoten `jcr:content` unter der Seite `la-skateparks` und Ansicht der Eigenschaften:

   ![JCR-Inhaltseigenschaften](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Beachten Sie den Wert für `cq:template`, der auf `/conf/wknd/settings/wcm/templates/article-page` verweist, die zuvor erstellte Artikelseitenvorlage.

   Beachten Sie auch den Wert von `sling:resourceType`, der auf `wknd/components/page` verweist. Dies ist die vom AEM Projektarchiv erstellte Seitenkomponente, die für die Wiedergabe der Seite auf Basis der Vorlage verantwortlich ist.

1. Erweitern Sie den Knoten `jcr:content` unter `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` und Ansicht der Node-Hierarchie:

   ![JCR Content LA Skateparks](assets/pages-templates/page-jcr-structure.png)

   Sie sollten in der Lage sein, jeden Knoten lose Komponenten zuzuordnen, die erstellt wurden. Überprüfen Sie, ob Sie die verschiedenen Layout-Container identifizieren können, indem Sie die mit `container` präfixierten Knoten überprüfen.

1. Überprüfen Sie anschließend die Seitenkomponente bei `/apps/wknd/components/page`. Ansicht der Komponenteneigenschaften in der CRXDE Lite:

   ![Eigenschaften von Seitenkomponenten](assets/pages-templates/page-component-properties.png)

   Beachten Sie, dass unter der Seitenkomponente nur 2 HTML-Skripte, `customfooterlibs.html` und `customheaderlibs.html` stehen. *Wie also rendert diese Komponente die Seite?*

   Die `sling:resourceSuperType`-Eigenschaft verweist auf `core/wcm/components/page/v2/page`. Mit dieser Eigenschaft kann die Seitenkomponente der WKND **alle** der Funktionalität der Seitenkomponente &quot;Core Component&quot;übernehmen. Dies ist das erste Beispiel für das [Proxy Component Pattern](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern). Weitere Informationen finden Sie [hier.](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html).

1. Inspect eine weitere Komponente innerhalb der WKND-Komponenten, die `Breadcrumb`-Komponente, die sich unter: `/apps/wknd/components/breadcrumb`. Beachten Sie, dass dieselbe `sling:resourceSuperType`-Eigenschaft gefunden werden kann, diesmal jedoch auf `core/wcm/components/breadcrumb/v2/breadcrumb` verweist. Dies ist ein weiteres Beispiel für die Verwendung des Musters der Proxy-Komponente, um eine Core-Komponente einzuschließen. Tatsächlich sind alle Komponenten in der WKND-Codebasis Stellvertreter von AEM Core Components (mit Ausnahme unserer berühmten HelloWorld-Komponente). Es empfiehlt sich, möglichst viele Funktionen von Kernkomponenten erneut zu verwenden, bevor *benutzerdefinierter Code geschrieben wird.*

1. Überprüfen Sie anschließend die Core Component Page bei `/libs/core/wcm/components/page/v2/page` mithilfe der CRXDE Lite:

   >[!NOTE]
   >
   > In AEM 6.5/6.4 befinden sich die Kernkomponenten unter `/apps/core/wcm/components`. In AEM als Cloud Service befinden sich die Kernkomponenten unter `/libs` und werden automatisch aktualisiert.

   ![Core-Komponentenseite](assets/pages-templates/core-page-component-properties.png)

   Beachten Sie, dass viele weitere Skripte unter dieser Seite enthalten sind. Die Seite &quot;Core-Komponenten&quot;enthält viele Funktionen. Diese Funktion ist in mehrere Skripten unterteilt, um die Wartung und Lesbarkeit zu erleichtern. Sie können die Einbindung der HTML-Skripte verfolgen, indem Sie `page.html` öffnen und nach `data-sly-include` suchen:

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

   Der andere Grund für das Ausbrechen der HTML in mehrere Skripten besteht darin, dass die Proxy-Komponenten einzelne Skripte überschreiben können, um benutzerdefinierte Geschäftslogik zu implementieren. Die HTML-Skripten `customfooterlibs.html` und `customheaderlibs.html` werden für den expliziten Zweck erstellt, durch die Implementierung von Projekten außer Kraft gesetzt zu werden.

   Weitere Informationen dazu, wie die bearbeitbare Vorlage in die Darstellung der [Inhaltsseite eingebunden wird, finden Sie in diesem Artikel](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html).

1. Inspect die andere Core-Komponente, wie die Breadcrumb bei `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`. Ansicht des Skripts `breadcrumb.html`, um zu verstehen, wie das Markup für die Breadcrumb-Komponente letztendlich generiert wird.

## Speichern von Konfigurationen in der Quellcodeverwaltung {#configuration-persistence}

In vielen Fällen, besonders zu Beginn eines AEM Projekts, ist es nützlich, Konfigurationen wie Vorlagen und zugehörige Inhaltsrichtlinien zur Quellcodeverwaltung beizubehalten. Dadurch wird sichergestellt, dass alle Entwickler mit demselben Inhaltssatz und denselben Konfigurationen arbeiten und zusätzliche Konsistenz zwischen den Umgebung sicherstellen. Sobald ein Projekt eine gewisse Reife erreicht hat, kann die Verwaltung von Vorlagen einer speziellen Gruppe von Stromverbrauchern überlassen werden.

Vorerst behandeln wir die Vorlagen wie andere Codeabschnitte und synchronisieren die **Article Page Template** als Teil des Projekts. Bisher haben wir **Code** aus unserem AEM Projekt in eine lokale Instanz von AEM verschoben. Die **Artikelseitenvorlage** wurde direkt auf einer lokalen Instanz von AEM erstellt. Daher müssen wir **die Vorlage in unser AEM Projekt importieren.** Das Modul **ui.content** ist zu diesem Zweck im AEM Projekt enthalten.

Die nächsten Schritte werden mit der VSCode IDE mit dem Plugin [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview) durchgeführt. Sie können jedoch jede IDE verwenden, die Sie für **import** konfiguriert haben, oder Inhalte aus einer lokalen Instanz von AEM importieren.

1. Öffnen Sie im VSCode das Projekt `aem-guides-wknd`.

1. Erweitern Sie das Modul **ui.content** im Project Explorer. Erweitern Sie den Ordner `src` und navigieren Sie zu `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Klicken Sie mit der rechten ] Maustaste auf den  `templates` Ordner und wählen Sie  **Aus AEM Server** importieren:

   ![VSCode-Importvorlage](assets/pages-templates/vscode-import-templates.png)

   Die `article-page`-Vorlagen sollten importiert und die `page-content`-, `xf-web-variation`-Vorlagen sollten ebenfalls aktualisiert werden.

   ![Aktualisierte Vorlagen](assets/pages-templates/updated-templates.png)

1. Wiederholen Sie die Schritte zum Importieren von Inhalten, wählen Sie jedoch den Ordner **policies** unter `/conf/wknd/settings/wcm/policies` aus.

   ![VSCode-Importrichtlinien](assets/pages-templates/policies-article-page-template.png)

1. Inspect Sie die Datei `filter.xml` unter `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

Ansicht des fertigen Codes auf [GitHub](https://github.com/adobe/aem-guides-wknd) oder lokale Überprüfung und Bereitstellung des Codes in der Git-Klammer `tutorial/pages-templates-solution`.

1. Klonen Sie das [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)-Repository.
1. Sehen Sie sich die Verzweigung `tutorial/pages-templates-solution` an.

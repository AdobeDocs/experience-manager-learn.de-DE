---
title: Implementierungshandbuch für die einfache Suche
description: Bei der Implementierung der einfachen Suche handelt es sich um die Materialien vom Summit Lab „AEM Search Demystified“ von 2017. Diese Seite enthält die Materialien dieses Labs. Eine Lab-Führung finden Sie in der Lab-Arbeitsmappe im Präsentationsabschnitt dieser Seite.
version: 6.4, 6.5
feature: Search
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: aa268c5f-d29e-4868-a58b-444379cb83be
last-substantial-update: 2022-08-10T00:00:00Z
thumbnail: 32090.jpg
duration: 182
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 100%

---

# Implementierungshandbuch für die einfache Suche{#simple-search-implementation-guide}

Bei der Implementierung der einfachen Suche handelt es sich um die Materialien vom **Adobe Summit Lab „AEM Search Demystified“**. Diese Seite enthält die Materialien dieses Labs. Eine Lab-Führung finden Sie in der Lab-Arbeitsmappe im Präsentationsabschnitt dieser Seite.

![Überblick über die Sucharchitektur](assets/l4080/simple-search-application.png)

## Präsentationsmaterialien {#bookmarks}

* [Lab-Arbeitsmappe](assets/l4080/l4080-lab-workbook.pdf)
* [Präsentation](assets/l4080/l4080-presentation.pdf)

## Lesezeichen {#bookmarks-1}

### Tools {#tools}

* [Index-Manager](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_oakindexmanager)
* [Erläutern der Abfrage](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_queryperformance)
* [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/cqPageLucene) > /oak:index/cqPageLucene
* [CRX Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* [QueryBuilder-Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?)
* [Oak Index Definition Generator](https://oakutils.appspot.com/generate/index)

### Kapitel {#chapters}

*Bei den folgenden Kapitel-Links wird davon ausgegangen, dass die [anfänglichen Pakete](#initialpackages) in AEM Author unter`http://localhost:4502`* installiert sind.

* [Kapitel 1](http://localhost:4502/editor.html/content/summit/l4080/chapter-1.html)
* [Kapitel 2](http://localhost:4502/editor.html/content/summit/l4080/chapter-2.html)
* [Kapitel3](http://localhost:4502/editor.html/content/summit/l4080/chapter-3.html)
* [Kapitel 4](http://localhost:4502/editor.html/content/summit/l4080/chapter-4.html)
* [Kapitel 5](http://localhost:4502/editor.html/content/summit/l4080/chapter-5.html)
* [Kapitel 6](http://localhost:4502/editor.html/content/summit/l4080/chapter-6.html)
* [Kapitel 7](http://localhost:4502/editor.html/content/summit/l4080/chapter-7.html)
* [Kapitel 8](http://localhost:4502/editor.html/content/summit/l4080/chapter-8.html)
* [Kapitel 9](http://localhost:4502/editor.html/content/summit/l4080/chapter-9.html)

## Pakete {#packages}

### Anfängliche Pakete {#initial-packages}

* [Tags](assets/l4080/summit-tags.zip)
* [Anwendungspaket für die einfache Suche](assets/l4080/simple.ui.apps-0.0.1-snapshot.zip)

### Kapitelpakete {#chapter-packages}

* [Lösung Kapitel 1](assets/l4080/l4080-chapter1.zip)
* [Lösung Kapitel 2](assets/l4080/l4080-chapter2.zip)
* [Lösung Kapitel 3](assets/l4080/l4080-chapter3.zip)
* [Lösung Kapitel 4](assets/l4080/l4080-chapter4.zip)
* [Setup Kapitel 5](assets/l4080/l4080-chapter5-setup.zip)
* [Lösung Kapitel 5](assets/l4080/l4080-chapter5-solution.zip)
* [Lösung Kapitel 6](assets/l4080/l4080-chapter6.zip)
* [Lösung Kapitel 9](assets/l4080/l4080-chapter9.zip)

## Referenzierte Materialien {#reference-materials}

* [GitHub-Repository](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Sling-Modelle](https://sling.apache.org/documentation/bundles/models.html)
* [Sling-Modell-Exporter](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [QueryBuilder-API](https://experienceleague.adobe.com/docs/?lang=de)
* [AEM-Chrome-Plug-in](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode) ([Dokumentationsseite](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/))

## Korrekturen und Follow-up {#corrections-and-follow-up}

Korrekturen und Erläuterungen aus den Lab-Diskussionen und Antworten auf weitere Fragen von Teilnehmerinnen und Teilnehmern.

1. **Wie lässt sich eine Neuindizierung stoppen?**

   Die Neuindizierung kann über IndexStats MBean gestoppt werden, verfügbar über [AEM Web Console > JMX](http://localhost:4502/system/console/jmx).

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * Führen Sie `abortAndPause()` aus, um die Neuindizierung abzubrechen. Dadurch wird der Index für eine weitere Neuindizierung so lange gesperrt, bis `resume()` aufgerufen wird.
      * Durch Ausführen von `resume()` wird der Indizierungsprozess neu gestartet.
   * Dokumentation: [https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **Wie können Oak-Indizes mehrere Mandanten unterstützen?**

   Oak unterstützt das Platzieren von Indizes überall in der Inhaltsstruktur, und diese Indizes indizieren nur innerhalb dieser Unterstruktur. Beispielsweise kann **`/content/site-a/oak:index/cqPageLucene`** erstellt werden, um Inhalte nur unter **`/content/site-a`zu indizieren.**

   Ein gleichwertiger Ansatz ist die Anwendung der Eigenschaften **`includePaths`** und **`queryPaths`** auf einen Index unter **`/oak:index`**. Zum Beispiel:

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   Bei diesem Ansatz sind folgende Aspekte zu berücksichtigen:

   * Abfragen MÜSSEN eine Pfadbeschränkung angeben, die dem Abfragepfadbereich des Index entspricht, oder diesem untergeordnet sein.
   * Umfassendere Indizes (z. B. `/oak:index/cqPageLucene`) indizieren EBENFALLS die Daten, was zu einer doppelten Erfassung und entsprechenden Kosten bei der Festplattennutzung führt.
   * Gegebenenfalls ist eine doppelte Konfigurationsverwaltung erforderlich (z. B. durch Hinzufügen von identischen indexRules über mehrere Mandanten-Indizes hinweg, wenn sie denselben Abfragesätzen entsprechen müssen).
   * Dieser Ansatz ist am besten auf der AEM-Veröffentlichungsebene für die benutzerdefinierte Site-Suche geeignet, da es in AEM Author üblich ist, Abfragen oben in der Inhaltsstruktur für verschiedene Mandanten auszuführen (z. B. über OmniSearch). Unterschiedliche Indexdefinitionen können zu unterschiedlichem Verhalten führen, das nur auf der Pfadbeschränkung basiert.

3. **Wo ist eine Liste aller verfügbaren Analyzer?**

   Oak macht eine Reihe von Lucene-bereitgestellten Analyzer-Konfigurationselementen zur Verwendung in AEM verfügbar.

   * [Dokumentation zu Apache Oak-Analyzern](https://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [Tokenizer](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [Filter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [CharFilter](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **Wie kann in derselben Abfrage nach Seiten und Assets gesucht werden?**

   In AEM 6.3 gibt es nun die Möglichkeit, mehrere Knotentypen in derselben bereitgestellten Abfrage abzufragen. Ein Beispiel ist die folgende QueryBuilder-Abfrage. Beachten Sie, dass jede Unterabfrage in einen eigenen Index aufgelöst werden kann. Hier wird etwa die Unterabfrage `cq:Page` in `/oak:index/cqPageLucene` und die Unterabfrage `dam:Asset` in `/oak:index/damAssetLucene` aufgelöst.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   Daraus ergeben sich die folgende Abfrage und der folgende Abfrageplan:

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   Untersuchen Sie die Abfrage und Ergebnisse über den [QueryBuilder-Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group) und das [AEM-Chrome-Plug-in](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=de-DE).

5. **Wie kann in derselben Abfrage über mehrere Pfade hinweg gesucht werden?**

   In AEM 6.3 gibt es nun die Möglichkeit, gewünschte Informationen über mehrere Pfade hinweg in derselben bereitgestellten Abfrage abzufragen. Ein Beispiel ist die folgende QueryBuilder-Abfrage. Beachten Sie, dass jede Unterabfrage in einen eigenen Index aufgelöst werden kann.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   Daraus ergeben sich die folgende Abfrage und der folgende Abfrageplan:

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   Untersuchen Sie die Abfrage und Ergebnisse über den [QueryBuilder-Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group) und das [AEM-Chrome-Plug-in](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=de-DE).

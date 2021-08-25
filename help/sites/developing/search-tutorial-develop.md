---
title: Implementierungshandbuch für einfache Suchen
description: Bei der Implementierung der einfachen Suche handelt es sich um die Materialien vom Summit Lab „AEM Search Demystified“ von 2017. Diese Seite enthält die Materialien aus diesem Labor. Eine Führung durch das Labor erhalten Sie in der Arbeitsmappe für den Lab im Abschnitt Präsentation dieser Seite.
version: 6.3, 6.4, 6.5
feature: Search
topic: Development
role: Developer
level: Intermediate, Experienced
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '897'
ht-degree: 11%

---


# Implementierungshandbuch für einfache Suchen{#simple-search-implementation-guide}

Bei der Implementierung der einfachen Suche handelt es sich um die Materialien aus dem **Adobe Summit-Labor AEM Search Demystified**. Diese Seite enthält die Materialien aus diesem Labor. Eine Führung durch das Labor erhalten Sie in der Arbeitsmappe für den Lab im Abschnitt Präsentation dieser Seite.

![Sucharchitektur - Überblick](assets/l4080/simple-search-application.png)

## Präsentationsmaterialien {#bookmarks}

* [Lab-Arbeitsmappe](assets/l4080/l4080-lab-workbook.pdf)
* [Präsentation](assets/l4080/l4080-presentation.pdf)

## Lesezeichen {#bookmarks-1}

### Tools {#tools}

* [Index-Manager](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_oakindexmanager)
* [Abfrage erläutern](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_queryperformance)
* [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/cqPageLucene)  > /oak:index/cqPageLucene
* [CRX Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* [QueryBuilder-Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?)
* [Oak Index Definition Generator](https://oakutils.appspot.com/generate/index)

### Kapitel {#chapters}

*Die folgenden Kapitellinks gehen davon aus, dass die  [anfänglichen ](#initialpackages) Pakete auf der AEM-Autoreninstanz unter`http://localhost:4502`*

* [Kapitel 1](http://localhost:4502/editor.html/content/summit/l4080/chapter-1.html)
* [Kapitel 2](http://localhost:4502/editor.html/content/summit/l4080/chapter-2.html)
* [Kapitel 3](http://localhost:4502/editor.html/content/summit/l4080/chapter-3.html)
* [Kapitel 4](http://localhost:4502/editor.html/content/summit/l4080/chapter-4.html)
* [Kapitel 5](http://localhost:4502/editor.html/content/summit/l4080/chapter-5.html)
* [Kapitel 6](http://localhost:4502/editor.html/content/summit/l4080/chapter-6.html)
* [Kapitel 7](http://localhost:4502/editor.html/content/summit/l4080/chapter-7.html)
* [Kapitel 8](http://localhost:4502/editor.html/content/summit/l4080/chapter-8.html)
* [Kapitel 9](http://localhost:4502/editor.html/content/summit/l4080/chapter-9.html)

## Pakete {#packages}

### Erstpakete {#initial-packages}

* [Tags](assets/l4080/summit-tags.zip)
* [Einfaches Suchanwendungspaket](assets/l4080/simple.ui.apps-0.0.1-snapshot.zip)

### Kapitelpakete {#chapter-packages}

* [Kapitel 1 Lösung](assets/l4080/l4080-chapter1.zip)
* [Kapitel 2 Lösung](assets/l4080/l4080-chapter2.zip)
* [Kapitel 3 Lösung](assets/l4080/l4080-chapter3.zip)
* [Kapitel 4 Lösung](assets/l4080/l4080-chapter4.zip)
* [Einrichten von Kapitel 5](assets/l4080/l4080-chapter5-setup.zip)
* [Kapitel 5 Lösung](assets/l4080/l4080-chapter5-solution.zip)
* [Kapitel 6 Lösung](assets/l4080/l4080-chapter6.zip)
* [Kapitel 9 Lösung](assets/l4080/l4080-chapter9.zip)

## Referenzierte Materialien {#reference-materials}

* [GitHub-Repository](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Sling-Modelle](https://sling.apache.org/documentation/bundles/models.html)
* [Sling Model Exporter](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [QueryBuilder-API](https://experienceleague.adobe.com/docs/?lang=de)
* [AEM Chrome-Plug-in](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode)  ([Dokumentationsseite](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/))

## Korrekturen und Folgemaßnahmen {#corrections-and-follow-up}

Korrekturen und Erläuterungen aus den Labordiskussionen und Antworten auf Folgefragen von Teilnehmern.

1. **Wie kann ich die Neuindizierung stoppen?**

   Die Neuindizierung kann über das MBean IndexStats gestoppt werden, das über [AEM Web-Konsole > JMX](http://localhost:4502/system/console/jmx) verfügbar ist.

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * Führen Sie `abortAndPause()` aus, um die Neuindizierung abzubrechen. Dadurch wird der Index für eine weitere Neuindizierung gesperrt, bis `resume()` aufgerufen wird.
      * Wenn Sie `resume()` ausführen, wird der Indizierungsprozess neu gestartet.
   * Dokumentation: [https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **Wie können Oak-Indizes mehrere Mandanten unterstützen?**

   Oak unterstützt das Platzieren von Indizes über die Inhaltsstruktur, und diese Indizes werden nur innerhalb dieser Unterstruktur indiziert. Beispielsweise könnte **`/content/site-a/oak:index/cqPageLucene`** so erstellt werden, dass Inhalte nur unter **`/content/site-a`.** indiziert werden.

   Eine gleichwertige Methode besteht darin, die Eigenschaften **`includePaths`** und **`queryPaths`** für einen Index unter **`/oak:index`** zu verwenden. Beispiel:

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   Bei diesem Ansatz werden folgende Aspekte berücksichtigt:

   * Abfragen MÜSSEN eine Pfadbeschränkung angeben, die dem Abfragepfadbereich des Index entspricht oder dort untergeordnet ist.
   * Umfassendere Indizes (z. B. `/oak:index/cqPageLucene`) indizieren die Daten ebenfalls, was zu doppelter Erfassung und Kosten für die Festplattennutzung führt.
   * Kann eine doppelte Konfigurationsverwaltung erfordern (z. B. Hinzufügen von gleichen indexRules über mehrere Mandanten-Indizes hinweg, wenn sie denselben Abfragesätzen entsprechen müssen)
   * Dieser Ansatz wird am besten auf der AEM-Veröffentlichungsstufe für die benutzerdefinierte Site-Suche bereitgestellt, da es in der AEM-Autoreninstanz üblich ist, Abfragen oben in der Inhaltsstruktur für verschiedene Mandanten auszuführen (z. B. über OmniSearch) - unterschiedliche Indexdefinitionen können zu unterschiedlichem Verhalten führen, das nur auf der Pfadbeschränkung basiert.


3. **Wo ist eine Liste aller verfügbaren Analyzer?**

   Oak stellt eine Reihe von Lucene-bereitgestellten Analyzer-Konfigurationselementen zur Verwendung in AEM bereit.

   * [Dokumentation zu Apache Oak Analyzers](https://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [Tokenizers](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [Filter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [CharFilters](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **Wie kann in derselben Abfrage nach Seiten und Assets gesucht werden?**

   Neu in AEM 6.3 ist die Möglichkeit, mehrere Knotentypen in derselben bereitgestellten Abfrage abzufragen. Die folgende QueryBuilder-Abfrage. Beachten Sie, dass jede &quot;Sub-Abfrage&quot;in einen eigenen Index aufgelöst werden kann. In diesem Beispiel wird die `cq:Page`-Unterabfrage zu `/oak:index/cqPageLucene` und die `dam:Asset`-Unterabfrage zu `/oak:index/damAssetLucene` aufgelöst.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   Ergebnisse im folgenden Abfrage- und Abfrageplan:

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   Erkunden Sie die Abfrage und Ergebnisse über [QueryBuilder-Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group) und [AEM Chrome-Plug-in](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).

5. **Wie kann in derselben Abfrage über mehrere Pfade gesucht werden?**

   Neu in AEM 6.3 ist die Möglichkeit, mehrere Pfade in derselben bereitgestellten Abfrage abzufragen. Die folgende QueryBuilder-Abfrage. Beachten Sie, dass jede &quot;Sub-Abfrage&quot;in einen eigenen Index aufgelöst werden kann.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   Ergebnisse im folgenden Abfrage- und Abfrageplan

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   Erkunden Sie die Abfrage und Ergebnisse über [QueryBuilder-Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group) und [AEM Chrome-Plug-in](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).

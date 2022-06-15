---
title: Traversale Warnhinweise in AEM as a Cloud Service
description: Erfahren Sie, wie Sie in AEM as a Cloud Service Traversal-Warnungen minimieren können.
topics: Migration
feature: Migration
role: Architect, Developer
level: Beginner
kt: 10427
hidefromtoc: true
hide: true
index: false
thumbnail: kt-10427.jpg
exl-id: 8fcc9364-b84c-4458-82e2-66b47429cd4b
source-git-commit: 48943df64d9793066f8f19497ef42f8aa80e5795
workflow-type: tm+mt
source-wordcount: '838'
ht-degree: 10%

---

# Traffic-Warnhinweise

>[!TIP]
>Markieren Sie diese Seite als Referenz für die Zukunft.

_Was sind traversale Warnungen?_

Traversal-Warnungen __aemerror__ Protokollanweisungen, die auf schlecht funktionierende Abfragen hinweisen, werden im AEM-Veröffentlichungsdienst ausgeführt. Traversale Warnungen treten in der Regel auf zwei Arten in AEM auf:

1. __Langsame Abfragen__ die keine Indizes verwenden, was zu langsamen Reaktionszeiten führt.
1. __Fehlende Abfragen__, die `RuntimeNodeTraversalException`, was zu einem fehlerhaften Erlebnis führt.

Wenn Verfolgungswarnungen deaktiviert werden, wird AEM Leistung verlangsamt und es kann zu fehlerhaften Erlebnissen für Ihre Benutzer kommen.

## So lösen Sie traversale Warnungen

Die Reduzierung von Traversal-Warnungen kann mithilfe von drei einfachen Schritten erreicht werden: analysieren, anpassen und überprüfen. Erwarten Sie mehrere Iterationen der Anpassung und überprüfen Sie, bevor Sie die optimalen Anpassungen ermitteln.

<div class="columns is-multiline">

<!-- Analyze -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Analyze" tabindex="0">
   <div class="x-card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="#analyze" title="Analysieren" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/1-analyze.png" alt="Analysieren">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">Problem analysieren</p>
               <p class="is-size-6">Ermitteln und verstehen Sie, welche Abfragen durchlaufen.</p>
               <a href="#analyze" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Analyse</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Adjust -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adjust" tabindex="0">
   <div class="x-card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="#adjust" title="Anpassen" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/2-adjust.png" alt="Anpassen">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">Anpassen des Codes oder der Konfiguration</p>
               <p class="is-size-6">Aktualisieren Sie Abfragen und Indizes, um Abfragedurchläufe zu vermeiden.</p>
               <a href="#adjust" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Anpassen</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Verify -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Verify" tabindex="0">
   <div class="x-card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="#verify" title="Verify (Überprüfen)" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/3-verify.png" alt="Verify (Überprüfen)">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">Überprüfen der vorgenommenen Anpassungen</p>                       
               <p class="is-size-6">Überprüfen Sie, ob Änderungen an Abfragen und Indizes Traversionen entfernen.</p>
               <a href="#verify" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Überprüfen</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>

## 1. Analyse{#analyze}

Ermitteln Sie zunächst, welche AEM Publish-Dienste seitenübergreifende Warnungen aufweisen. Gehen Sie dazu in Cloud Manager wie folgt vor: [Herunterladen der Veröffentlichungsdienste `aemerror` logs](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/logs.html#cloud-manager){target=&quot;_blank&quot;} aus allen Umgebungen (Entwicklung, Staging und Produktion) für die Vergangenheit __drei Tage__.

![AEM as a Cloud Service Protokolle herunterladen](./assets/traversals/download-logs.jpg)

Öffnen Sie die Protokolldateien und suchen Sie nach der Java™-Klasse. `org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor`. Das Protokoll, das Traversal-Warnungen enthält, enthält eine Reihe von Anweisungen, die in etwa wie folgt aussehen:

```log
24.05.2022 14:18:46.146 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor Traversed 5000 nodes with filter 
Filter(query=select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content') 
/* xpath: /jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]]) 
called by apps.wknd.components.search.example__002e__jsp._jspService; 
consider creating an index or changing the query
```

Abhängig vom Kontext der Ausführung der Abfrage können die Protokollanweisungen hilfreiche Informationen zum Urheber der Abfrage enthalten:

+ HTTP-Anforderungs-URL, die mit der Abfrageausführung verknüpft ist

   + Beispiel: `GET /content/wknd/us/en/example.html HTTP/1.1`

+ Oak-Abfragesyntax

   + Beispiel: `select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content')`

+ XPath-Abfrage

   + Beispiel: `/jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]])`

+ Code, der die Abfrage ausführt

   + Beispiel:  `apps.wknd.components.search.example__002e__jsp._jspService` → `/apps/wknd/components/search/example.html`

__Fehlende Abfragen__ im Anschluss an eine `RuntimeNodeTraversalException` -Anweisung, ähnlich wie:

```log
24.05.2022 14:18:47.240 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.query.FilterIterators The query read or traversed more than 100000 nodes.
org.apache.jackrabbit.oak.query.RuntimeNodeTraversalException: 
    The query read or traversed more than 100000 nodes. To avoid affecting other tasks, processing was stopped.
    ...
```

## 2. Anpassen{#adjust}

Sobald die fehlerhaften Abfragen und ihr aufrufender Code erkannt wurden, müssen Anpassungen vorgenommen werden. Es können zwei Arten von Anpassungen vorgenommen werden, um traversale Warnungen zu vermeiden:

### Abfrage anpassen

__Abfrage ändern__ , um neue Abfrageeinschränkungen hinzuzufügen, die zu bestehenden Indexbeschränkungen aufgelöst werden. Wenn möglich, sollten Sie die Abfrage ändern und stattdessen Indizes ändern.

+ [Erfahren Sie, wie Sie die Abfrageleistung anpassen.](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#query-performance-tuning){target=&quot;_blank&quot;}

### Index anpassen

__Ändern (oder Erstellen) eines AEM Index__ sodass bestehende Abfrageeinschränkungen für die Indexaktualisierungen aufgelöst werden können.

+ [Erfahren Sie, wie Sie vorhandene Indizes anpassen können.](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#query-performance-tuning){target=&quot;_blank&quot;}
+ [Erfahren Sie, wie Sie Indizes erstellen](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#create-a-new-index){target=&quot;_blank&quot;}

## 3. Überprüfen{#verify}

Anpassungen, die entweder an den Abfragen, Indizes oder beidem vorgenommen werden, müssen überprüft werden, um sicherzustellen, dass sie die traversalen Warnungen mindern.

![Abfrage erläutern](./assets/traversals/verify.gif)

Wenn nur [Abfrageanpassung](#adjust-the-query) durchgeführt werden, kann die Abfrage direkt auf AEM as a Cloud Service über die Developer Console getestet werden. [Abfrage erläutern](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html#queries){target=&quot;_blank&quot;}. Erläutern Sie, wie die Abfrage mit dem AEM-Autorendienst ausgeführt wird. Da die Indexdefinitionen jedoch in den Autoren- und Veröffentlichungsdiensten identisch sind, reicht es aus, Abfragen mit dem AEM-Autorendienst zu validieren.

Wenn [Indexanpassungen](#adjust-the-index) festgelegt sind, muss der Index AEM as a Cloud Service bereitgestellt werden. Wenn die Indexanpassungen bereitgestellt sind, wird die [Abfrage erläutern](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html#queries){target=&quot;_blank&quot;} kann verwendet werden, um die Abfrage weiter zu optimieren.

Letztlich werden alle Änderungen (Abfrage und Code) an Git übertragen und mithilfe von Cloud Manager AEM as a Cloud Service bereitgestellt. Nach der Bereitstellung werden die mit den ursprünglichen Traversal-Warnungen verknüpften Codepfade erneut getestet und es wird sichergestellt, dass die Traversal-Warnungen nicht mehr im `aemerror` log.

## Sonstige Ressourcen

Sehen Sie sich diese anderen nützlichen Ressourcen an, um AEM Indizes, Suchvorgänge und Traversalwarnungen zu verstehen.

<div class="columns is-multiline">

<!-- Cloud 5 - Search &amp; Indexing -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Cloud 5 - Search &amp; Indexing" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-5/cloud5-aem-search-and-indexing.html" title="Cloud 5 - Suche und Indizierung" tabindex="-1"><img class="is-bordered-r-small" src="../../../cloud-5/imgs/009-thumb.png" alt="Cloud 5 - Suche und Indizierung"></a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-5/cloud5-aem-search-and-indexing.html" title="Cloud 5 - Suche und Indizierung">Cloud 5 - Suche und Indizierung</a></p>
               <p class="is-size-6">Das Cloud 5-Team zeigt die Ein- und Ausgänge der Suche und Indizierung auf AEM as a Cloud Service.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-5/cloud5-aem-search-and-indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Weitere Informationen</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Content Search and Indexing -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Content Search and Indexing
" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?lang=de" title="Inhaltssuche und -indizierung" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--docs.png" alt="Inhaltssuche und -indizierung">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html" title="Inhaltssuche und -indizierung">Dokumentation zur Inhaltssuche und -indizierung</a></p>
               <p class="is-size-6">Erfahren Sie, wie Sie in AEM as a Cloud Service Indizes erstellen und verwalten.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Weitere Informationen</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Modernizing your Oak indexes -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Modernizing your Oak indexes" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" title="Modernisieren Ihrer Oak-Indizes" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--aem-experts-series.png" alt="Modernisieren Ihrer Oak-Indizes">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" title="Modernisieren Ihrer Oak-Indizes">Modernisieren Ihrer Oak-Indizes</a></p>
               <p class="is-size-6">Erfahren Sie, wie Sie AEM 6 Oak-Indexdefinitionen konvertieren, um as a Cloud Service kompatibel zu sein, und die Indizes in Zukunft verwalten.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Weitere Informationen</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Index definition documentation -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Index definition documentation" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" title="Dokumentation zur Indexdefinition" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--oak-docs.png" alt="Dokumentation zur Indexdefinition">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" title="Dokumentation zur Indexdefinition">Lucene-Indexdokumentation</a></p>
               <p class="has-ellipsis is-size-6">Der Apache Oak Jackrabbit Lucene-Indexverweis, der alle unterstützten Lucene-Indexkonfigurationen dokumentiert.</p>
               <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Weitere Informationen</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>

---
title: Einrichten der intelligenten Übersetzungssuche mit AEM Assets
description: Die intelligente Übersetzungssuche ermöglicht die Verwendung von nicht englischen Suchbegriffen, um zu englischen Inhalten aufzulösen. Um AEM für die intelligente Übersetzungssuche einzurichten, müssen das OSGi-Bundle für die maschinelle Übersetzung von Apache Oak sowie die relevanten kostenlosen und Open-Source-Apache Joshua-Sprachpakete installiert und konfiguriert werden, die die Übersetzungsregeln enthalten.
version: 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
doc-type: Technical Video
exl-id: 7be8c3d5-b944-4421-97b3-bd5766c1b1b5
duration: 634
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '811'
ht-degree: 100%

---

# Einrichten der intelligenten Übersetzungssuche mit AEM Assets{#set-up-smart-translation-search-with-aem-assets}

Die intelligente Übersetzungssuche ermöglicht die Verwendung von nicht englischen Suchbegriffen, um zu englischen Inhalten aufzulösen. Um AEM für die intelligente Übersetzungssuche einzurichten, müssen das OSGi-Bundle für die maschinelle Übersetzung von Apache Oak sowie die relevanten kostenlosen und Open-Source-Apache Joshua-Sprachpakete installiert und konfiguriert werden, die die Übersetzungsregeln enthalten.

>[!VIDEO](https://video.tv.adobe.com/v/21291?quality=12&learn=on)

>[!NOTE]
>
>Die intelligente Übersetzungssuche muss in jeder AEM-Instanz eingerichtet werden, für die sie erforderlich ist.

1. Laden Sie das OSGi-Bundle für die maschinelle Übersetzung von Oak herunter und installieren Sie es
   * [Laden Sie das OSGi-Bundle für die maschinelle Übersetzung von Oak herunter](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22), das der Oak-Version von AEM entspricht.
   * Installieren Sie das heruntergeladene OSGi-Bundle für die maschinelle Übersetzung von Oak in AEM via [`/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. Herunterladen und Aktualisieren der Sprachpakete für Apache Joshua
   * Laden Sie die gewünschten [Apache Joshua-Sprachpakete](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs) herunter und entpacken Sie sie.
   * Bearbeiten Sie die Datei `joshua.config` und kommentieren Sie die zwei Zeilen aus, die mit Folgendem beginnen:

     ```
     feature-function = LanguageModel ...
     ```

   * Ermitteln und notieren Sie die Größe des Modellordners des Sprachpakets, da dies Einfluss darauf hat, wie viel zusätzlichen Heap-Speicher AEM benötigt.
   * Verschieben Sie den entpackten Apache Joshua Sprachpaket-Ordner (mit den `joshua.config`-Bearbeitungen) nach

     ```
     .../crx-quickstart/opt/<source_language-target_language>
     ```

     Zum Beispiel:

     ```
      .../crx-quickstart/opt/es-en
     ```

3. Neustarten von AEM mit aktualisierter Heap-Speicherzuordnung
   * AEM anhalten
   * Bestimmen der neuen erforderlichen Heap-Größe für AEM

      * Die Heap-Größe von AEM vor dem Sprachmangel + die Größe des Modellverzeichnisses, aufgerundet auf die nächsten 2 GB
      * Ein Beispiel: Wenn die AEM-Installation vor den Sprachpaketen 8 GB Heap zum Ausführen benötigt und der Modellordner des Sprachpakets 3,8 GB unkomprimiert ist, beträgt die neue Heap-Größe:

        Das ursprüngliche `8GB` + (`3.75GB` aufgerundet auf das nächste `2GB`, also `4GB`) ergibt insgesamt `12GB`

   * Vergewissern Sie sich, dass der Computer über diese Menge an zusätzlichem Speicher verfügt.
   * Aktualisieren Sie AEM-Startskripte, um die neue Heap-Größe anzupassen

      * Z.B. `java -Xmx12g -jar cq-author-p4502.jar`

   * Starten Sie AEM mit der erhöhten Heap-Größe neu

   >[!NOTE]
   >
   >Der benötigte Heap-Speicher für Sprachpakete kann groß werden, insbesondere wenn mehrere Sprachpakete verwendet werden.
   >
   >
   >Stellen Sie immer sicher, dass **die Instanz über genügend Speicher** verfügt, um die Vergrößerung des zugewiesenen Heap-Speicherplatzes zu bewältigen.
   >
   >
   >Der **Basis-Heap muss immer so berechnet werden, dass eine akzeptable Leistung ohne installierte Sprachpakete** möglich ist.

4. Registrierung der Sprachpakete über die OSGi-Konfigurationen von Apache Jackrabbit Oak Machine Translation Full-text Query Terms Provider

   * Erstellen Sie für jedes Sprachpaket [eine neue OSGi-Konfiguration von Apache Jackrabbit Oak Machine Translation Full-text Query Terms Provider](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) über den Konfigurations-Manager der AEM-Web-Konsole.

      * `Joshua Config Path` ist der absolute Pfad zur Datei „joshua.config“. Der AEM-Prozess muss alle Dateien im Ordner des Sprachpakets lesen können.
      * `Node types` sind die möglichen Knotentypen, deren Volltextsuche dieses Sprachpaket zur Übersetzung heranzieht.
      * `Minimum score` ist der Mindestkonfidenzwert für einen übersetzten Begriff, damit dieser verwendet werden kann.

         * Zum Beispiel kann hombre (spanisch für „Mann“) mit einem Konfidenzwert von `0.9` in das englische Wort „man“ und mit einem Konfidenzwert von `0.2` in das englische Wort „human“ übersetzt werden. Wenn der Mindestwert auf `0.3` eingestellt wird, wird die Übersetzung „hombre“ in „man“ beibehalten, aber die Übersetzung „hombre“ in „human“ verworfen, da diese Übersetzungsbewertung von `0.2` unter dem Mindestwert von `0.3` liegt.

5. Durchführen einer Volltextsuche nach Assets
   * Da „dam:Asset“ der Knotentyp ist, für den dieses Sprachpaket wieder registriert wurde, müssen wir mit der Volltextsuche nach AEM Assets suchen, um dies zu überprüfen.
   * Navigieren Sie zu AEM > Assets und öffnen Sie Omnisearch. Suchen Sie nach einem Begriff in der Sprache, deren Sprachpaket installiert wurde.
   * Passen Sie bei Bedarf den Mindestwert in den OSGi-Konfigurationen an, um die Genauigkeit der Ergebnisse sicherzustellen.

6. Aktualisieren von Sprachpaketen
   * Apache Joshua-Sprachpakete werden vom Apache Joshua-Projekt in vollem Umfang gepflegt und ihre Aktualisierung oder Korrektur liegt im Ermessen des Apache Joshua-Projekts.
   * Wenn ein Sprachpaket aktualisiert wird, müssen zur Installation der Updates in AEM die oben genannten Schritte 2 bis 4 ausgeführt werden, wobei die Heap-Größe je nach Bedarf erhöht oder verringert werden muss.

      * Beachten Sie, dass Sie beim Verschieben des entpackten Sprachpakets in den Ordner „crx-quickstart/opt“ alle vorhandenen Sprachpaketordner verschieben müssen, bevor Sie das neue Sprachpaket hineinkopieren.

   * Wenn AEM keinen Neustart erfordert, müssen die relevanten OSGi-Konfigurationen von Apache Jackrabbit Oak Machine Translation Fulltext Query Terms Provider, die sich auf die aktualisierten Sprachpakete beziehen, neu gespeichert werden, damit AEM die aktualisierten Dateien verarbeitet.

## Aktualisieren des Index „damAssetLucene“ {#updating-damassetlucene-index}

Damit [Smart-Tags für AEM](https://helpx.adobe.com/de/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) von intelligenter Übersetzung von AEM beeinflusst werden können, muss der `/oak   :index  /damAssetLucene`-Index von AEM aktualisiert werden, um die predictedTags (der Systemname für „Smart-Tags“) als Teil des aggregierten Lucene-Index des Assets zu markieren.

Stellen Sie unter `/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags` sicher, dass die Konfiguration wie folgt lautet:

```xml
 <damAssetLucene jcr:primaryType="oak:QueryIndexDefinition">
        <indexRules jcr:primaryType="nt:unstructured">
            <dam:Asset jcr:primaryType="nt:unstructured">
                <properties jcr:primaryType="nt:unstructured">
                    ...
                    <predictedTags
                        jcr:primaryType="nt:unstructured"
                        isRegexp="{Boolean}true"
                        name="jcr:content/metadata/predictedTags/*/name"
                        useInSpellheck="{Boolean}true"
                        useInSuggest="{Boolean}true"
                        analyzed="{Boolean}true"
                        nodeScopeIndex="{Boolean}true"/>
```

## Zusätzliche Ressourcen{#additional-resources}

* [OSGi-Bundle für die maschinelle Übersetzung von Apache Oak](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Apache Joshua-Sprachpakete](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [Smart-Tags für AEM](https://helpx.adobe.com/de/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [Best Practices für Abfragen und Indizierung](https://helpx.adobe.com/de/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)

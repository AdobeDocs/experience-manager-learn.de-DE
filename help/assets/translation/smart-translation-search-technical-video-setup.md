---
title: Einrichten der intelligenten Übersetzungssuche mit AEM Assets
description: Die intelligente Übersetzungssuche ermöglicht die Verwendung von nicht englischen Suchbegriffen, um zu englischen Inhalten aufzulösen. Um AEM für die intelligente Übersetzungssuche einzurichten, müssen das OSGi-Bundle für die maschinelle Übersetzung von Apache Oak sowie die relevanten kostenlosen und Open-Source-Apache Joshua-Sprachpakete installiert und konfiguriert werden, die die Übersetzungsregeln enthalten.
version: 6.3, 6.4, 6.5
feature: Suchen
topic: Content Management
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '870'
ht-degree: 1%

---


# Einrichten der intelligenten Übersetzungssuche mit AEM Assets{#set-up-smart-translation-search-with-aem-assets}

Die intelligente Übersetzungssuche ermöglicht die Verwendung von nicht englischen Suchbegriffen, um zu englischen Inhalten aufzulösen. Um AEM für die intelligente Übersetzungssuche einzurichten, müssen das OSGi-Bundle für die maschinelle Übersetzung von Apache Oak sowie die relevanten kostenlosen und Open-Source-Apache Joshua-Sprachpakete installiert und konfiguriert werden, die die Übersetzungsregeln enthalten.

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>Die intelligente Übersetzungssuche muss in jeder AEM Instanz eingerichtet werden, die sie benötigt.

1. Laden Sie das OSGi-Bundle Oak Search Machine Translation herunter und installieren Sie es.
   * [Laden Sie das OSGi-](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) Bundle für die maschinelle Übersetzung von Oak herunter, das AEM Oak-Version entspricht.
   * Installieren Sie das heruntergeladene OSGi-Bundle Oak Search Machine Translation über [ `/system/console/bundles`](http://localhost:4502/system/console/bundles) in AEM.

2. Herunterladen und Aktualisieren der Sprachpakete für Apache Joshua
   * Laden Sie die gewünschten [Apache Joshua Sprachpakete](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs) herunter und entpacken Sie sie.
   * Bearbeiten Sie die Datei `joshua.config` und kommentieren Sie die zwei Zeilen aus, die mit beginnen:

      ```
      feature-function = LanguageModel ...
      ```

   * Bestimmen und zeichnen Sie die Größe des Modellordners des Sprachpakets auf, da dies beeinflusst, wie viel zusätzlicher Heap-Speicherplatz AEM benötigt wird.
   * Verschieben Sie den entpackten Apache Joshua-Sprachpaketordner (mit den Änderungen `joshua.config`) nach

      ```
      .../crx-quickstart/opt/<source_language-target_language>
      ```

      Beispiel:

      ```
       .../crx-quickstart/opt/es-en
      ```

3. AEM mit aktualisierter Heap-Speicherzuordnung neu starten
   * Stoppen Sie AEM.
   * Bestimmen der neuen erforderlichen Heap-Größe für AEM

      * AEM Heap-Größe vor Sprachmangel + die Größe des Modellverzeichnisses, aufgerundet auf 2 GB
      * Beispiel: Wenn vorsprachige Packs für die AEM-Installation 8 GB Heap benötigen, um ausgeführt zu werden, und der Modellordner des Sprachpakets 3,8 GB unkomprimiert ist, lautet die neue Heap-Größe:

         Das Original `8GB` + ( `3.75GB`, auf das nächste `2GB` aufgerundet, was `4GB` ist) für insgesamt `12GB`
   * Stellen Sie sicher, dass der Computer über diese Menge zusätzlichen Speicherplatzes verfügt.
   * Aktualisieren AEM Startskripte, um die neue Heap-Größe anzupassen

      * Ex. `java -Xmx12g -jar cq-author-p4502.jar`
   * Starten Sie AEM mit der erhöhten Heap-Größe neu.

   >[!NOTE]
   >
   >Der benötigte Heap-Speicher für Sprachpakete kann groß werden, insbesondere wenn mehrere Sprachpakete verwendet werden.
   >
   >
   >Stellen Sie immer sicher, dass **die Instanz über genügend Arbeitsspeicher** verfügt, um die Zunahme des zugewiesenen Heap-Speicherplatzes zu berücksichtigen.
   >
   >
   >Der **Basis-Heap muss immer so berechnet werden, dass eine akzeptable Leistung ohne installierte Sprachpakete** gewährleistet ist.

4. Registrieren Sie die Sprachpakete über die OSGi-Konfigurationen des Apache Jackrabbit Oak Machine Translation (Volltext Query Term Provider).

   * Erstellen Sie für jedes Sprachpaket [eine neue Apache Jackrabbit Oak Machine Translation-Volltext-Abfragebedingungen-Provider-OSGi-Konfiguration](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) über den Configuration Manager der AEM Web Console.

      * `Joshua Config Path` ist der absolute Pfad zur Datei &quot;joshua.config&quot;. Der AEM muss alle Dateien im Ordner des Sprachpakets lesen können.
      * `Node types` sind die Knotentypen für Kandidaten, deren Volltextsuche dieses Sprachpaket für die Übersetzung freigibt.
      * `Minimum score` ist der minimale Konfidenzwert für einen übersetzten Begriff, der verwendet werden soll.

         * Beispiel: hombre (Spanisch für &quot;man&quot;) kann in das englische Wort &quot;man&quot;mit einem Konfidenzwert von `0.9` übersetzen und auch in das englische Wort &quot;human&quot;mit einem Konfidenzwert `0.2` übersetzen. Wenn Sie die Mindestpunktzahl auf `0.3` einstellen, bleibt die Übersetzung &quot;hombre&quot;zu &quot;man&quot; erhalten, aber die Übersetzung &quot;hombre&quot;zu &quot;human&quot;wird verworfen, da dieser Übersetzungswert von `0.2` kleiner als der Mindestwert von `0.3` ist.

5. Durchführen einer Volltextsuche nach Assets
   * Da dam:Asset der Knotentyp ist, dass dieses Sprachpaket erneut registriert wird, müssen wir zur Validierung dieses Problems mithilfe der Volltextsuche nach AEM Assets suchen.
   * Navigieren Sie zu AEM > Assets und öffnen Sie Omnisearch. Suchen Sie nach einem Begriff in der Sprache, deren Sprachpaket installiert wurde.
   * Passen Sie bei Bedarf die Mindestbewertung in den OSGi-Konfigurationen an, um die Genauigkeit der Ergebnisse sicherzustellen.

6. Aktualisieren von Sprachpaketen
   * Apache Joshua Sprachpakete werden vom Apache Joshua-Projekt in vollem Umfang gepflegt und ihre Aktualisierung oder Korrektur liegt im Ermessen des Apache Joshua-Projekts.
   * Wenn ein Sprachpaket aktualisiert wird, müssen zur Installation der Updates in AEM die obigen Schritte 2 bis 4 befolgt werden, wobei die Heap-Größe nach Bedarf nach oben oder unten angepasst werden muss.

      * Beachten Sie, dass Sie beim Verschieben des entpackten Sprachpakets in den Ordner crx-quickstart/opt einen vorhandenen Sprachpack-Ordner verschieben, bevor Sie ihn in den neuen kopieren.
   * Wenn AEM keinen Neustart erfordert, müssen die relevanten OSGi-Konfigurationen des Apache Jackrabbit Oak Machine Translation Fulltext Query Term Provider, die sich auf die aktualisierten Sprachpakete beziehen, erneut gespeichert werden, damit AEM die aktualisierten Dateien verarbeitet.


## Aktualisieren des damAssetLucene-Index {#updating-damassetlucene-index}

Damit [AEM Smart-Tags](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) von AEM Smart-Übersetzung betroffen sein können, muss der Index `/oak   :index  /damAssetLucene` aktualisiert werden, um die predictedTags (den Systemnamen für &quot;Smart-Tags&quot;) als Teil des Asset-Aggregat-Lucene-Index zu kennzeichnen.

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

* [Apache Oak Search Machine Translation OSGi-Bundle](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Apache Joshua Language Packs](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [AEM Smart-Tags](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [Best Practices für Abfrage und Indizierung](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)
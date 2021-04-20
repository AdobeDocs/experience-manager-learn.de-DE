---
title: Intelligente Übersetzungssuche mit AEM Assets einrichten
description: Die intelligente Übersetzungssuche ermöglicht die Verwendung von Suchbegriffen, die nicht Englisch sind, um auf englische Inhalte zu gelangen. Um AEM für die Suche nach intelligenten Übersetzungen einzurichten, muss das Apache Oak Search Machine Translation OSGi Bundle installiert und konfiguriert werden sowie die entsprechenden kostenlosen und Open Source Apache Joshua Sprachpakete, die die Übersetzungsregeln enthalten.
version: 6.3, 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '872'
ht-degree: 1%

---


# Intelligente Übersetzungssuche mit AEM Assets einrichten{#set-up-smart-translation-search-with-aem-assets}

Die intelligente Übersetzungssuche ermöglicht die Verwendung von Suchbegriffen, die nicht Englisch sind, um auf englische Inhalte zu gelangen. Um AEM für die Suche nach intelligenten Übersetzungen einzurichten, muss das Apache Oak Search Machine Translation OSGi Bundle installiert und konfiguriert werden sowie die entsprechenden kostenlosen und Open Source Apache Joshua Sprachpakete, die die Übersetzungsregeln enthalten.

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>Die intelligente Übersetzungssuche muss für jede AEM Instanz eingerichtet werden, für die sie erforderlich ist.

1. Laden Sie das OSGi-Bundle Oak Search Machine Translation herunter und installieren Sie es.
   * [Laden Sie das Oak Search Machine Translation OSGi ](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) Bundle herunter, das der AEM Oak Version entspricht.
   * Installieren Sie das heruntergeladene OSGi-Bundle der Oak Search Machine Translation über [ `/system/console/bundles`](http://localhost:4502/system/console/bundles) in AEM.

2. Apache Joshua Sprachpakete herunterladen und aktualisieren
   * Laden Sie die gewünschten [Apache Joshua Sprachpakete](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs) herunter und entpacken Sie sie.
   * Bearbeiten Sie die Datei `joshua.config` und kommentieren Sie die zwei Zeilen aus, die mit beginnen:

      ```
      feature-function = LanguageModel ...
      ```

   * Legen Sie die Größe des Modellordners des Sprachpakets fest, und zeichnen Sie diese auf, da dadurch der zusätzliche Speicherplatz AEM benötigt wird.
   * Verschieben Sie den entpackten Apache Joshua Sprachpaket-Ordner (mit den Bearbeitungen `joshua.config`) nach

      ```
      .../crx-quickstart/opt/<source_language-target_language>
      ```

      Beispiel:

      ```
       .../crx-quickstart/opt/es-en
      ```

3. AEM mit aktualisierter Heap-Speicherzuordnung neu starten
   * Stoppen Sie AEM.
   * Bestimmen Sie die neue erforderliche Heap-Größe für AEM

      * AEM voreingestellte Heap-Größe + Größe des Modellverzeichnisses auf 2 GB aufgerundet
      * Beispiel: Wenn für Sprachpakete die AEM Installation 8 GB Heap benötigt und der Modellordner des Sprachpakets 3,8 GB unkomprimiert ist, lautet die neue Heap-Größe wie folgt:

         Das Original `8GB` + ( `3.75GB`, auf das nächste `2GB` aufgerundet, was `4GB` ist) für insgesamt `12GB`
   * Stellen Sie sicher, dass auf dem Computer diese Menge zusätzlicher Speicher verfügbar ist.
   * Aktualisieren AEM Beginn-up-Skripten zur Anpassung an die neue Heap-Größe

      * Ex. `java -Xmx12g -jar cq-author-p4502.jar`
   * Starten Sie AEM mit der erhöhten Heap-Größe neu.

   >[!NOTE]
   >
   >Der erforderliche Heap-Speicherplatz für Sprachpakete kann erheblich vergrößert werden, insbesondere wenn mehrere Sprachpakete verwendet werden.
   >
   >
   >Vergewissern Sie sich immer, dass **die Instanz über genügend Arbeitsspeicher** verfügt, um die Erhöhungen des zugewiesenen Heap-Speichers zu berücksichtigen.
   >
   >
   >Die **Base-Heap muss immer so berechnet werden, dass eine akzeptable Leistung ohne installierte Sprachpakete** unterstützt wird.

4. Registrieren Sie die Sprachpakete über Apache Jackrabbit Oak Machine Translation Full-text-Abfrage-Provider OSGi-Konfigurationen

   * Erstellen Sie für jedes Sprachpaket [über den Configuration Manager der AEM Web Console eine neue Apache Jackrabbit Oak Machine Translation Full-Text-Abfrage-Provider-OSGi-Konfiguration](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory).

      * `Joshua Config Path` ist der absolute Pfad zur Datei &quot;joshua.config&quot;. Der AEM muss alle Dateien im Ordner des Sprachpakets lesen können.
      * `Node types` sind die Knotentypen, deren Volltextsuche dieses Sprachpaket zur Übersetzung einbindet.
      * `Minimum score` ist der Mindestwert für die Konfidenz eines übersetzten Begriffs, der verwendet werden soll.

         * Zum Beispiel kann hombre (Spanisch für &quot;man&quot;) in das englische Wort &quot;man&quot; mit einem Konfidenzwert von `0.9` übersetzen und auch in das englische Wort &quot;human&quot; mit einem Konfidenzwert `0.2` übersetzen. Wenn Sie das Mindestergebnis auf `0.3` einstellen, bleibt die Übersetzung &quot;hombre&quot;auf &quot;man&quot;eingestellt, aber die Übersetzung &quot;hombre&quot;auf &quot;human&quot;wird verworfen, da dieser Transformation-Wert von `0.2` kleiner als der Mindestwert von `0.3` ist.

5. Durchführen einer Volltextsuche nach Assets
   * Da dam:Asset der Knotentyp ist, den dieses Sprachpaket erneut registriert ist, müssen wir nach AEM Assets suchen, um dies zu validieren.
   * Navigieren Sie zu AEM > Assets und öffnen Sie OmnitureSearch. Suchen Sie nach einem Begriff in der Sprache, deren Sprachpaket installiert wurde.
   * Passen Sie bei Bedarf die Mindestpunktzahl in den OSGi-Konfigurationen an, um die Genauigkeit der Ergebnisse sicherzustellen.

6. Aktualisieren von Sprachpaketen
   * Apache Joshua Sprachpakete werden vom Apache Joshua Projekt in vollem Umfang gepflegt, und ihre Aktualisierung oder Korrektur liegt im Ermessen des Apache Joshua-Projekts.
   * Wenn ein Sprachpaket aktualisiert wird, müssen zur Installation der Updates in AEM die oben genannten Schritte 2 bis 4 befolgt werden, wobei die Heap-Größe nach Bedarf nach oben oder unten angepasst werden muss.

      * Beachten Sie, dass beim Verschieben des entpackten Sprachpakets in den Ordner crx-quickstart/opt alle vorhandenen Sprachpakete verschoben werden müssen, bevor Sie sie in den neuen Ordner kopieren.
   * Wenn AEM keinen Neustart erfordert, müssen die entsprechenden OSGi-Konfigurationen für die Apache Jackrabbit Oak Abfrage Translation Fulltext-, die sich auf das aktualisierte Sprachpaket/die aktualisierten Sprachpakete beziehen, erneut gespeichert werden, damit AEM die aktualisierten Dateien verarbeitet werden.


## Aktualisieren von damAssetLucene Index {#updating-damassetlucene-index}

Damit [AEM Smart Tags](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) von AEM Smart Translation betroffen sein können, muss AEM `/oak   :index  /damAssetLucene`-Index aktualisiert werden, um die prognostizierten Tags (den Systemnamen für &quot;Smart Tags&quot;) als Teil des Asset-Aggregat-Lucene-Index zu markieren.

Vergewissern Sie sich unter `/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags`, dass die Konfiguration wie folgt lautet:

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

* [Apache Oak Search Machine Translation OSGi Bundle](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Apache Joshua-Sprachpakete](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [AEM Smart-Tags](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [Best Practices für das Abfragen und Indizieren](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)
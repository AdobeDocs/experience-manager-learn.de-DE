---
title: Indizes mithilfe von oak-run.jar verwalten
description: Der index-Befehl von oak-run.jar fasst eine Reihe von Funktionen zusammen, um Oak-Indizes in AEM zu verwalten, von der Erfassung von Indexstatistiken über die Durchführung von Indexkonsistenzprüfungen bis zum erneuten/Indizieren von Indizes selbst.
version: 6.4, 6.5
feature: Eiche
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 3%

---


# Indizes mithilfe von oak-run.jar verwalten

[!DNL oak-run.jar]Der Befehl index fasst eine Reihe von Funktionen zusammen, um  [!DNL Oak]200 Indizes in AEM zu verwalten, von der Erfassung von Indexstatistiken über die Durchführung von Indexkonsistenzprüfungen bis hin zum erneuten/Indizieren von Indizes selbst.

>[!NOTE]
>
>In diesem Artikel und Videos werden die Begriffe Indizierung und Neuindizierung synonym verwendet und als dieselbe Operation betrachtet.

## [!DNL oak-run.jar] Grundlagen zu Indexbefehlen

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* Die verwendete Version von [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) muss mit der Version von Oak übereinstimmen, die auf der AEM Instanz verwendet wird.
* Bei der Verwaltung von Indizes mit [!DNL oak-run.jar] wird der Befehl **[!DNL index]** mit verschiedenen Flags verwendet, um verschiedene Vorgänge zu unterstützen.

   * `java -jar oak-run*.jar index ...`

## Indexstatistiken

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` sichert alle Indexdefinitionen, wichtige Indexstatistiken und Indexinhalte für Offline-Analysen. 
* Die Indexstatistikerfassung ist sicher für die Ausführung in AEM Instanzen.

## Index-Konsistenzprüfung

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` bestimmt schnell, ob lucene Oak-Indizes beschädigt sind.
* Die Konsistenzprüfung kann auf AEM Instanz ausgeführt werden, auf der Konsistenzprüfstufen 1 und 2 angewendet werden.

## TarMK Online-Indizierung mit [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* Die Online-Indizierung von [!DNL TarMK] mit [!DNL oak-run.jar] ist schneller als die Einstellung von `reindex=true` auf dem `oak:queryIndexDefinition`-Knoten. Trotz dieser Leistungssteigerung erfordert die Online-Indizierung mit [!DNL oak-run.jar] noch ein Wartungsfenster, um die Indizierung durchzuführen.

* Die Online-Indizierung von [!DNL TarMK] mit [!DNL oak-run.jar] sollte **nicht** für AEM Instanzen außerhalb des AEM Instanzwartungsfensters ausgeführt werden.

## TarMK Offline-Indizierung mit oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* Die Offline-Indizierung von [!DNL TarMK] mit [!DNL oak-run.jar] ist der einfachste [!DNL oak-run.jar]-basierte Indexierungsansatz für [!DNL TarMK], da hierfür ein einzelner [!DNL oak-run.jar]-Befehl erforderlich ist. Die AEM Instanz muss jedoch beendet werden.

## TarMK Out-of-Band-Indizierung mit oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* Die Out-of-Band-Indizierung bei [!DNL TarMK] mit [!DNL oak-run.jar] minimiert die Auswirkungen der Indexierung bei AEM-Instanzen in Gebrauch.
* Die Out-of-Band-Indizierung ist der empfohlene Indexierungsansatz für AEM Installationen, bei denen die Zeit zum Neuindizieren/Indexieren die verfügbaren Wartungsfenster überschreitet.

## MongoMK Online-Indizierung mit oak-run.jar

* Der Online-Index mit [!DNL oak-run.jar] auf [!DNL MongoMK] und [!DNL RDBMK] ist die empfohlene Methode für die Neuindizierung von [!DNL MongoMK]- (und [!DNL RDBMK]) AEM Installationen. **Es sollte keine andere Methode für  [!DNL MongoMK] oder  [!DNL RDBMK]verwendet werden.**
* Diese Indexierung muss nur für eine einzelne AEM Instanz im Cluster ausgeführt werden.
* Die Online-Indizierung von [!DNL MongoMK] ist sicher, um mit einem laufenden AEM Cluster ausgeführt zu werden, da der Repository-Traversal nur auf einem einzelnen [!DNL MongoDB]-Knoten ausgeführt wird, sodass die anderen weiterhin Anforderungen bereitstellen können, ohne dass dies erhebliche Auswirkungen auf die Leistung hat.

Der Befehl [!DNL oak-run.jar] index, um eine Online-Indizierung von [!DNL MongoMK] durchzuführen, ist der [gleiche wie die  [!DNL TarMK] Online-Indizierung mit [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) mit der Differenz, dass der Segmentspeicherparameter auf die [!DNL MongoDB]-Instanz verweist, die den Node Store enthält.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## Begleitmaterialien

* [Download [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *Vergewissern Sie sich, dass die heruntergeladene Version mit der auf AEM installierten Version von Oak übereinstimmt, wie oben beschrieben*
* [Apache Jackrabbit Oak oak-run.jar Index Befehlsdokumentation](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)

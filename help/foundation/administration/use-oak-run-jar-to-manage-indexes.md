---
title: Verwenden Sie oak-run.jar zum Verwalten von Indizes
description: Der Indexbefehl von oak-run.jar konsolidiert eine Reihe von Funktionen zur Verwaltung von Oak-Indizes in AEM, von der Erfassung von Indexstatistiken, der Durchführung von Indexkonsistenzprüfungen und der Neuindizierung/Indizierung von Indizes selbst.
version: 6.4, 6.5
feature: Suchen
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
topic: Leistung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 3%

---


# Verwenden Sie oak-run.jar zum Verwalten von Indizes

[!DNL oak-run.jar]Der Indexbefehl von umfasst eine Reihe von Funktionen, mit denen  [!DNL Oak]200 Indizes in AEM verwaltet werden können, von der Erfassung von Indexstatistiken über die Durchführung von Indexkonsistenzprüfungen bis hin zur Neuindizierung/Indizierung von Indizes selbst.

>[!NOTE]
>
>In diesem Artikel und in Videos werden die Begriffe Indizierung und Neuindizierung synonym verwendet und als derselbe Vorgang betrachtet.

## [!DNL oak-run.jar] Grundlagen zum Index-Befehl

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* Die verwendete Version von [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) muss mit der Version von Oak übereinstimmen, die in der AEM-Instanz verwendet wird.
* Die Verwaltung von Indizes mit [!DNL oak-run.jar] nutzt den Befehl **[!DNL index]** mit verschiedenen Flags, um verschiedene Vorgänge zu unterstützen.

   * `java -jar oak-run*.jar index ...`

## Indexstatistiken

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` sichert alle Indexdefinitionen, wichtige Indexstatistiken und Indexinhalte für Offline-Analysen. 
* Die Indexstatistikerfassung ist sicher für die Ausführung in AEM Instanzen.

## Indexkonsistenzprüfung

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` schnell feststellen, ob Lucene Oak-Indizes beschädigt sind.
* Die Konsistenzprüfung ist sicher, in AEM Instanz ausgeführt zu werden, um Konsistenzprüfungen auf den Ebenen 1 und 2 durchzuführen.

## TarMK Online-Indizierung mit [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* Die Online-Indizierung von [!DNL TarMK] mit [!DNL oak-run.jar] ist schneller als die Einstellung von `reindex=true` auf dem Knoten `oak:queryIndexDefinition` . Trotz dieser Leistungssteigerung erfordert die Online-Indizierung mit [!DNL oak-run.jar] weiterhin ein Wartungsfenster, um die Indizierung durchzuführen.

* Die Online-Indizierung von [!DNL TarMK] mit [!DNL oak-run.jar] sollte **not** für AEM Instanzen außerhalb des Wartungsfensters AEM Instanzen ausgeführt werden.

## TarMK Offline-Indizierung mit oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* Die Offline-Indizierung von [!DNL TarMK] mit [!DNL oak-run.jar] ist der einfachste [!DNL oak-run.jar]-basierte Indizierungsansatz für [!DNL TarMK], da ein einzelner [!DNL oak-run.jar]-Befehl erforderlich ist. Es ist jedoch erforderlich, dass die AEM-Instanz heruntergefahren wird.

## TarMK Out-of-Band-Indizierung mit oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* Die Out-of-Band-Indizierung auf [!DNL TarMK] mit [!DNL oak-run.jar] minimiert die Auswirkungen der Indizierung auf AEM-Instanzen im Betrieb.
* Die Out-of-Band-Indizierung ist der empfohlene Indizierungsansatz für AEM Installationen, bei denen die Zeit für die Neuindizierung/Indizierung die verfügbaren Wartungsfenster überschreitet.

## MongoMK Online-Indizierung mit oak-run.jar

* Der Online-Index mit [!DNL oak-run.jar] unter [!DNL MongoMK] und [!DNL RDBMK] ist die empfohlene Methode für die Neuindizierung von [!DNL MongoMK] (und [!DNL RDBMK]) AEM Installationen. **Für  [!DNL MongoMK] oder  [!DNL RDBMK]sollte keine andere Methode angewendet werden.**
* Diese Indizierung muss nur für eine einzelne AEM-Instanz im Cluster ausgeführt werden.
* Die Online-Indizierung von [!DNL MongoMK] kann für einen laufenden AEM-Cluster sicher ausgeführt werden, da der Repository-Traversal nur auf einem einzelnen [!DNL MongoDB]-Knoten ausgeführt wird, sodass andere weiterhin Anfragen bearbeiten können, ohne dass sich dies auf die Leistung auswirkt.

Der Indexbefehl [!DNL oak-run.jar] zum Ausführen einer Online-Indizierung von [!DNL MongoMK] ist der [gleiche wie die  [!DNL TarMK] Online-Indizierung mit [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) mit dem Unterschied, dass der Segmentspeicherparameter auf die [!DNL MongoDB]-Instanz verweist, die den Knotenspeicher enthält.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## Unterstützende Materialien

* [Download [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *Stellen Sie sicher, dass die heruntergeladene Version mit der auf AEM installierten Version von Oak übereinstimmt, wie oben beschrieben.*
* [Apache Jackrabbit Oak oak-run.jar Index Befehlsdokumentation](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)

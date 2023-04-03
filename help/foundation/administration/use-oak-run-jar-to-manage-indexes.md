---
title: Verwenden Sie oak-run.jar zum Verwalten von Indizes
description: Der Indexbefehl von oak-run.jar konsolidiert eine Reihe von Funktionen zur Verwaltung von Oak-Indizes in AEM, von der Erfassung von Indexstatistiken, der Durchführung von Indexkonsistenzprüfungen und der Neuindizierung/Indizierung von Indizes selbst.
version: 6.4, 6.5
feature: Search
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 4%

---

# Verwenden Sie oak-run.jar zum Verwalten von Indizes

[!DNL oak-run.jar]Der Index-Befehl von enthält eine Reihe von zu verwaltenden Funktionen [!DNL Oak]200 Indizes in AEM, von der Erfassung von Indexstatistiken, der Durchführung von Indexkonsistenzprüfungen und der Neuindizierung von Indizes selbst.

>[!NOTE]
>
>In diesem Artikel und in Videos werden die Begriffe Indizierung und Neuindizierung synonym verwendet und als derselbe Vorgang betrachtet.

## [!DNL oak-run.jar] Grundlagen zum Index-Befehl

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* Die Version von [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) verwendet muss mit der Version von Oak übereinstimmen, die in der AEM-Instanz verwendet wird.
* Verwalten von Indizes mithilfe von [!DNL oak-run.jar] nutzt die **[!DNL index]** -Befehl mit verschiedenen Flags, um verschiedene Vorgänge zu unterstützen.

   * `java -jar oak-run*.jar index ...`

## Indexstatistiken

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` sichert alle Indexdefinitionen, wichtige Indexstatistiken und Indexinhalte für Offline-Analysen. 
* Die Indexstatistikerfassung ist sicher für die Ausführung in AEM Instanzen.

## Indexkonsistenzprüfung

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` schnell feststellen, ob Lucene Oak-Indizes beschädigt sind.
* Die Konsistenzprüfung ist sicher, in AEM Instanz ausgeführt zu werden, um Konsistenzprüfungen auf den Ebenen 1 und 2 durchzuführen.

## TarMK Online-Indizierung mit [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* Online-Indizierung von [!DNL TarMK] using [!DNL oak-run.jar] ist schneller als die Einstellung `reindex=true` auf `oak:queryIndexDefinition` Knoten. Trotz dieser Leistungssteigerung wird die Online-Indizierung mithilfe von [!DNL oak-run.jar] erfordert weiterhin ein Wartungsfenster, um die Indizierung durchzuführen.

* Online-Indizierung von [!DNL TarMK] using [!DNL oak-run.jar] sollte **not** werden für AEM Instanzen außerhalb des Wartungsfensters der AEM Instanzen ausgeführt.

## TarMK Offline-Indizierung mit oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* Offline-Indizierung von [!DNL TarMK] using [!DNL oak-run.jar] ist der einfachste [!DNL oak-run.jar] basierter Indizierungsansatz für [!DNL TarMK] da eine [!DNL oak-run.jar] , es ist jedoch erforderlich, dass die AEM-Instanz heruntergefahren wird.

## TarMK Out-of-Band-Indizierung mit oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* Out-of-Band-Indizierung auf [!DNL TarMK] using [!DNL oak-run.jar] minimiert die Auswirkungen der Indizierung auf AEM-Instanzen im Betrieb.
* Die Out-of-Band-Indizierung ist der empfohlene Indizierungsansatz für AEM Installationen, bei denen die Zeit für die Neuindizierung/Indizierung die verfügbaren Wartungsfenster überschreitet.

## MongoMK Online-Indizierung mit oak-run.jar

* Online-Index mit [!DNL oak-run.jar] on [!DNL MongoMK] und [!DNL RDBMK] ist die empfohlene Methode für die Neuindizierung [!DNL MongoMK] und [!DNL RDBMK]) AEM Installationen. **Es sollte keine andere Methode angewendet werden für [!DNL MongoMK] oder [!DNL RDBMK].**
* Diese Indizierung muss nur für eine einzelne AEM-Instanz im Cluster ausgeführt werden.
* Online-Indizierung von [!DNL MongoMK] ist sicher, für einen laufenden AEM-Cluster ausgeführt zu werden, da der Repository-Durchlauf nur auf einer einzelnen [!DNL MongoDB] -Knoten verwenden, sodass andere weiterhin Anfragen bearbeiten können, ohne dass sich dies auf die Leistung auswirkt.

Die [!DNL oak-run.jar] Indexbefehl zum Ausführen einer Online-Indizierung von [!DNL MongoMK] ist die [entspricht [!DNL TarMK] Online-Indizierung mit [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) mit der Differenz, dass der Segmentspeicherparameter auf die [!DNL MongoDB] -Instanz, die den Knotenspeicher enthält.

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
* [Apache Jackrabbit Oak oak-run.jar Index Befehlsdokumentation](Https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)

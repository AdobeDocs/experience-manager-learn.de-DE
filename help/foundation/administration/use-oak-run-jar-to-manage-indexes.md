---
title: Verwenden von oak-run.jar zum Verwalten von Indizes
description: 'Der Indexbefehl von oak-run.jar konsolidiert eine Reihe von Funktionen zur Verwaltung von Oak-Indizes in AEM: der Erfassung von Indexstatistiken, der Durchführung von Indexkonsistenzprüfungen und der Neuindizierung/Indizierung von Indizes selbst.'
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
workflow-type: ht
source-wordcount: '449'
ht-degree: 100%

---

# Verwenden von oak-run.jar zum Verwalten von Indizes

Der Index-Befehl [!DNL oak-run.jar] konsolidiert eine Reihe von Funktionen zur Verwaltung von [!DNL Oak]200 Indizes in AEM: der Erfassung von Indexstatistiken über die Durchführung von Indexkonsistenzprüfungen bis hin zur Neuindizierung von Indizes selbst.

>[!NOTE]
>
>In diesem Artikel und in den Videos werden die Begriffe Indizierung und Neuindizierung synonym verwendet und als derselbe Vorgang betrachtet.

## [!DNL oak-run.jar] Grundlagen zum Index-Befehl

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* Die verwendete Version von [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) muss mit der auf der AEM-Instanz verwendeten Version von Oak übereinstimmen.
* Die Verwaltung von Indizes mit [!DNL oak-run.jar] nutzt den Befehl **[!DNL index]** mit verschiedenen Flags zur Unterstützung verschiedener Operationen.

   * `java -jar oak-run*.jar index ...`

## Indexstatistiken

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` sichert alle Indexdefinitionen, wichtige Indexstatistiken und Indexinhalte für Offline-Analysen. 
* Das Sammeln von Indexstatistiken kann sicher auf in Betrieb befindlichen AEM-Instanzen ausgeführt werden.

## Indexkonsistenzprüfung

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` ermittelt schnell, ob Lucene Oak-Indizes beschädigt sind.
* Die Konsistenzprüfung kann problemlos auf einer verwendeten AEM-Instanz ausgeführt werden, um die Konsistenz auf den Ebenen 1 und 2 zu prüfen.

## TarMK Online-Indizierung mit [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* Die Online-Indizierung von [!DNL TarMK] über [!DNL oak-run.jar] ist schneller als die Einstellung von `reindex=true` auf dem Knoten `oak:queryIndexDefinition`.  Trotz dieser Leistungssteigerung erfordert die Online-Indizierung mit [!DNL oak-run.jar] immer noch ein Wartungsfenster, um die Indizierung durchzuführen.

* Die Online-Indizierung von [!DNL TarMK] mit [!DNL oak-run.jar] sollte **nicht** für AEM-Instanzen außerhalb des AEM-Instanzen-Wartungsfensters ausgeführt werden.

## TarMK Offline-Indizierung mit oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* Die Offline-Indizierung von [!DNL TarMK] unter Verwendung von [!DNL oak-run.jar] ist der einfachste [!DNL oak-run.jar]-basierte Indizierungsansatz für [!DNL TarMK], da er einen einzigen [!DNL oak-run.jar]-Befehl erfordert, allerdings muss dazu die AEM-Instanz heruntergefahren werden.

## TarMK Out-of-Band-Indizierung mit oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* Die Out-of-Band-Indizierung auf [!DNL TarMK] mit [!DNL oak-run.jar] minimiert die Auswirkungen der Indizierung auf verwendete AEM-Instanzen.
* Die Out-of-Band-Indizierung ist der empfohlene Indizierungsansatz für AEM-Installationen, bei denen die Zeit für die Neuindizierung/Indizierung die verfügbaren Wartungsfenster überschreitet.

## MongoMK Online-Indizierung mit oak-run.jar

* Der Online-Index mit [!DNL oak-run.jar] auf [!DNL MongoMK] und [!DNL RDBMK] ist die empfohlene Methode für die Neuindizierung von AEM-Installationen mit [!DNL MongoMK] (und [!DNL RDBMK]).  **Es sollte keine andere Methode für [!DNL MongoMK] oder [!DNL RDBMK] angewendet werden.**
* Diese Indizierung muss nur für eine einzelne AEM-Instanz im Cluster ausgeführt werden.
* Die Online-Indizierung von [!DNL MongoMK] kann sicher in einem laufenden AEM-Cluster ausgeführt werden, da der Repository-Durchlauf nur auf einem einzigen [!DNL MongoDB]-Knoten stattfindet, sodass die anderen Knoten weiterhin ohne nennenswerte Leistungseinbußen Anfragen bedienen können.

Der Index-Befehl von [!DNL oak-run.jar] zur Durchführung einer Online-Indizierung von [!DNL MongoMK] ist [genauso wie die [!DNL TarMK] Online-Indizierung mit [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar), mit dem Unterschied, dass der Parameter für den Segment-Speicher auf die [!DNL MongoDB]-Instanz zeigt, die den Knotenspeicher enthält.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## Hilfsmaterialien

* [Download [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *Stellen Sie sicher, dass die heruntergeladene Version mit der auf AEM installierten Version von Oak übereinstimmt, wie oben beschrieben.*
* [Dokumentation der Index-Befehle in Apache Jackrabbit Oak oak-run.jar ](Https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)

---
title: Suchen und Entfernen veralteter APIs in AEM as a Cloud Service
description: Erfahren Sie, wie Sie veraltete APIs in AEM as a Cloud Service finden und entfernen.
version: Experience Manager as a Cloud Service
role: Developer, Architect
level: Beginner
doc-type: tutorial
duration: null
jira: KT-20288
thumbnail: KT-20288.png
last-substantial-update: 2026-02-09T00:00:00Z
exl-id: 287894ea-9cc1-4c27-ac7e-967ad46f4789
source-git-commit: effacd58725dab502f6fb6a4750646c1ea956de2
workflow-type: tm+mt
source-wordcount: '519'
ht-degree: 3%

---

# Suchen und Entfernen veralteter APIs in AEM as a Cloud Service

Erfahren Sie, wie Sie veraltete APIs in AEM as a Cloud Service finden und entfernen.

## Übersicht

AEM as a Cloud Service **Aktionszentrum** benachrichtigt Sie über _veraltete APIs_ in Ihrem Projekt. Um sicherzustellen, dass Ihre Anwendung sicher und leistungsfähig ist und Sie Code mithilfe von Cloud Manager-Pipelines weiter bereitstellen können, entfernen Sie veraltete APIs aus Ihrem Projekt.

In diesem Tutorial erfahren Sie, wie Sie veraltete APIs in Ihrer AEM as a Cloud Service-Umgebung mithilfe des [AEM Analyzer Maven-Plug-ins&rbrace; suchen und &#x200B;](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md).

## So finden Sie veraltete APIs

Führen Sie diese Schritte aus, um veraltete APIs in Ihrem AEM as a Cloud Service-Projekt zu finden.

1. **Verwenden Sie das neueste AEM Analyzer Maven-Plug-in**

   Verwenden Sie in Ihrem AEM-Projekt die neueste Version des [AEM Analyzer Maven-Plug-ins](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md).

   - Im `pom.xml` wird die Plug-in-Version normalerweise deklariert. Vergleichen Sie Ihre Version mit der neuesten [veröffentlichten Version](https://mvnrepository.com/artifact/com.adobe.aem/aemanalyser-maven-plugin).

     ```xml
     ...
     <aemanalyser.version>1.6.14</aemanalyser.version> <!-- Latest released version as of 09-Feb-2026 -->
     ...
     <!-- AEM Analyser Plugin -->
     <plugin>
         <groupId>com.adobe.aem</groupId>
         <artifactId>aemanalyser-maven-plugin</artifactId>
         <version>${aemanalyser.version}</version>
         <extensions>true</extensions>
     </plugin>
     ...
     ```

   - Das Plug-in vergleicht mit der neuesten verfügbaren AEM SDK. Verwenden Sie die neueste AEM SDK-Version in der `pom.xml` Ihres Projekts. Dies hilft, veraltete APIs als IDE-Warnungen darzustellen.

     ```xml
     ...
     <aem.sdk.api>2026.2.24288.20260204T121510Z-260100</aem.sdk.api> <!-- Latest available AEM SDK version as of 09-Feb-2026 -->
     ...
     ```

   - Stellen Sie sicher, dass das `all`-Modul das Plug-in in der `verify` Phase ausführt.

     ```xml
     ...
     <build>
         <plugins>
             ...
             <plugin>
                 <groupId>com.adobe.aem</groupId>
                 <artifactId>aemanalyser-maven-plugin</artifactId>
                 <extensions>true</extensions>
                 <executions>
                     <execution>
                         <id>analyse-project</id>
                         <phase>verify</phase>
                         <goals>
                             <goal>project-analyse</goal>
                         </goals>
                     </execution>
                 </executions>
             </plugin>
             ...
         </plugins>
     </build>
     ...
     ```

2. **Führen Sie einen Build aus und überprüfen Sie auf Warnungen**

   Wenn Sie `mvn clean install` ausführen, meldet der Analyzer veraltete APIs als **[WARNUNG]**-Meldungen in der Ausgabe. Zum Beispiel:

   ```shell
   ...
   [WARNING] The analyser found the following warnings for author and publish :
   [WARNING] [region-deprecated-api] com.adobe.aem.guides:aem-guides-wknd.core:4.0.5-SNAPSHOT: Usage of deprecated package found : org.apache.commons.lang : Commons Lang 2 is in maintenance mode. Commons Lang 3 should be used instead. Deprecated since 2021-04-30 For removal : 2021-12-31 (com.adobe.aem.guides:aem-guides-wknd.all:4.0.5-SNAPSHOT)
   ...
   ```

   Es ist einfach, diese Meldungen zu übersehen, wenn man sich auf den Erfolg oder Misserfolg eines Builds konzentriert.

3. **Erhalten Sie eine klare Liste veralteter APIs**

   Der obige Schritt enthält auch dieselben Informationen. Führen Sie jedoch die `verify` Phase auf dem `all` aus, um alle **[WARNUNG]**-Meldungen an einem Ort anzuzeigen. Zum Beispiel:

   ```shell
   $ mvn clean verify -pl all
   ```

   Die **[WARNUNG]**-Meldungen in der Build-Ausgabe führen die veralteten APIs in Ihrem Projekt auf.

## Entfernen veralteter APIs

Der AEM Analyzer meldet **Was** als veraltet und bietet die **Empfehlung** zur Fehlerbehebung. Verwenden Sie jedoch die nachstehende Tabelle, um die richtige Aktion auszuwählen, und folgen Sie der verknüpften Dokumentation, wenn Sie weitere Details benötigen.

### Veraltete API-Korrekturstrategie

| Warnungstyp des Analyzers | Was es anzeigt | Empfohlene Aktion | Verweis |
| --------------------- | ----------------- | ------------------ | --------- |
| Veraltete AEM-API | API muss aus AEM as a Cloud Service entfernt werden | Ersetzen der Verwendung durch die unterstützte öffentliche API | [API-Entfernungsanleitung](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| Veraltetes AEM-Paket oder veraltete Klasse | Paket oder Klasse wird nicht mehr unterstützt | Code zur Verwendung der empfohlenen Alternative umgestalten | [Veraltete APIs](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#aem-apis) |
| Veraltete Drittanbieterbibliothek | Bibliothek wird in zukünftigen SDKs nicht unterstützt | Aktualisieren der Abhängigkeit und Refaktorierung der Nutzung | [Allgemeine Richtlinien](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| Veraltete Sling-/OSGi-Muster | Alte Anmerkungen oder APIs erkannt | Migrieren zu modernen Sling- und OSGi-APIs | [Entfernen von Sling-/OSGi-Mustern](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| Geplante Entfernung (Datum in der Zukunft) | Die API funktioniert weiterhin, aber die Entfernung wird später erzwungen | Planen der Bereinigung vor der Pipeline-Durchsetzung | [Versionshinweise](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/release-notes/home) |

### Praktische Leitlinien

- Analyzer-Warnungen als **zukünftige Pipeline-Fehler** behandeln, nicht als optionale Nachrichten.
- Korrigieren Sie veraltete APIs lokal mit **neuesten AEM SDK**.
- Halten Sie die Analyzer-Ausgabe sauber, um Probleme bei zukünftigen AEM-Upgrades zu vermeiden.

Durch die frühzeitige Behebung veralteter APIs bleibt Ihr Projekt **sicher und bereitstellungsbereit**.

## Zusätzliche Ressourcen

- [AEM Analyzer-Maven-Plug-in](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md)
- [Veraltete und entfernte Funktionen und APIs](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance)

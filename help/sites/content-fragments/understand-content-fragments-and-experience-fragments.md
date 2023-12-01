---
title: Inhaltsfragmente und Experience Fragments
description: Erfahren Sie mehr über die Ähnlichkeiten und Unterschiede zwischen Inhaltsfragmenten und Experience Fragments sowie darüber, wann und wie der jeweilige Typ verwendet wird.
sub-product: Experience Manager Assets, Experience Manager Sites
feature: Content Fragments, Experience Fragments
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Article
exl-id: ccbc68d1-a83e-4092-9a49-53c56c14483e
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1044'
ht-degree: 100%

---

# Inhaltsfragmente und Experience Fragments

Inhaltsfragmente und Experience Fragments von Adobe Experience Manager scheinen sich oberflächlich betrachtet zwar zu ähneln, spielen aber jeweils eine wichtige Rolle in unterschiedlichen Anwendungsfällen. Erfahren Sie, inwiefern sich Inhaltsfragmente und Experience Fragments ähnlich sind, sich unterscheiden und wann und wie sie verwendet werden.

## Vergleich

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>Inhaltsfragmente (Content Fragments, CF)</strong></td>
<td><strong>Experience Fragments (XF)</strong></td>
</tr><tr><td><strong>Definition</strong></td>
<td><ul>
<li>Wiederverwendbarer, präsentationsunabhängiger <strong>Inhalt</strong>, der aus strukturierten Datenelementen (Text, Daten, Verweisen usw.) besteht</li>
</ul>
</td>
<td><ul>
<li>Ein wiederverwendbarer Verbund aus einer oder mehreren AEM-Komponenten, der den Inhalt und die Präsentation definiert und so ein eigenständiges, sinnvolles <strong>Erlebnis</strong> schafft</li>
</ul>
</td>
</tr><tr><td><strong>Kernmandanten</strong></td>
<td><ul>
<li>Inhaltsorientiert</li>
<li>Definiert durch ein <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=de" target="_blank">strukturiertes, formularbasiertes Datenmodell</a></li>
<li>Unabhängig von Design und Layout</li>
<li>Der Kanal ist für die Präsentation des Inhalts des Inhaltsfragments (Layout und Design) verantwortlich</li>
</ul>
</td>
<td><ul>
<li>Präsentationsorientiert</li>
<li>Definiert durch eine unstrukturierte Komposition der AEM-Komponenten</li>
<li>Definiert das Design und Layout von Inhalten</li>
<li>Wird in Kanälen unverändert verwendet</li>
</ul>
</td>
</tr><tr><td><strong>Technische Details</strong></td>
<td><ul>
<li>Implementiert als <strong>dam:Asset</strong></li>
<li>Definiert durch ein <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=de" target="_blank">Inhaltsfragmentmodell</a></li>
</ul>
</td>
<td><ul>
<li>Implementiert als <strong>cq:Page</strong></li>
<li>Definiert durch bearbeitbare Vorlagen</li>
<li>Native HTML-Ausgabedarstellung</li>
</ul>
</td>
</tr><tr><td><strong>Varianten</strong></td>
<td><ul>
<li>Pimärvariante = kanonische Variante</li>
<li>Anwendungsspezifische Varianten, die an Kanälen ausgerichtet werden können</li>
</ul>
</td>
<td><ul>
<li>Kanal- oder kontextspezifische Varianten</li>
<li>Synchronisierung der Varianten über AEM Live Copy</li>
<li>Wiederverwendung von Inhalten in verschiedenen Varianten durch <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=de" target="_blank">Bausteine</a></li>
</ul>
</td>
</tr><tr><td><strong>Funktionen</strong></td>
<td><ul>
<li>Varianten</li>
<li>Versionen</li>
<li>Variantenübergreifende <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=de#synchronizing-with-master" target="_blank">Synchronisierung</a> von Inhalten</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-managing.html?lang=de#comparing-fragment-versions" target="_blank">Visuelle Gegenüberstellung</a> von Inhaltsfragmentversionen</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=de#annotating-a-content-fragment" target="_blank">Anmerkungen</a> bei mehrzeiligen Textelementen</li>
<li>Intelligente <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=de#summarizing-text" target="_blank">Zusammenfassung</a> von mehrzeiligen Textelementen</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/creating-translation-projects-for-content-fragments.html?lang=de" target="_blank">Übersetzung/Lokalisierung</a></li>
</ul>
</td>
<td><ul>
<li>Varianten</li>
<li>Varianten als Live Copys</li>
<li>Versionen</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=de#building-blocks" target="_blank">Bausteine</a></li>
<li>Anmerkungen</li>
<li>Responsives Layout und Vorschau</li>
<li>Übersetzung/Lokalisierung</li>
<li>Komplexes Datenmodell über Inhaltsfragmentverweise</li>
<li>In-App-Vorschau</li>
</ul>
</td>
</tr><tr><td><strong>Verwendung</strong></td>
<td><ul>
<li>JSON-Export über <a href="https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=de">AEM Headless-GraphQL-APIs</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=de" target="_blank">Inhaltsfragment-Komponente der AEM-Kernkomponenten</a> zur Verwendung in AEM Sites, AEM Screens oder Experience Fragments</li>
<li>JSON-Export über <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=de" target="_blank">AEM Content Services</a> zur Nutzung durch Dritte</li>
<li>JSON-Export nach Adobe Target für zielgerichtete Angebote</li>
<li>JSON-Export über AEM HTTP Assets-APIs zur Nutzung durch Dritte</li>
</ul>
</td>
<td><ul>
<li>AEM Experience Fragment-Komponente zur Verwendung in AEM Sites, AEM Screens oder anderen Experience Fragments</li>
<li>Export als <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=de" target="_blank">einfaches HTML</a> zur Nutzung durch Drittanbietersysteme</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=de" target="_blank">HTML-Export in Adobe Target</a> für zielgerichtete Angebote</li>
<li>JSON-Export nach Adobe Target für zielgerichtete Angebote</li>
</ul>
</td>
</tr><tr><td><strong>Häufige Anwendungsfälle</strong></td>
<td><ul>
<li>Headless-Anwendungsfälle über GraphQL</li>
<li>Strukturierte Dateneingabe/formularbasierte Inhalte</li>
<li>Lange redaktionelle Inhalte (mehrzeiliges Element)</li>
<li>Management von Inhalten außerhalb des Lebenszyklus der Kanäle, die diese bereitstellen</li>
</ul>
</td>
<td><ul>
<li>Zentralisiertes Management von mehrkanaligen Werbematerialien mithilfe von Kanalvarianten</li>
<li>Wiederverwendung von Inhalte auf mehreren Seiten einer Website</li>
<li>Website-Chrome (z. B. Kopf- und Fußzeile)</li>
<li>Management eines Erlebnisses außerhalb des Lebenszyklus der Kanäle, die es bereitstellen</li>
</ul>
</td>
</tr><tr><td><strong>Dokumentation</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=de&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">Benutzerhandbuch für AEM-Inhaltsfragmente</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=de" target="_blank">Verwenden von Inhaltsfragmenten in AEM</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=de" target="_blank">Adobe-Dokumentation zu Experience Fragments</a></li>
</ul>
</td>
</tr></tbody></table>

## Architektur von Inhaltsfragmenten

Das folgende Diagramm zeigt die allgemeine Architektur für AEM-Inhaltsfragmente.

![Architektur von Inhaltsfragmenten](./assets/content-fragments-architecture.png)

+ **Inhaltsfragmentmodelle** definieren die Elemente (oder Felder), die festlegen, welche Inhalte vom Inhaltsfragment erfasst und bereitgestellt werden können.
+ Das **Inhaltsfragment** ist eine Instanz eines Inhaltsfragmentmodells, das eine logische Inhaltsentität darstellt.
+ **Inhaltsfragmentvarianten** folgen dem Inhaltsfragmentmodell, weisen jedoch Varianten in Bezug auf den Inhalt auf.
+ Inhaltsfragmente können wie folgt bereitgestellt/genutzt werden:
   + Verwenden von Inhaltsfragmenten in **AEM Sites** (oder AEM Screens) über die Inhaltsfragment-Komponente der AEM WCM-Kernkomponenten
   + Nutzen von **Inhaltsfragmenten** aus Headless-Apps mit AEM Headless-GraphQL-APIs
   + Bereitstellen der Inhalte von Inhaltsfragmentvarianten als JSON über **AEM Content Services** und API-Seiten für schreibgeschützte Anwendungsfälle
   + Direktes Bereitstellen von Inhaltsfragment-Inhalten (alle Varianten) als JSON per Direktaufruf an AEM Assets über die **AEM Assets HTTP-API** für CRUD-Anwendungsfälle

## Architektur von Experience Fragments

![Architektur von Experience Fragments](./assets/experience-fragments-architecture.png)

+ **Bearbeitbare Vorlagen**, die wiederum durch **bearbeitbare Vorlagentypen** und **Implementierung einer AEM-Seitenkomponente** definiert werden, bestimmen die zulässigen AEM-Komponenten, die zur Experience Fragment-Erstellung verwendet werden können.
+ Ein **Experience Fragment** ist eine Instanz einer bearbeitbaren Vorlage, die ein logisches Erlebnis darstellt.
+ Experience Fragment-**Varianten** folgen der bearbeitbaren Vorlage, weisen jedoch Varianten in Bezug auf das Erlebnis (Inhalt und Design) auf.
+ Experience Fragments können wie folgt bereitgestellt/genutzt werden:
   + Verwenden von Experience Fragments in AEM Sites (oder AEM Screens) über die AEM Experience Fragment-Komponente
   + Bereitstellen von Experience Fragment-Varianteninhalten als JSON (mit eingebettetem HTML) über **AEM Content Services** und API-Seiten
   + Direktes Bereitstellen einer Experience Fragment-Variante als **einfaches HTML**
   + Exportieren von Experience Fragments in **Adobe Target** als HTML- oder JSON-Angebote
   + Unterstützung nativer HTML-Angebote durch AEM Sites, für JSON-Angebote ist aber eine benutzerdefinierte Entwicklung erforderlich

## Unterstützende Ressourcen für Inhaltsfragmente

+ [Benutzerhandbuch zu Inhaltsfragmenten](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=de&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Einführung in Adobe Experience Manager als Headless-CMS](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/introduction.html?lang=de)
+ [Verwenden von Inhaltsfragmenten in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=de)
+ [Inhaltsfragment-Komponente der AEM WCM-Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=de)
+ [Verwenden von Inhaltsfragmenten und AEM Headless](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=de)
+ [Erste Schritte mit AEM Content Services](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=de)

## Unterstützende Ressourcen für Experience Fragments

+ [Adobe-Dokumentation zu Experience Fragments](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=de)
+ [Grundlegendes zu AEM Experience Fragments](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=de)
+ [Verwenden von AEM Experience Fragments](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=de)
+ [Verwenden von AEM Experience Fragments mit Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)

---
title: Grundlegendes zu Inhaltsfragmenten und Experience Fragments
description: Erfahren Sie mehr über die Ähnlichkeiten und Unterschiede zwischen Inhaltsfragmenten und Experience Fragments sowie darüber, wann und wie die einzelnen Typen verwendet werden.
sub-product: assets, sites, content services
feature: Content Fragments, Experience Fragments
topics: headless
version: 6.4, 6.5
doc-type: article
activity: understand
audience: all
topic: Content Management
role: User
level: Beginner
exl-id: ccbc68d1-a83e-4092-9a49-53c56c14483e
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '1016'
ht-degree: 4%

---

# Grundlegendes zu Inhaltsfragmenten und Experience Fragments

Adobe Experience Manager-Inhaltsfragmente und Experience Fragments scheinen sich zwar auf der Oberfläche ähnlich, spielen jedoch in verschiedenen Anwendungsfällen jeweils eine wichtige Rolle. Erfahren Sie, wie Inhaltsfragmente und Experience Fragments ähnlich sind, sich unterscheiden und wann und wie sie verwendet werden.

## Vergleich von Inhaltsfragmenten und Experience Fragments

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>Inhaltsfragmente</strong></td>
<td><strong>Experience Fragments (XF)</strong></td>
</tr><tr><td><strong>Definition</strong></td>
<td><ul>
<li>Wiederverwendbar, Präsentationsunabhängig <strong>content</strong>, das aus strukturierten Datenelementen (Text, Daten, Verweise usw.)</li>
</ul>
</td>
<td><ul>
<li>Ein wiederverwendbares Composite aus einer oder mehreren AEM Komponenten, die Inhalte und Darstellungen definieren, die eine <strong>Erlebnis</strong> was allein Sinn ergibt</li>
</ul>
</td>
</tr><tr><td><strong>Kernmandanten</strong></td>
<td><ul>
<li>Inhaltsorientiert</li>
<li>Definiert durch eine <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">strukturiertes, formularbasiertes Datenmodell.</a></li>
<li>Design und Layout sind unabhängig.</li>
<li>Der Kanal ist für die Darstellung des Inhalts des Inhaltsfragments (Layout und Design) verantwortlich</li>
</ul>
</td>
<td><ul>
<li>Präsentation zentriert</li>
<li>Definiert durch unstrukturierte Zusammensetzung AEM Komponenten</li>
<li>Definiert das Design und Layout von Inhalten</li>
<li>Wird in Kanälen unverändert verwendet</li>
</ul>
</td>
</tr><tr><td><strong>Technische Details</strong></td>
<td><ul>
<li>Implementiert als <strong>dam:Asset</strong></li>
<li>Definiert durch eine <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">Inhaltsfragmentmodell</a></li>
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
<li>Die Übergeordnete Variante ist die kanonische Variante.</li>
<li>Varianten sind anwendungsspezifisch und können an Kanälen ausgerichtet werden.</li>
</ul>
</td>
<td><ul>
<li>Varianten sind kanal- oder kontextspezifisch</li>
<li>Varianten werden über AEM Live Copy synchronisiert</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html" target="_blank">Bausteine</a> die Wiederverwendung von Inhalten in verschiedenen Varianten zulassen</li>
</ul>
</td>
</tr><tr><td><strong>Funktionen</strong></td>
<td><ul>
<li>Varianten</li>
<li>Versionen</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#synchronizing-with-master" target="_blank">Synchronisierung</a> Inhaltsvarianten</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-managing.html?lang=en#comparing-fragment-versions" target="_blank">Visual Diff</a> von Inhaltsfragmentversionen</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#annotating-a-content-fragment" target="_blank">Anmerkungen</a> von mehrzeiligen Textelementen</li>
<li>Intelligent <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#summarizing-text" target="_blank">Zusammenfassung</a> von mehrzeiligen Textelementen.</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/creating-translation-projects-for-content-fragments.html?lang=en" target="_blank">Übersetzung/Lokalisierung</a></li>
</ul>
</td>
<td><ul>
<li>Varianten</li>
<li>Varianten als Live Copies</li>
<li>Versionen</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en#building-blocks" target="_blank">Bausteine</a></li>
<li>Anmerkungen</li>
<li>Responsives Layout und Vorschau</li>
<li>Übersetzung/Lokalisierung</li>
</ul>
</td>
</tr><tr><td><strong>Verwenden Sie</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">AEM Kernkomponenten Inhaltsfragment-Komponente</a> zur Verwendung in AEM Sites, AEM Screens oder Experience Fragments.</li>
<li>JSON-Export über <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en" target="_blank">AEM Content Services</a> für den Verbrauch von Drittanbietern</li>
<li>JSON über AEM HTTP Assets-APIs für die Nutzung durch Drittanbieter.</li>
</ul>
</td>
<td><ul>
<li>AEM Experience Fragment-Komponente zur Verwendung in AEM Sites, AEM Screens oder anderen Experience Fragments.</li>
<li>Exportieren als <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">Nur HTML</a> zur Verwendung durch Drittanbietersysteme</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=en" target="_blank">HTML-Export in Adobe Target</a> für zielgerichtete Angebote</li>
<li>JSON-Export in Adobe Target für zielgerichtete Angebote</li>
</ul>
</td>
</tr><tr><td><strong>Häufige Anwendungsfälle</strong></td>
<td><ul>
<li>Hochstrukturierte Dateneingabe/formularbasierter Inhalt</li>
<li>Lange redaktionelle Inhalte (mehrzeiliges Element)</li>
<li>Inhalt, der außerhalb des Lebenszyklus der Kanäle verwaltet wird, die ihn bereitstellen</li>
</ul>
</td>
<td><ul>
<li>Zentralisiertes Management von mehrkanaligen Werbematerialien anhand von Kanalvarianten.</li>
<li>Inhalte werden auf mehreren Seiten einer Website wiederverwendet.</li>
<li>Website-Chrome (z. B. Kopf- und Fußzeile)</li>
<li>Ein Erlebnis, das außerhalb des Lebenszyklus der Kanäle verwaltet wird, die es bereitstellen</li>
</ul>
</td>
</tr><tr><td><strong>Dokumentation</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">Benutzerhandbuch für AEM Inhaltsfragmente</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en" target="_blank">Verwenden von Inhaltsfragmenten in AEM</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">Adobe-Dokumentation zu Experience Fragments</a></li>
</ul>
</td>
</tr></tbody></table>

## Architektur von Inhaltsfragmenten

Das folgende Diagramm zeigt die Gesamtarchitektur für AEM Inhaltsfragmente

!![Architektur von Inhaltsfragmenten](./assets/content-fragments-architecture.png)

+ **Inhaltsfragmentmodelle** definieren die Elemente (oder Felder), die definieren, welche Inhalte das Inhaltsfragment erfassen und verfügbar machen kann.
+ Die **Inhaltsfragment** ist eine Instanz eines Inhaltsfragmentmodells, das eine logische Inhaltsentität darstellt.
+ Inhaltsfragment **Varianten** beim Inhaltsfragmentmodell bleiben, jedoch Inhaltsvarianten aufweisen.
+ Inhaltsfragmente können wie folgt verfügbar gemacht/genutzt werden:
   + Verwenden von Inhaltsfragmenten in **AEM Sites** (oder AEM Screens) über die Inhaltsfragment-Komponente der AEM WCM-Kernkomponenten.
   + Einbetten eines Inhaltsfragments in ein **Experience Fragment** über die Inhaltsfragment-Komponente der AEM WCM-Kernkomponenten zur Verwendung in allen Anwendungsfällen von Experience Fragment.
   + Anzeigen von Inhaltsfragmenten, die Inhalte als JSON über **AEM Content Services** und API-Seiten für schreibgeschützte Anwendungsfälle.
   + Direkte Anzeige von Inhaltsfragmentinhalten (alle Varianten) als JSON über Direktaufrufe an AEM Assets über die **AEM Assets HTTP-API** für CRUD-Anwendungsfälle.

## Experience Fragments-Architektur

!![Experience Fragments-Architektur](./assets/experience-fragments-architecture.png)

+ **Bearbeitbare Vorlagen**, die wiederum durch **Bearbeitbare Vorlagentypen** und **Implementierung AEM Seitenkomponenten**, definieren Sie die zulässigen AEM Komponenten, die zum Erstellen eines Experience Fragment verwendet werden können.
+ Die **Experience Fragment** ist eine Instanz einer bearbeitbaren Vorlage, die ein logisches Erlebnis darstellt.
+ Experience Fragment **Varianten** die bearbeitbare Vorlage einzuhalten, jedoch Varianten des Erlebnisses (Inhalt und Design) aufweisen.
+ Experience Fragments können wie folgt verfügbar gemacht/genutzt werden:
   + Verwenden von Experience Fragments in AEM Sites (oder AEM Screens) über die AEM Experience Fragment-Komponente.
   + Anzeigen eines Experience Fragment-Varianten-Inhalts als JSON (mit eingebettetem HTML) via **AEM Content Services** und API-Seiten.
   + Direktes Anzeigen einer Experience Fragment-Variante als **&quot;Plain HTML&quot;**.
   + Exportieren von Experience Fragments in **Adobe Target** als HTML- oder JSON-Angebote.
   + AEM Sites unterstützt nativ HTML-Angebote. JSON-Angebote erfordern jedoch eine benutzerdefinierte Entwicklung.

## Unterstützende Materialien für Inhaltsfragmente

+ [Benutzerhandbuch zu Inhaltsfragmenten](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Verwenden von Inhaltsfragmenten in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en)
+ [Inhaltsfragment-Komponente AEM WCM-Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=de)
+ [Verwenden von Inhaltsfragmenten und AEM Headless](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=de)
+ [Erste Schritte mit AEM Content Services](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en)

## Unterstützende Materialien für Experience Fragments

+ [Adobe-Dokumentation zu Experience Fragments](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en)
+ [Grundlegendes zu AEM Experience Fragments](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [Verwenden AEM Experience Fragments](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [Verwenden AEM Experience Fragments mit Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)

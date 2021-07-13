---
title: Grundlegendes zu Inhaltsfragmenten und Experience Fragments
description: Adobe Experience Manager-Inhaltsfragmente und Experience Fragments scheinen sich zwar auf der Oberfläche ähnlich, spielen jedoch in verschiedenen Anwendungsfällen jeweils eine wichtige Rolle. Erfahren Sie, wie Inhaltsfragmente und Experience Fragments ähnlich sind, sich unterscheiden und wann und wie sie verwendet werden.
sub-product: Assets, Sites, Inhaltsdienste
feature: Inhaltsfragmente, Experience Fragments
topics: headless
version: 6.3, 6.4, 6.5
doc-type: article
activity: understand
audience: all
topic: Content Management
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '1004'
ht-degree: 8%

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
<li>Wiederverwendbar, darstellungsagnostisch <strong>content</strong>, bestehend aus strukturierten Datenelementen (Text, Daten, Verweise usw.)</li>
</ul>
</td>
<td><ul>
<li>Ein wiederverwendbares Composite aus einer oder mehreren AEM Komponenten, die Inhalte und Darstellungen definieren, die ein <strong>Erlebnis</strong> bilden, das allein Sinn ergibt</li>
</ul>
</td>
</tr><tr><td><strong>Kernmandanten</strong></td>
<td><ul>
<li>Inhaltsorientiert</li>
<li>Definiert durch ein <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">strukturiertes, formularbasiertes Datenmodell.</a></li>
<li>Design und Layout sind unabhängig.</li>
<li>Der Kanal ist für die Präsentation des Inhalts des Inhaltsfragments (Layout und Design) verantwortlich.</li>
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
<li>Definiert durch ein <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">Inhaltsfragmentmodell</a></li>
</ul>
</td>
<td><ul>
<li>Implementiert als <strong>cq:Page</strong></li>
<li>Definiert durch bearbeitbare Vorlagen</li>
<li>Native HTML-Ausgabe</li>
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
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">Erstellen </a> von Blocksalinhalten zur Wiederverwendung in verschiedenen Varianten</li>
</ul>
</td>
</tr><tr><td><strong>Funktionen</strong></td>
<td><ul>
<li>Varianten</li>
<li>Versionen</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SynchronizingwithMaster" target="_blank"></a> Synchronisierung von Inhalten in verschiedenen Varianten</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-managing.html#ComparingFragmentVersions" target="_blank">Visuelle Unterschiede </a> bei Inhaltsfragmentversionen</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#AnnotatingaContentFragment" target="_blank"></a> Anmerkungen zu mehrzeiligen Textelementen</li>
<li>Intelligente <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SummarizingText" target="_blank">Zusammenfassung</a> von mehrzeiligen Textelementen.</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/creating-translation-projects-for-content-fragments.html" target="_blank">Übersetzung/Lokalisierung</a></li>
</ul>
</td>
<td><ul>
<li>Varianten</li>
<li>Varianten als Live Copies</li>
<li>Versionen</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">Bausteine</a></li>
<li>Anmerkungen</li>
<li>Responsives Layout und Vorschau</li>
<li>Übersetzung/Lokalisierung</li>
</ul>
</td>
</tr><tr><td><strong>Verwenden Sie</strong></td>
<td><ul>
<li><a href="https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">AEM Inhaltsfragment-</a> Komponente der Kernkomponenten zur Verwendung in AEM Sites, AEM Screens oder in Experience Fragments.</li>
<li>JSON-Export über <a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html" target="_blank">AEM Content Services</a> für den Verbrauch durch Dritte</li>
<li>JSON über AEM HTTP Assets-APIs für die Nutzung durch Drittanbieter.</li>
</ul>
</td>
<td><ul>
<li>AEM Experience Fragment-Komponente zur Verwendung in AEM Sites, AEM Screens oder anderen Experience Fragments.</li>
<li>Exportieren Sie als <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#ThePlainHTMLRendition" target="_blank">Nur HTML</a> zur Verwendung durch Drittanbietersysteme.</li>
<li><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/experience-fragments-target.html" target="_blank">HTML-Export in Adobe </a> Targeting für zielgerichtete Angebote</li>
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
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">Benutzerhandbuch für AEM Inhaltsfragmente</a></li>
<li><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html" target="_blank">Verwenden von Inhaltsfragmenten in AEM</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html" target="_blank">Adobe-Dokumentation zu Experience Fragments</a></li>
</ul>
</td>
</tr></tbody></table>

## Architektur von Inhaltsfragmenten

Das folgende Diagramm zeigt die Gesamtarchitektur für AEM Inhaltsfragmente

!![Architektur von Inhaltsfragmenten](./assets/content-fragments-architecture.png)

+ **Inhaltsfragmentmodelle** definieren die Elemente (oder Felder), die definieren, welche Inhalte das Inhaltsfragment erfassen und verfügbar machen kann.
+ Das **Inhaltsfragment** ist eine Instanz eines Inhaltsfragmentmodells, das eine logische Inhaltsentität darstellt.
+ Inhaltsfragmente **Varianten** halten sich an das Inhaltsfragmentmodell, weisen jedoch Inhaltsvarianten auf.
+ Inhaltsfragmente können wie folgt verfügbar gemacht/genutzt werden:
   + Verwenden von Inhaltsfragmenten für **AEM Sites** (oder AEM Screens) über die Inhaltsfragment-Komponente der AEM WCM-Kernkomponenten.
   + Einbetten eines Inhaltsfragments in ein **Experience Fragment** über die Inhaltsfragment-Komponente der AEM WCM-Kernkomponenten zur Verwendung in allen Anwendungsfällen von Experience Fragment.
   + Beim Anzeigen eines Inhaltsfragments wird der Inhalt als JSON über **AEM Content Services** und API-Seiten für schreibgeschützte Anwendungsfälle variiert.
   + Direktes Freigeben von Inhaltsfragmentinhalten (alle Varianten) als JSON über Direktaufrufe an AEM Assets über die **AEM Assets HTTP API** für CRUD-Anwendungsfälle.

## Experience Fragments-Architektur

!![Experience Fragments-Architektur](./assets/experience-fragments-architecture.png)

+ **Bearbeitbare Vorlagen**, die wiederum durch  **bearbeitbare** Vorlagentypen und eine Implementierung  **AEM Seitenkomponenten** definiert werden, definieren die zulässigen AEM Komponenten, die zum Erstellen eines Experience Fragment verwendet werden können.
+ Das **Experience Fragment** ist eine Instanz einer bearbeitbaren Vorlage, die ein logisches Erlebnis darstellt.
+ Experience Fragment **Varianten** halten sich an die bearbeitbare Vorlage. Es gibt jedoch Varianten des Erlebnisses (Inhalt und Design).
+ Experience Fragments können wie folgt verfügbar gemacht/genutzt werden:
   + Verwenden von Experience Fragments in AEM Sites (oder AEM Screens) über die AEM Experience Fragment-Komponente.
   + Beim Anzeigen eines Experience Fragment wird der Inhalt als JSON (mit eingebettetem HTML) über **AEM Content Services** und API-Seiten variiert.
   + Direktes Anzeigen einer Experience Fragment-Variante als **&quot;Nur HTML&quot;**.
   + Exportieren von Experience Fragments in **Adobe Target** als HTML- oder JSON-Angebote.
   + AEM Sites unterstützt nativ HTML-Angebote. JSON-Angebote erfordern jedoch eine benutzerdefinierte Entwicklung.

## Unterstützende Materialien für Inhaltsfragmente

+ [Benutzerhandbuch zu Inhaltsfragmenten](https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Verwenden von Inhaltsfragmenten in AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html)
+ [Inhaltsfragment-Komponente AEM WCM-Kernkomponenten](https://docs.adobe.com/content/help/de/experience-manager-core-components/using/components/content-fragment-component.html)
+ [Verwenden von Inhaltsfragmenten und AEM Content Services](https://helpx.adobe.com/experience-manager/kt/sites/using/structured-fragments-content-services-feature-video-use.html)
+ [Erste Schritte mit AEM Content Services](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html)

## Unterstützende Materialien für Experience Fragments

+ [Adobe-Dokumentation zu Experience Fragments](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
+ [Grundlegendes zu AEM Experience Fragments](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-understand.html)
+ [Verwenden AEM Experience Fragments](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html)
+ [Verwenden AEM Experience Fragments mit Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)

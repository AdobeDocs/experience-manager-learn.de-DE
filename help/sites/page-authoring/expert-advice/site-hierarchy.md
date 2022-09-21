---
title: Tipps zu Site-Hierarchie, Taxonomie und Tagging
description: Best Practices für Site-Hierarchie, Taxonomie und Tagging
hide: true
hidefromtoc: true
source-git-commit: 3eb429039589ae26a81bc6d24f020a77517133e8
workflow-type: tm+mt
source-wordcount: '2053'
ht-degree: 0%

---


# Best Practices für Tags, Taxonomie und Metadaten: Allgemeine Zusammenfassung

Metadaten und Tags sind entscheidend für die Steigerung der Effizienz in AEM. Benutzer, Führungskräfte und Management erkennen die Notwendigkeit einer ganzheitlichen Strategie, aber sie finden es schwierig, Fortschritte zu erzielen. Oft wird Wissen bei den Nutzern vernachlässigt, was eine ganzheitliche Strategie erschwert - und die Anpassung noch problematischer macht.

Was ist der Unterschied zwischen Metadaten und Tags? Welche geschäftlichen Aspekte sollten bei der Umsetzung Ihrer Strategie berücksichtigt werden?

Eine intensivere Zusammenfassung finden Sie hier . [here](https://adobe.sharepoint.com/:w:/r/sites/ACSSuccessServices/_layouts/15/Doc.aspx?sourcedoc=%7BFE5E873A-A3B6-4F40-BF22-A2C9F1269802%7D&amp;file=AEM_TagTaxonomyAndMetadata_BestPractice_en%20(2).docx&amp;action=default&amp;mobileredirect=true).

## Welchen Zweck haben Metadaten?

Metadaten fügen Struktur zu weniger strukturierten Inhalten hinzu.
Beispiel: Ein Grundbild hat Pixel. Wir können diese &quot;Kerndaten&quot;nennen. Es sind die Metadaten, die das Format, die Kategorie, Lizenzdetails usw. beschreiben.
Metadaten werden am häufigsten für Assets verwendet. Es gibt jedoch auch eine große Anzahl von Anwendungsfällen für Metadaten in Inhaltsseiten oder Experience Fragments.

## Quellen für Metadaten

Im Folgenden finden Sie die Kategorien, für die Metadaten generiert werden können:

* Extrahierte Metadaten - Die Informationen sind bereits im Dokument verfügbar, z. B. in natürlicher Sprache.
* Abgeleitete Metadaten - Die Informationen sind nicht in den Originaldaten verfügbar, können jedoch durch Querverweise auf frühere Kenntnisse abgeleitet werden.
* Manuell hinzugefügte Metadaten: Dies sind Metadaten, die nicht in eine der ersten Kategorien fallen und von einem Menschen manuell hinzugefügt werden müssen.

## Typen von Metadaten

Innerhalb der oben aufgeführten Kategorien gibt es vier Haupttypen:

* Technische und beschreibende Metadaten: Bietet Informationen zu technischen Details des Inhalts (d. h. Titel, Sprache usw.)
* Operative Metadaten: Dokumentiert den Lebenszyklus eines Assets (d. h. genehmigt, in kreativen Kampagnen)
* Administrative Metadaten: Status oder Status eines Assets innerhalb einer Organisation (d. h. Lizenzinformationen, Eigentümer)
* Strukturelle Metadaten: Hilft bei der Kategorisierung von Assets oder Seiten für einen reibungslosen Geschäftsprozess (gilt für die meisten Tags und Taxonomien)

## Ordner- und Dateinamen

Ordner sind eine natürliche Möglichkeit, um in AEM Inhalt zu navigieren und ihn zu durchsuchen. Wie werden Ihre Stakeholder mit AEM interagieren? Dadurch wird bestimmt, wie Ihre Ordner strukturiert sind. Normalerweise wird die Ordnerstruktur mit einer (oder zwei) der folgenden Einstellungen erstellt:

* Navigation
* Browsen
* Kategorisierung
* Zugriffssteuerung

Bei AEM Sites ist die Navigation wichtig. Ordner werden verwendet, um den Zugriff auf Assets und Seiten zu steuern.

Welche Autorenebenen benötigen Zugriff auf Homepages? Was ist mit Produktseiten? Oder Kampagne? Verwenden Sie Berechtigungen und die Ordnerstruktur, um die richtige Governance festzulegen.

## Speichern von Metadaten

Es gibt drei Methoden zum Speichern von Metadaten:

* binär: Binärformat bezieht sich auf die Art des Assets (Photoshop, InDesign, PNG, JPG).
* Asset-Knoten: Dies sind Metadaten für das Asset selbst, unabhängig vom verwendeten System oder Prozess.
* Externer Speicherort: Metadaten, die sich nicht direkt auf dem Asset befinden, aber als Deskriptor für den &quot;Status&quot;eines Assets verwendet werden können (Beispiel: Workflows, die sich auf ein Asset auswirken, aber nicht direkt darauf angewendet werden können

## Metadatenmodell

Die Struktur der Erfassung und Formatierung von Metadaten wird als Metadatenmodell oder Metadatenschema bezeichnet. Dies muss vereinbart werden, bevor Assets oder Seiten in das System aufgenommen werden.

Ein Metadatenmodell ist in der Regel so aufgebaut, dass es die folgenden Anwendungsfälle erfüllt:

* Suchen und abrufen: Hilft beim Speichern wichtiger Inhaltsaspekte, die das einfache Abrufen nach Unternehmen erleichtern.
* Wiederverwendung: Hilft, alte Assets für die Wiederverwendung zu nutzen (Zeit und Geld sparen)
* Lizenzverwaltung: Verfolgen des Eigentums der Organisation an Vermögenswerten (häufig aus rechtlichen Gründen)
* Verteilen: Bereitstellung von Inhalten für Verbraucher oder Syndikation von Assets für Geschäftspartner.
* Archivieren: Metadaten, die ein Asset notieren, sind veraltet (immer Best Practice, ein &quot;archiviertes&quot;Flag für ein Asset zu platzieren, um wichtige Informationen nicht zu verlieren)/
* Querverweis: Verknüpfte Metadaten, die die Beziehung zwischen zwei oder mehr Assets erfassen (die Synthese von Metadaten ermöglicht Querverweise und eine kohärente Gruppenorganisation)
* Navigieren Sie: Die Ordnerstruktur, in der Assets gespeichert werden (zum Abrufen von Informationen durch Durchsuchen verwendet)

*Autorenmetadaten unterstützen hauptsächlich betriebliche Prozesse. Publish unterstützt Anwendungsfälle für Abruf und Verteilung.*

## Verwenden von Tags als vordefinierte Begriffe

Ein Tag ist ein Schlüsselwort oder ein Begriff, der einem Informationsstück zugeordnet ist. Anstatt beispielsweise &quot;Auto&quot;, &quot;Fahrzeug&quot;, &quot;Automobil&quot;einzugeben, erlaubt ein Tag-System nur einen Wert, aus dem Sie auswählen können, wodurch die Suche vorhersehbarer wird.  Tags normalisieren und vereinfachen die Kategorisierung von Assets.

*Hinweis: Obwohl AEM das Ad-hoc-Tagging ermöglicht, sollten Sie sich von diesem Verfahren fernhalten, da dies zu einer undefinierten und unleserlichen Taxonomie führen könnte.*

Häufige Verwendung von Tags:

* Suchbegriffsuche: Ein Tag kann beschreiben, dass eine Ressource zu einer bestimmten Gruppe von Entitäten gehört. Beispiel: Ein Tag &quot;image/subject/car&quot; beschreibt die Ressource, die zum Bildsatz gehört, der ein Auto anzeigt.
* Antriebsbeziehungen: Alle Ressourcen, die dasselbe Tag teilen, können als verbunden betrachtet werden. Das Tagging anstelle der direkten Verknüpfung ist besonders auf Websites mit vielen dynamischen und verbundenen Inhalten nützlich.
* Laufwerknavigation: In einer hierarchischen Taxonomie geordnete Tags können eine Navigation erstellen, eine Verknüpfung zu ähnlichen Dokumenten.
Tags sollten auch Informationen angezeigt werden, die verschiedene Datentypen miteinander verbinden, basierend auf Geschäftsbegriffen und nicht auf technischen Eigenschaften.

## Häufige Anwendungen für Tags

Bei Verwendung in AEM können Tags dazu beitragen, eine wesentlich kürzere Implementierung der komplexen Funktion zu erreichen, z. B.:

* Facettensuche
* Personalisierte Navigation
* Verwandte Inhalte
* Inhaltsreferenzen
* Suchmaschinenoptimierung
* Wichtige Konzepte hervorheben

## Taxonomien

Eine Taxonomie ist ein System der Organisation von Tags basierend auf gemeinsamen Merkmalen, die in der Regel hierarchisch nach organisatorischen Anforderungen strukturiert sind. Die Struktur kann dabei helfen, ein Tag schneller zu finden oder eine Generalisierung durchzusetzen.
Beispiel: Es besteht die Notwendigkeit, die Kategorisierung von Fahrzeugbildern vorzunehmen.  Die Taxonomie könnte wie folgt aussehen:

/subject/car/ /subject/car/sportscar/subject/car/sportscar/porsche/subject/car/sportscar/ferrari ... /subject/car/minivan /subject/car/minivan/mercedes /subject/car/minivan/volkswagen ... /subject/car/limousine ...

Jetzt kann ein Benutzer entscheiden, ob er Bilder von Sportwagen im Allgemeinen oder einen &quot;Porsche&quot; im Besonderen nachschlagen möchte. Schließlich sind beide Sportwagen.
Best Practice: Vermeiden Sie flache Taxonomien. Einfache Taxonomien verfügen nicht über die oben beschriebenen Vorteile und erfordern eine ständige Wartung

**Verwendung einer Taxonomie als Thesaurus.**  Wenn ein Benutzer nach einem Keyword sucht, erstellt das System eine zweite Suche für alle dort gefundenen Synonyme.
Zusätzlich kann das System, anstatt &quot;Auto&quot;manuell einzugeben, eine Liste mit Schlüsselwörtern bereitstellen, um die Konsistenz zu verbessern.

**Verwenden einer Taxonomie als Wörterbuch.** Anstatt nur &quot;Auto&quot;zu drucken, können Sie das einzelne Tag erweitern und alle Synonyme des Tags verwenden.

**Mehrere Kategorien.** Im Gegensatz zu einer Ordnerhierarchie können Tags verwendet werden, um mehrere Kategorien gleichzeitig auszudrücken. Ein Asset mit folgenden Tags:

/subject/car/minivan/mercedes /subject/people/family /color/red

## Metadaten vs. Tag

Nicht alle Metadaten sollten als Kandidaten für das Tagging-System betrachtet werden. Technische Metadaten können die Informationen unnötigerweise duplizieren. Der beste Kandidat für Tags sind Geschäftsmetadaten. .Tags sind eine gute Wahl, um konsistentes Vokabular, facettierte Suche und Navigation durchzusetzen.

## Tag-Management

Das Tag-Management profitiert von einem dedizierten Kernteam. Neue Mitglieder sollten zuerst den Zweck und die Funktion der Taxonomie lernen, bevor neue Tags hinzugefügt werden.  Erfahrene Experten, die als Gatekeeper für neue Tags agieren, werden die langfristige Inkonsistenz verringern.

## Tag-Erstellung

Taxonomien sollten von Inhaltsautoren verwendet und von Endbenutzern verstanden werden. Sie sollten vor der Inhaltserstellung erstellt werden. Jegliche Tastaturbefehle führen zu zusätzlichem Verwaltungs- und Wartungsaufwand.

## Laufende Wartung

Die Dinge ändern sich, und auch die Anforderungen der Tag-Liste. Erfahren Sie mehr über eine solide Wartung, die die Duplizierung reduziert.

Stellen Sie sicher, dass Inhaltsverantwortliche wissen, wie sie Änderungen vorschlagen können, und dass Bearbeiter oder Inhaltsmanager die Begriffe regelmäßig überprüfen.

## Best Practices mit Tags und Taxonomien

**Tags standardisieren.** Erstellen Sie ein Glossar, das ein aussagekräftiges Vokabular liefert. Ohne die Festlegung von Normen werden Doppelarbeit Probleme verursachen. Darüber hinaus wird empfohlen, nicht nur die Taxonomie, sondern auch die Verwendung der Tags zu prüfen.

**Taggen Sie nicht zu oft.** Tags können ihre Bedeutung verlieren, wenn sie zu häufig verteilt werden.Bereinigen Sie irrelevante Tags für eine optimale Effizienz.

**Neuauswertung von Tags im Zeitverlauf.** Denken Sie daran, dass die Terminologie und der Geschäftskontext des Unternehmens selten statisch bleiben. Möglicherweise müssen Sie Tags neu standardisieren und erneut anwenden.

**Verwenden von KI-gestütztem Smart-Tagging.** Smart-Tagging [Link anzeigen] ist eine KI-Funktion in AEM, um den Aufwand für das manuelle Tagging von Assets zu reduzieren. Smart-Tagging verwendet eine KI, um Informationen zum Betreff eines Bildes zu erhalten. Es werden beschreibende Tags generiert, die den Inhalt eines Bildes beschreiben.

## Qualität und Wartung der Metadaten

Das Verständnis der Geschäftsanforderungen ist ein wichtiger Schritt beim Ausführen eines Metadatenverwaltungsmodells. Ohne Definition können die Informationen nicht gespeichert werden. Es ist erforderlich, das Modell regelmäßig zu überprüfen. Dies ist eine wichtige Qualitätskontrolle.

Darüber hinaus sollten Metadaten so früh wie möglich im Inhaltserstellungsprozess erfasst werden. Wenn Metadaten nicht zum richtigen Zeitpunkt angewendet werden, besteht kaum eine Möglichkeit, sie rückwirkend anzuwenden.

**Metadaten verwenden** Verbesserung der Zusammenarbeit: Nutzen Sie Adobe Asset Link, Adobe Bridge und das AEM Desktop, um kreative Prozesse miteinander zu verbinden und Metadaten zu nutzen, um kreative Workflows zu optimieren. Die Verwendung dieser Tools bereichert Metadaten und das Benutzererlebnis in Ihrem kreativen Prozess.

## Best Practices für die Metadatenverwaltung

* Weisen Sie das Kernteam mit einem starken Exekutivmandat zu: ein Metadaten-Core-Team zu bilden, das über umfassende Kenntnisse des Business Ökosystems und ein starkes Mandat der Unternehmensleitung verfügt.
* Definieren Sie Metadatenstrategie und -verwaltung: Eine gute Metadatenstrategie kann Unternehmen dabei helfen, die Notwendigkeit und Vorteile der Metadaten zu erklären.  Eine Strategie besteht aus Metadatenschema(en), Taxonomie, Geschäftsprozessen (für Datenqualität und -erfassung), Rollen und Zuständigkeiten sowie Governance-Prozessen. *
* Konsistentes Metadatenmodell definieren und kommunizieren: Definierte Strategien und Überlegungen sollten gut dokumentiert und innerhalb der Organisationen kommuniziert werden.
* Standardbenennungsregel: Erstellen Sie eine konsistente und beschreibende Dateibenennungskonvention, um das Branding, die Informationsverwaltung und die Benutzerfreundlichkeit zu verbessern.
* Sichere Zeichen in Dateinamen: Dateiname sollte von allen gängigen Betriebssystemen interpretiert werden können. Sie können sicher Zeichen, Zahlen, Umlaute, Leerzeichen und Unterstriche verwenden. Das Minuszeichen ist ebenfalls sicher, aber wenn Sie ausschneiden und einfügen, kann es wie ein &quot;Bindestrich&quot;aussehen.
* Versionsbenennungskonvention: AEM bietet einige Funktionen zum Beibehalten früherer Asset-Versionen. In einigen Fällen möchten Sie möglicherweise mehrere Versionen beibehalten. Sie sollten jedoch sicherstellen, dass das Versionierungsschema konsistent ist.

## Organisatorische und beschreibende Metadaten

Einige Richtlinien helfen Ihnen bei der Entscheidung, wie Metadaten kategorisiert werden:

**Beschreibung** - Wenn die Daten das Asset oder den Inhalt beschreiben, sollten sie Teil der angehängten Metadaten sein.

**Suche** - Wenn die Metadaten bei der Suche verwendet werden sollen, müssen sie angehängt werden.

**Belichtung** - Wenn Sie die Metadaten auf einer Verteilungsplattform einem Drittanbieter zur Verfügung stellen, achten Sie darauf, nicht auch &quot;interne&quot;Metadaten verfügbar zu machen.

**Dauer** - Je länger die Metadaten leben sollen, desto wahrscheinlicher ist es, ein guter Kandidat für angehängte Metadaten zu sein.

**Verwandte Geschäftsprozesse** - Es ist auf jeden Fall hilfreich, eine permanente Produkt-ID als Teil der Metadaten zu haben. Die Kategorie eines Elements im Verhältnis zum Produktkatalog stellt jedoch zweifelhafte Metadaten für das Asset dar.

**Organisation und Verarbeitung** - Wenn die Metadaten organisatorisch sind, z. B. Status in einem Genehmigungs-Workflow oder Eigentümer einer bestimmten Abteilung, sollten externe Metadaten über dem Anhängen der Metadaten an das Asset betrachtet werden.

*Stellen Sie die folgenden Fragen, um die Strategie zu erstellen:*

* Welche Art von Inhalten und &quot;zusätzlichen Informationen&quot; (= Metadaten) sind erforderlich, um Geschäftsprobleme/Geschäftsfragen/Geschäftsprobleme zu lösen?
* Was sind die Variablen, die &quot;Felder&quot;im Schema und welche möglichen Werte sind möglich? Welche Variablen benötigen eine freie Texteingabe, die nach Typ (Zahl, Datum, boolesch, ...), einer Reihe fester Werte (z. B. Länder) oder Tags aus einer bestimmten Taxonomie eingegrenzt werden kann. Wie viele Tags sind erforderlich, erlaubt?
* Welche technischen Probleme/Probleme/Fragen können durch Metadaten gelöst werden?
* Wie können Sie diese Inhalte/Metadaten abrufen/erstellen? Wie viel kostet das Abrufen/Erstellen dieser Metadaten?
* Welche Metadatentypen sind für eine bestimmte Benutzergruppe erforderlich?
* Wie werden Metadaten gepflegt und aktualisiert?
* Wer ist für welchen Teil des Prozesses verantwortlich?
* Wie können Sie sicherstellen, dass vereinbarte Geschäftsprozesse eingehalten werden?
* Welche Standards sollten Sie einhalten? Sollten Sie einen Branchenstandard übernehmen und ändern (Dublin Core, ISO 19115, PRISM usw.) oder sollte die Organisation eigene Standards erstellen?
* Wo ist die Strategie dokumentiert? Wie können Sie sicherstellen, dass alle Interessengruppen Zugriff haben? Wie können Sie sicherstellen, dass neu integrierte Mitarbeiter den vereinbarten Standard einhalten (z. B. besuchen Sie Schulungen, bevor Sie Zugang erhalten)?

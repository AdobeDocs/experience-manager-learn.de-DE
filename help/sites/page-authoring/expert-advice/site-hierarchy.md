---
title: Handbuch zu Site-Hierarchie, Taxonomie und Tagging
description: Eine vollständige Übersicht über AEM Sites-Metadaten, Tagging, Taxonomie und Hierarchie. Verwenden Sie dieses Handbuch, um sicherzustellen, dass Ihre Inhaltsstrategie konsistent ist und Sie die Best Practices befolgen
topic: Content Management
feature: Learn From Your Peers
role: Admin, User
jira: KT-14254
level: Beginner, Intermediate
doc-type: Article
exl-id: c88c3ec7-9060-43e2-a6a2-d47bba6f7cf3
duration: 535
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '2035'
ht-degree: 100%

---

# Best Practices für Tags, Taxonomie und Metadaten: Allgemeine Zusammenfassung

Metadaten und Tags sind entscheidend für die Steigerung der Effizienz in AEM. Benutzende, Führungskräfte und Management erkennen oft die Notwendigkeit einer ganzheitlichen Strategie, aber sie finden es schwierig, Fortschritte zu erzielen. Oft ist das Wissen bei den Benutzenden isoliert, was eine ganzheitliche Strategie erschwert – und die Anpassung noch problematischer macht.

Was ist der Unterschied zwischen Metadaten und Tags? Welche geschäftlichen Aspekte sollten bei der Umsetzung Ihrer Strategie berücksichtigt werden?

## Welchen Zweck haben Metadaten?

Metadaten fügen Struktur zu weniger strukturierten Inhalten hinzu.
Beispiel: Ein Grundbild hat Pixel. Wir können diese „Kerndaten“ nennen. Es sind die Metadaten, die das Format, die Kategorie, Lizenzdetails usw. beschreiben.
Metadaten werden am häufigsten für Assets verwendet. Es gibt jedoch auch eine große Anzahl von Anwendungsfällen für Metadaten in Inhaltsseiten oder Experience Fragments.

## Quellen für Metadaten

Im Folgenden finden Sie die Kategorien, für die Metadaten generiert werden können:

* Extrahierte Metadaten: Die Informationen sind bereits im Dokument verfügbar, z. B. in natürlicher Sprache.
* Abgeleitete Metadaten: Die Informationen sind nicht in den Originaldaten verfügbar, können jedoch durch Querverweise auf frühere Kenntnisse abgeleitet werden.
* Manuell hinzugefügte Metadaten: Dies sind Metadaten, die nicht in eine der ersten Kategorien fallen und von einem Menschen manuell hinzugefügt werden müssen.

## Metadatentypen

Innerhalb der oben aufgeführten Kategorien gibt es vier Haupttypen:

* Technische und beschreibende Metadaten: Bieten Informationen zu technischen Details des Inhalts (d. h. Titel, Sprache usw.)
* Operative Metadaten: Dokumentieren den Lebenszyklus eines Assets (d. h. genehmigt, kreativ, Kampagne).
* Administrative Metadaten: Status oder Zustand eines Assets innerhalb einer Organisation (d. h. Lizenzinformationen, Eigentumsverhältnisse)
* Strukturelle Metadaten: Helfen bei der Kategorisierung von Assets oder Seiten für einen reibungslosen Geschäftsprozess (gilt für die meisten Tags und Taxonomien)

## Ordner- und Dateinamen

Ordner sind eine natürliche Möglichkeit zum Navigieren und Durchsuchen der Inhalte in AEM. Wie werden Ihre Stakeholder mit AEM interagieren? Dadurch wird bestimmt, wie Ihre Ordner strukturiert sind. Normalerweise wird die Ordnerstruktur mit einer (oder zwei) der folgenden Einstellungen erstellt:

* Navigation
* Browsen
* Kategorisierung
* Zugriffssteuerung

Bei AEM Sites ist die Navigation wichtig. Ordner werden verwendet, um den Zugriff auf Assets und Seiten zu steuern.

Welche Autorenebenen benötigen Zugriff auf Homepages? Was ist mit Produktseiten? Oder Kampagnen? Verwenden Sie Berechtigungen und die Ordnerstruktur, um die richtige Governance festzulegen.

## Speichern von Metadaten

Es gibt drei Methoden zum Speichern von Metadaten:

* Binär: Binärformat, das mit der Art des Assets (Photoshop, InDesign, PNG, JPG) in Beziehung steht.
* Asset-Knoten: Dies sind Metadaten für das Asset selbst, unabhängig vom verwendeten System oder Prozess.
* Externer Speicherort: Metadaten, die sich nicht direkt auf dem Asset befinden, aber als Deskriptor für den „Status“ eines Assets verwendet werden können (Beispiel: ein Workflow, der sich auf ein Asset auswirken, aber nicht direkt darauf angewendet werden kann)

## Metadatenmodell

Die Struktur der Erfassung und Formatierung von Metadaten wird als Metadatenmodell oder Metadatenschema bezeichnet. Darüber muss Einigkeit herrschen, bevor Assets oder Seiten in das System aufgenommen werden.

Ein Metadatenmodell ist in der Regel so aufgebaut, dass es die folgenden Anwendungsfälle erfüllt:

* Suche und Abruf: Hilft bei der Speicherung wichtiger Inhaltsaspekte, die den einfachen Abruf durch Unternehmen erleichtern
* Wiederverwendung: Hilft, alte Assets für die Wiederverwendung zu verwenden (spart Zeit und Geld)
* Lizenzverwaltung: Verfolgt das Eigentum der Organisation am Asset (häufig aus rechtlichen Gründen)
* Verteilung: Bereitstellen von Inhalten für Verbraucherinnen und Verbraucher oder Synchronisieren von Assets für Geschäftspartner
* Archivierung: Metadaten, die darauf hinweisen, dass ein Asset veraltet ist (es ist immer eine Best Practice, ein Asset als „archiviert“ zu kennzeichnen, um keine wichtigen Informationen zu verlieren)
* Querverweis: Verknüpfte Metadaten, die die Beziehung zwischen zwei oder mehr Assets erfassen (die Synthese von Metadaten ermöglicht Querverweise und eine einheitliche Gruppenorganisation)
* Navigation: Die Ordnerstruktur, in der Assets gespeichert sind (zum Abrufen von Informationen durch Blättern)

*Authoring-Metadaten unterstützen hauptsächlich betriebliche Prozesse. Veröffentlichungsmetadaten unterstützen Anwendungsfälle für Abruf und Verteilung.*

## Verwenden von Tags als vordefinierte Begriffe

Ein Tag ist ein Schlüsselwort oder ein Begriff, der einem Informationsstück zugeordnet ist. Anstatt beispielsweise „Auto“, „Fahrzeug“, „Automobil“ einzugeben, erlaubt ein Tag-System nur einen Wert, aus dem Sie auswählen können, wodurch die Suche vorhersehbarer wird. Tags normalisieren und vereinfachen die Kategorisierung von Assets.

*Hinweis: Obwohl AEM das Ad-hoc-Tagging ermöglicht, besteht die Best Practise darin, dies nicht zu tun, da dies zu einer undefinierten und unhandlichen Taxonomie führen könnte.*

Häufige Verwendung von Tags:

* Keyword-Suche: Ein Tag kann beschreiben, dass eine Ressource zu einer bestimmten Gruppe von Entitäten gehört. Beispiel: Ein Tag „image/subject/car“ beschreibt, dass die Ressource zum Bildsatz gehört, der ein Auto anzeigt.
* Herstellung von Beziehungen: Alle Ressourcen, die dasselbe Tag haben, können als verbunden betrachtet werden. Das Tagging anstelle der direkten Verknüpfung ist besonders auf Websites mit vielen dynamischen und verbundenen Inhalten nützlich.
* Steuerung der Navigation: Tags, die in einer hierarchischen Taxonomie angeordnet sind, können eine Navigation oder einen Link zu ähnlichen Dokumenten bilden.
Tags sollten auch als Informationen angesehen werden, die verschiedene Datentypen miteinander verbinden, und zwar auf der Grundlage von Geschäftsbegriffen und nicht von technischen Eigenschaften.

## Häufige Anwendungen von Tags

Bei Verwendung in AEM können Tags dazu beitragen, eine wesentlich kürzere Implementierung einer komplexen Funktion zu erreichen, z. B.:

* Facettensuche
* Personalisierte Navigation
* Verwandte Inhalte
* Inhaltsreferenzen
* Suchmaschinenoptimierung
* Hervorheben von wichtigen Konzepten

## Taxonomien

Eine Taxonomie ist ein System der Organisation von Tags basierend auf gemeinsamen Merkmalen, die in der Regel hierarchisch nach organisatorischen Anforderungen strukturiert sind. Die Struktur kann dabei helfen, ein Tag schneller zu finden oder eine Generalisierung durchzusetzen.
Beispiel: Es besteht die Notwendigkeit, die Kategorisierung von Stock-Fahrzeugbildern vorzunehmen. Die Taxonomie könnte wie folgt aussehen:

/subjekt/auto/
/subjekt/car/sportscar
/subjekt/auto/sportwagen/porsche
/subjekt/auto/sportwagen/ferrari
…
/subjekt/auto/minivan
/subjekt/auto/minivan/mercedes
/subjekt/auto/minivan/volkswagen
…
/subjekt/auto/limousine
…

Jetzt können Benutzende entscheiden, ob sie Bilder von Sportwagen im Allgemeinen oder einen „Porsche“ im Besonderen nachschlagen möchten. Schließlich sind beide Sportwagen.
Best Practice: Vermeiden Sie flache Taxonomien. Flache Taxonomien verfügen nicht über die oben beschriebenen Vorteile und erfordern eine ständige Wartung

**Verwendung einer Taxonomie als Thesaurus.** Wenn eine Benutzerin bzw. ein Benutzer nach einem Keyword sucht, erstellt das System eine zweite Suche für alle dort gefundenen Synonyme.
Anstatt „Auto“ manuell einzugeben, kann das System eine Liste von Schlüsselwörtern bereitstellen, um die Konsistenz zu verbessern.

**Verwenden einer Taxonomie als Wörterbuch.** Anstatt nur „Auto“ zu drucken, können Sie das einzelne Tag erweitern und alle Synonyme des Tags verwenden.

**Mehrere Kategorien.** Im Gegensatz zu einer Ordnerhierarchie können Tags verwendet werden, um mehrere Kategorien gleichzeitig auszudrücken. Ein Asset mit folgenden Tags:

/subjekt/auto/minivan/mercedes
/subjekt/leute/family
/farbe/fot

## Metadaten vs. Tags

Nicht alle Metadaten sollten als Kandidaten für das Tagging-System betrachtet werden. Technische Metadaten können die Informationen unnötigerweise duplizieren. Der beste Kandidat für Tags sind Geschäftsmetadaten. Tags sind eine gute Wahl, um konsistentes Vokabular, Facettensuche und Navigation durchzusetzen.

## Tag-Management

Das Tag-Management profitiert von einem dedizierten Kern-Team. Neue Mitglieder sollten sich zunächst mit Zweck und Funktion der Taxonomie vertraut machen, bevor sie neue Tags hinzufügen. Erfahrene Fachkräfte, die als Gatekeeper für neue Tags agieren, werden die Inkonsistenz langfristig verringern.

## Tag-Erstellung

Die Taxonomien sollten von Inhaltsautorinnen und Inhaltsautoren verwendet und von Endbenutzenden verstanden werden. Sie sollten vor der Inhaltserstellung erstellt werden. Jegliche Abkürzungen führen hier zu zusätzlichem Verwaltungs- und Wartungsaufwand.

## Laufende Wartung

Die Dinge ändern sich, und ebenso die Anforderungen der Tag-Liste. Entwickeln Sie einen soliden Wartungsprozess, der die Duplizierung reduziert.

Stellen Sie sicher, dass Menschen, die Inhalte beisteuern, wissen, wie sie Änderungen vorschlagen können, und dass Redakteurinnen, Redakteure oder Content-Manager die Begriffe regelmäßig überprüfen.

## Best Practices für Tags und Taxonomien

**Standardisieren Sie Tags.** Erstellen Sie ein Glossar, das ein aussagekräftiges Vokabular liefert. Ohne die Festlegung von Standards wird Duplizieren zu Problemen führen. Darüber hinaus wird empfohlen, nicht nur die Taxonomie, sondern auch die Verwendung der Tags zu prüfen.

**Taggen Sie nicht zu viel.** Tags können ihre Bedeutung verlieren, wenn sie zu großzügig verteilt werden.Bereinigen Sie irrelevante Tags für eine optimale Effizienz.

**Neuauswertung von Tags im Zeitverlauf.** Denken Sie daran, dass die Terminologie und der Geschäftskontext des Unternehmens selten statisch bleiben. Möglicherweise müssen Sie Tags neu standardisieren und erneut anwenden.

**Verwenden von KI-gestütztem Smart-Tagging.** Smart-Tagging [(siehe Link)] ist eine KI-Funktion in AEM, um den Aufwand für das manuelle Tagging von Assets zu reduzieren. Smart-Tagging verwendet eine KI, um Informationen zum Motiv eines Bildes zu erhalten. Es werden beschreibende Tags generiert, die den Inhalt eines Bildes beschreiben.

## Qualität und Wartung der Metadaten

Das Verständnis der Geschäftsanforderungen ist ein wichtiger Schritt beim Ausführen eines Metadatenverwaltungsmodells. Ohne Definition können die Informationen nicht gespeichert werden. Es ist erforderlich, das Modell regelmäßig zu überprüfen. Dies ist eine wichtige Qualitätskontrolle.

Darüber hinaus sollten Metadaten so früh wie möglich im Inhaltserstellungsprozess erfasst werden. Wenn Metadaten nicht zum richtigen Zeitpunkt angewendet werden, besteht kaum eine Möglichkeit, sie rückwirkend anzuwenden.

**Verwenden von Metadaten** zur Verbesserung der Zusammenarbeit: Nutzen Sie Adobe Asset Link, Adobe Bridge und AEM Desktop, um kreative Prozesse miteinander zu verbinden und Metadaten zu nutzen, um kreative Workflows zu optimieren. Die Verwendung dieser Tools bereichert die Metadaten und das Anwendererlebnis in Ihrem kreativen Prozess.

## Best Practices für die Verwaltung von Metadaten

* Beauftragen eines Kern-Teams mit einem starken Mandat der Geschäftsleitung: Bilden Sie ein Kern-Team für Metadaten, das über ein umfassendes Verständnis des Unternehmens-Ökosystems und ein starkes Mandat der Unternehmensleitung verfügt.
* Definieren der Metadatenstrategie und -verwaltung: Eine gute Metadatenstrategie kann Unternehmen dabei helfen, die Notwendigkeit und die Vorteile der Metadaten zu erklären. Eine Strategie besteht aus Metadatenschemata, Taxonomie, Geschäftsprozessen (für Datenqualität und -erfassung), Rollen und Zuständigkeiten sowie Governance-Prozessen. *
* Definition und Kommunikation eines konsistenten Metadatenmodells: Die definierte Strategie und die Überlegungen dazu sollten gut dokumentiert und innerhalb der Organisation kommuniziert werden.
* Standard-Namenskonvention: Erstellen Sie eine konsistente und beschreibende Dateinamenskonvention für ein verbessertes Branding, Informations-Management und Benutzerfreundlichkeit.
* Sichere Zeichen in Dateinamen: Ein Dateiname sollte von allen gängigen Betriebssystemen interpretiert werden können. Die Verwendung von Buchstaben, Zahlen, Umlauten, Leerzeichen und Unterstrichen ist sicher. Das Minuszeichen ist zwar ebenfalls sicher, aber wenn Sie es ausschneiden und einfügen, könnte es wie ein „Gedankenstrich“ aussehen.
* Versions-Namenskonvention: AEM bietet einige Funktionen zum Beibehalten früherer Versionen von Assets. In einigen Fällen möchten Sie möglicherweise mehrere Versionen beibehalten. Sie sollten jedoch sicherstellen, dass das Versionierungsschema konsistent ist.

## Organisatorische vs. beschreibende Metadaten

Einige Richtlinien können Ihnen bei der Entscheidung helfen, wie Metadaten kategorisiert werden sollen:

**Beschreibung**: Wenn die Daten das Asset oder den Inhalt beschreiben, sollten sie Teil der angehängten Metadaten sein.

**Suche**: Wenn die Metadaten bei der Suche verwendet werden sollen, müssen sie angehängt werden.

**Belichtung**: Wenn Sie die Metadaten auf einer Verteilungsplattform einem Drittanbieter zur Verfügung stellen, achten Sie darauf, nicht auch „interne“ Metadaten verfügbar zu machen.

**Dauer**: Je länger die Metadaten leben sollen, desto wahrscheinlicher ist es, dass sie ein guter Kandidat für angehängte Metadaten sind.

**Verwandte Geschäftsprozesse**: Es ist auf jeden Fall hilfreich, eine permanente Produkt-ID als Teil der Metadaten zu haben. Die Kategorie eines Elements im Verhältnis zum Produktkatalog stellt jedoch zweifelhafte Metadaten für das Asset dar.

**Organisation und Verarbeitung**: Wenn die Art der Metadaten organisatorischer Natur ist, wie z. B. der Status in einem Genehmigungs-Workflow oder der Besitz einer bestimmten Abteilung, sollten externe Metadaten über das Anhängen der Metadaten an das Asset in Betracht gezogen werden.

*Um die Strategie zu erstellen, stellen Sie die folgenden Fragen:*

* Welche Art von Inhalten und „zusätzlichen Informationen“ (= Metadaten) sind erforderlich, um Geschäftsprobleme/Geschäftsfragen/Geschäftsangelegenheiten zu lösen?
* Was sind die Variablen, die „Felder“ im Schema, und was sind mögliche Werte? Welche Variablen benötigen eine Freitexteingabe, welche können durch einen Typ (Zahl, Datum, boolesche Werte, …), einen Satz fester Werte (z. B. Länder) oder Tags aus einer vorgegebenen Taxonomie eingegrenzt werden? Wie viele Tags sind erforderlich/erlaubt?
* Welche technischen Themen/Probleme/Fragen können durch Metadaten gelöst werden?
* Wie können Sie diese Inhalte/Metadaten abrufen/erstellen? Wie viel kostet das Abrufen/Erstellen dieser Metadaten?
* Welche Metadatentypen sind für eine bestimmte Benutzergruppe erforderlich?
* Wie werden Metadaten gepflegt und aktualisiert?
* Wer ist für welchen Teil des Prozesses verantwortlich?
* Wie können Sie sicherstellen, dass vereinbarte Geschäftsprozesse eingehalten werden?
* Welche Standards sollten Sie einhalten? Sollten Sie einen Branchenstandard übernehmen und ändern (Dublin Core, ISO 19115, PRISM usw.) oder sollte die Organisation eigene Standards erstellen?
* Wo ist die Strategie dokumentiert? Wie können Sie sicherstellen, dass alle Stakeholder Zugriff haben? Wie können Sie sicherstellen, dass neu integrierte Mitarbeitende den vereinbarten Standard einhalten (z. B. Besuch einer Schulung, bevor sie Zugang erhalten)?

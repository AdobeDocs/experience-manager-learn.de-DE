---
title: Beschleunigen der Geschwindigkeit von Inhalten bei AEM-Stilsystemen
description: Erfahren Sie, wie Sie mit AEM-Stilsystemen Designerinnen und Designer, Inhaltsautorinnen und Inhaltsautoren sowie Entwicklerinnen und Entwickler in Ihrem Unternehmen in die Lage versetzen können, Erlebnisse mit der Geschwindigkeit und Skalierung zu erstellen und bereitzustellen, die Ihre Kundinnen und Kunden erwarten.
solution: Experience Manager
exl-id: 449cd133-6ab6-456e-a0ad-30e3dea9b75b
duration: 230
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '817'
ht-degree: 100%

---

# Beschleunigen der Geschwindigkeit von Inhalten bei AEM-Stilsystemen

In diesem Artikel erfahren Sie, wie Sie mit AEM-Stilsystemen Designerinnen und Designern, Inhaltsautorinnen und Inhaltsautoren sowie Entwicklerinnen und Entwicklern in Ihrem Unternehmen in die Lage versetzen, Erlebnisse mit der Geschwindigkeit und Skalierung zu erstellen und bereitzustellen, die von Kundenseite her erwartet werden.

## Übersicht

AEM-Stilsysteme bieten vier wesentliche Vorteile:

* Vorlagenautorinnen und -autoren können in der Inhaltsrichtlinie einer Komponente oder Seite Stilklassen definieren.
* Inhaltsautorinnen und -autoren können Stile auswählen, die auf eine ganze Seite oder beim Bearbeiten einer Komponente auf einer Seite angewendet werden sollen.
* Komponenten und Vorlagen werden flexibler, indem Autorinnen und Autoren die Möglichkeit erhalten, alternative visuelle Varianten zu rendern.
* Die Notwendigkeit, eine benutzerdefinierte Komponente und/oder komplexe Dialogfelder zu entwickeln, um Komponentenvarianten zu präsentieren, wird reduziert oder vollständig eliminiert.

## Ersteinrichtung und Verwendung

Die fünf Schritte umfassende Einrichtung ähnelt einem standardmäßigen Workflow für die Komponentenentwicklung.

| **Führung** | **Design** | **Entwicklung/Architektur** | **Vorlagenerstellung** | **Inhaltserstellung** |
| --- | --- | --- | --- | --- |
| Bestimmt Inhalt und Ziele für diese Komponente | Bestimmt die visuelle und erlebnisorientierte Darstellung von Inhalten | Entwickelt CSS und JS zur Unterstützung von Erlebnissen, definiert zu verwendende Klassennamen und stellt diese bereit | Konfiguriert Vorlagenrichtlinien für formatierte Komponenten, indem vom Entwickler-Team definierte CSS-Klassennamen hinzugefügt werden. Benutzerfreundliche Namen sollten für jeden Stil verwendet werden. | Wendet beim Erstellen von Seiten Stile nach Bedarf an, um das gewünschte Look-and-Feel zu erzielen |

Hierbei handelt es sich zwar um die Ersteinrichtung, dennoch haben viele unserer Kundinnen und Kunden durch die Optimierung dieses Prozesses zusätzliche Agilität erreicht, z. B. durch Hochladen ihrer CSS in DAM, wodurch Stilaktualisierungen ohne Bereitstellung möglich sind. Andere Kundinnen und Kunden verfügen über einen vollständig ausgestatteten Satz von Dienstprogrammklassen, mit denen sie Komponenten und Stile entwickeln können, die dann ohne Bereitstellung oder Entwicklung genutzt werden können.

Stilsysteme sind in verschiedenen Varianten erhältlich:

1. Layout-Stile

   * Vielseitige Änderungen am Design und Layout

   * Verwendet für klar definierte und identifizierbare Ausgabedarstellungen

1. Anzeigestile
   * Geringfügige Varianten, die die grundlegende Natur des Stils nicht ändern

   * Beispielsweise die Änderung des Farbschemas, der Schrift, der Bildausrichtung usw.

1. Informative Stile

   * Ein-/Ausblenden von Feldern

>[!NOTE]
>
>Für eine Demo dieser Funktionen empfehlen wir, sich unser [Customer Success-Webinar](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) mit Will Brisbane und Joseph Van Buskirk anzusehen.

## Best Practices

* Standardstil zuerst festigen
   * Layout und Anzeige der Komponente, wenn sie vor der Anwendung von Stilsystemen auf der Seite eingefügt wird
   * Dies sollte die am häufigsten verwendete Ausgabedarstellung sein
* Möglichst nur Stiloptionen anzeigen, die einen Effekt haben
   * Wenn ineffektive Kombinationen verfügbar sind, stellen Sie sicher, dass sie keine negativen Auswirkungen haben
   * Beispiel: ein Layoutstil, der die Bildposition bestimmt und von einem ineffektiven Anzeigestil begleitet wird, der die Bildposition steuert
* Layout-Stile gegenüber kombinierten Anzeigestilen vorziehen
   * Reduziert die Anzahl der Permutationen, die qualitativ überprüft werden müssen
   * Stellt die Einhaltung von Markenstandards sicher
   * Vereinfacht das Authoring für Inhaltsautorinnen und -autoren
   * Hilft bei der Erstellung einer konsistenten Markenidentität auf der Website
* Kombinierte Stile zurückhaltend einsetzen
   * Sowohl kategorieübergreifend als auch innerhalb von Kategorien
* Genügend Zeit für gründliches Testen der kombinierten Stile einräumen
   * Hilft bei der Vermeidung unerwünschter Effekte.
* Anzahl der Stiloptionen und Permutationen minimieren
   * Zu viele Optionen können zu einer mangelnden Markenkonsistenz in Bezug auf das Look-and-Feel führen
   * Kann Inhaltsautorinnen und -autoren verunsichern, welche Kombinationen für den gewünschten Effekt erforderlich sind
   * Erhöht die Permutationen, die auf Qualität überprüft werden müssen.
* Für Business-Anwender freundliche Stilkennzeichnungen und -kategorien verwenden
   * „Blau“ und „Rot“ anstelle von „Primär“ und „Sekundär“
   * „Karte“ und „Held“ anstelle von „Variante A“ und „Variante B“
   * Für bestimmte Kundinnen und Kunden ist dies womöglich ganz normal: das Design-Team, das Business-Team und das Content-Team sind mit ihren primären und sekundären Farben oder den Varianten, die sie testen, sehr vertraut. Aus Gründen der Flexibilität und des Potenzials für zukünftige Veränderungen kann die Verwendung bestimmter Begriffe jedoch effizienter sein.

## Haupterkenntnisse

Stilsysteme reduzieren den Bedarf an komplexen Dialogfeldern, sind jedoch kein Ersatz dafür. Sie helfen dabei, Dinge zu vereinfachen. In bestimmten Fällen möchten Sie aber vielleicht lieber Komponenteneigenschaften oder Dialogfelder verwenden, statt ein Stilsystem dafür zu erstellen.

Stilsysteme können Prozesse aus Entwicklungsperspektive optimieren. Mit einem Stilsystem können Sie mehrere Erscheinungsformen desselben Inhalts erreichen. Ebenso können Sie aus Authoring-Sicht die Authoring-Geschwindigkeit beschleunigen, anstatt Autorinnen und Autoren schulen zu müssen und von ihnen zu verlangen, sich daran zu erinnern, welche Komponente wo verwendet werden soll.

Alles ist einfach übersichtlicher. Der HTML-Code innerhalb der Kernkomponenten ist sehr ausführlich. Wenn Sie all dies auf CSS-Ebene durchführen, wird die Komponente schneller erstellt und der Code ist zudem klarer.

Schließlich ist der Einsatz von Stilsystemen mehr Kunst als Wissenschaft. Wie bereits erwähnt, gibt es eine Reihe von Best Practices, aber Sie können das Setup für Ihr Unternehmen flexibel anpassen.

Weitere Informationen finden Sie in unserem [Customer Success-Webinar](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) mit Will Brisbane und Joseph Van Buskirk.

Weitere Informationen über Strategie und Meinungsführerschaft finden Sie im [Customer Success](https://experienceleague.adobe.com/docs/customer-success/customer-success/overview.html?lang=de)-Hub.

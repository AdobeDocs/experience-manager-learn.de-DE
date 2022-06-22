---
title: Beschleunigen der Inhaltsgeschwindigkeit mit AEM Stilsystemen
description: Erfahren Sie, wie Sie mit AEM Stilsystemen Designer, Inhaltsautoren und Entwickler in Ihrem Unternehmen in die Lage versetzen können, Erlebnisse mit der Geschwindigkeit und Skalierung zu erstellen und bereitzustellen, die Ihre Kunden erwarten.
solution: Experience Manager
exl-id: 449cd133-6ab6-456e-a0ad-30e3dea9b75b
source-git-commit: 89982f506a5e1ffc12f84a0f616aaa1dc2e00c5b
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 0%

---

# Beschleunigen der Inhaltsgeschwindigkeit mit AEM Stilsystemen

In diesem Artikel erfahren Sie, wie Sie mit AEM-Systemen Designer, Inhaltsautoren und Entwickler in Ihrem Unternehmen in die Lage versetzen, Erlebnisse so schnell und skaliert zu erstellen und bereitzustellen, wie es Ihre Kunden erwarten.

## Übersicht

AEM Stilsysteme bieten vier wesentliche Vorteile:

* Vorlagenautoren können in der Inhaltsrichtlinie einer Komponente oder Seite Stilklassen definieren
* Inhaltsautoren können Stile auswählen, die auf eine ganze Seite oder beim Bearbeiten einer Komponente auf einer Seite angewendet werden sollen
* Komponenten und Vorlagen werden flexibler gestaltet, indem Autoren die Möglichkeit erhalten, alternative visuelle Varianten zu rendern
* Die Notwendigkeit, eine benutzerdefinierte Komponente und/oder komplexe Dialogfelder zu entwickeln, um Komponentenvarianten zu präsentieren, wird reduziert oder vollständig eliminiert

## Ersteinrichtung und Verwendung

Die Einrichtung aus fünf Schritten ähnelt einem standardmäßigen Arbeitsablauf für die Komponentenentwicklung.

| **Führung** | **Designer** | **Entwickler/Architekt** | **Vorlagenautor** | **Inhaltsautor** |
| --- | --- | --- | --- | --- |
| Bestimmt Inhalt und Ziele für diese Komponente | Bestimmt die visuelle und experimentelle Darstellung von Inhalten | Entwickelt CSS und JS zur Unterstützung von Erlebnissen; definiert und stellt Klassennamen bereit, die verwendet werden sollen | Konfiguriert Vorlagenrichtlinien für formatierte Komponenten, indem von Entwicklern definierte CSS-Klassennamen hinzugefügt werden. Benutzerfreundliche Namen sollten für jeden Stil verwendet werden. | Wendet beim Erstellen von Seiten die Stile nach Bedarf an, um das gewünschte Erscheinungsbild zu erzielen |

Während dies die Ersteinrichtung ist, haben viele unserer Kunden durch die Optimierung dieses Prozesses zusätzliche Agilität erreicht, z. B. durch Hochladen ihrer CSS in DAM, wodurch Stilaktualisierungen ohne Bereitstellung möglich sind. Andere Kunden verfügen über einen voll funktionsfähigen Satz von Dienstprogrammklassen, mit denen sie Komponenten und Stile entwickeln können, die dann ohne Bereitstellung oder Entwicklung genutzt werden können.

Stilsysteme sind in verschiedenen Varianten erhältlich:

1. Layoutstile

   * Vielseitige Änderungen am Design und Layout

   * Wird für klar definierte und identifizierbare Ausgabedarstellungen verwendet

1. Anzeigestile
   * Geringfügige Varianten, die die grundlegende Natur des Stils nicht ändern

   * Ändern Sie beispielsweise das Farbschema, die Schriftart, die Bildausrichtung usw.

1. Informative Stile

   * Felder ein-/ausblenden

>[!NOTE]
>
>Für eine Demo dieser Funktionen empfehlen wir, sich unsere [Webinar zum Kundenerfolg](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) mit Will Brisbane und Joseph Van Buskirk.

## Best Practices

* Standardstil zuerst festigen
   * Layout und Anzeige der Komponente, wenn sie vor der Anwendung von Stilsystemen auf der Seite abgelegt wird
   * Dies sollte die am häufigsten verwendete Ausgabedarstellung sein.
* Versuchen Sie, nur Stiloptionen anzuzeigen, die einen Effekt haben, wenn möglich
   * Wenn ineffektive Kombinationen verfügbar sind, stellen Sie sicher, dass sie keine negativen Auswirkungen verursachen
   * Beispiel: ein Layoutstil, der die Bildposition bestimmt und von einem ineffektiven Anzeigestil begleitet wird, der die Bildposition steuert
* Auswahl für Layoutstile gegenüber kombinierten Anzeigestilen
   * Reduziert die Anzahl der Permutationen, die qualitativ geprüft werden müssen
   * Sicherstellung der Einhaltung von Markenstandards
   * Vereinfachtes Authoring für Inhaltsautoren
   * Hilft bei der Erstellung einer konsistenten Markenidentität der Website
* Seien Sie vorsichtig mit kombinierten Stilen
   * Sowohl innerhalb als auch innerhalb von Kategorien
* Weisen Sie die richtige Zeit zu, um kombinierte Stile gründlich zu testen.
   * Hilft bei der Vermeidung unerwünschter Wirkungen
* Minimieren der Anzahl der Stiloptionen und Permutationen
   * Zu viele Optionen können zu einer mangelnden Markenkonsistenz für Aussehen und Gefühl führen
   * Kann für Inhaltsautoren Verwirrung darüber verursachen, welche Kombinationen erforderlich sind, um den gewünschten Effekt zu erzielen
   * Erhöht die Permutationen, deren Qualität überprüft werden muss
* Verwenden benutzerfreundlicher Beschriftungen und Kategorien für Business-Stile
   * &quot;Blue&quot;und &quot;Red&quot;anstelle von &quot;Primär&quot;und &quot;Sekundär&quot;
   * &quot;Karte&quot;und &quot;Hero&quot;anstelle von &quot;Variante A&quot;und &quot;Variante B&quot;
   * Dies kann für einige Kunden eher allgemein sein. das Design-Team, das Business-Team und das Content-Team sind sehr vertraut mit ihren primären und sekundären Farben oder den Varianten, die sie testen. Aus Gründen der Flexibilität und des Potenzials für zukünftige Veränderungen kann die Verwendung bestimmter Begriffe jedoch effizienter sein.

## Wichtige Schritte

Stilsysteme reduzieren den Bedarf an komplexen Dialogfeldern, sind jedoch keine Dialogfeldersetzungen. Sie helfen dabei, Dinge zu vereinfachen. In einigen Fällen können Sie jedoch Komponenteneigenschaften oder Dialogfelder verwenden, anstatt ein Stilsystem dafür zu erstellen.

Sie können Prozesse aus Entwicklungsperspektive optimieren. Mit einem Stilsystem können Sie mehrere Erscheinungsformen desselben Inhalts erreichen. Ebenso können Sie aus der Sicht der Autoren die Authoring-Geschwindigkeit beschleunigen, anstatt die Autoren von Schulungen und Autoren daran zu erinnern, welche Komponente in welchem Palast verwendet werden soll.

Die Dinge sind einfach sauberer. Die HTML innerhalb der Kernkomponenten ist sehr ausführlich. Wenn Sie all dies auf CSS-Ebene durchführen, wird der Komponenten-Build schneller und der Code ist auch sauberer.

Schließlich ist der Einsatz von Stilsystemen mehr Kunst als Wissenschaft. Wie bereits erwähnt, gibt es eine Reihe von Best Practices, aber Sie können die Einrichtung Ihres Unternehmens flexibel anpassen.

Weitere Informationen finden Sie in unserer [Kundenerfolgs-Webinar](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) mit Will Brisbane und Joseph Van Buskirk.

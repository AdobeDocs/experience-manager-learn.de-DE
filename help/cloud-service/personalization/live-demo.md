---
title: Live-Demonstration von Personalization-Anwendungsfällen
description: Erleben Sie die Personalisierung in Aktion auf der WKND-Aktivierungs-Website mit A/B-Tests, Verhaltens-Targeting und Beispielen für die Personalisierung bekannter Benutzender.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations
role: Developer, Leader, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-11-03T00:00:00Z
jira: KT-19546
thumbnail: KT-19546.jpeg
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '944'
ht-degree: 2%

---

# Live-Demonstration von Anwendungsfällen zur Personalisierung

Besuchen Sie die [WKND-Aktivierungs](https://wknd.enablementadobe.com/de/de.html){target="wknd"}Website, um reale Beispiele für A/B-Tests, Verhaltens-Targeting und bekannte Benutzerpersonalisierung zu sehen.

>[!VIDEO](https://video.tv.adobe.com/v/3476461/?learn=on&enablevpops)

Diese Seite führt Sie durch praktische Demonstrationen der einzelnen Personalisierungsszenarien. Erkunden Sie die Möglichkeiten, bevor Sie diese Funktionen auf Ihrer eigenen AEM-Site erstellen.

>[!IMPORTANT]
>
> Öffnen Sie die Demo-Site in mehreren Browser-Fenstern oder im Inkognito-/privaten Browser-Modus, um verschiedene personalisierte Varianten gleichzeitig zu erleben.
> Bei Verwendung des privaten Browser-Modus können Firefox und Safari das ECID-Cookie blockieren. Alternativ können Sie den regulären Browser-Modus verwenden oder Cookies löschen, bevor Sie ein neues Personalisierungsszenario ausprobieren.

## Demo-Anwendungsfälle

Die [WKND-Aktivierungs](https://wknd.enablementadobe.com/de/de.html){target="wknd"}Website zeigt drei Arten der Personalisierung:

| Personalization-Typ | Was Sie sehen werden | Timing |
|---------------------|-----------------|---------|
| **Behavioral Targeting** | Inhalte werden je nach Ihrem Browser-Verhalten und Ihren Interessen angepasst. Wird häufig als _Personalisierung der nächsten Seite oder der gleichen Seite“_ | Echtzeit und Batch |
| **Personalization für bekannten Benutzer** | Maßgeschneiderte Erlebnisse basierend auf vollständigen Kundenprofilen, die aus Daten über mehrere Systeme hinweg erstellt wurden. Wird im Allgemeinen als &quot;_in jedem Maßstab“_ | Echtzeit |
| **A/B-Tests** | Verschiedene Inhaltsvarianten wurden getestet, um die beste Leistung zu erzielen. Wird im Allgemeinen als _bezeichnet_ | Echtzeit |

## Verhaltens-Targeting

Inhalte werden automatisch entsprechend den Aktionen und Interessen der Besucher während ihrer Browser-Sitzung angepasst. Dies wird häufig als _Personalisierung der nächsten Seite oder der gleichen Seite“_.

### Startseite, Adventures und Magazine

Diese Erlebnisse werden sofort basierend auf Ihrem aktuellen Browser-Verhalten angezeigt (Echtzeit-Personalisierung). Adobe Experience Platform Edge Network wird verwendet, um Entscheidungen zur Personalisierung in Echtzeit zu treffen.

| Seite   | Was Sie sehen werden | Testen | Erfahrung |
|------|-----------------|-------------|------------|
| [Home](https://wknd.enablementadobe.com/de/de.html){target="wknd"} | Ein personalisiertes **familienfreundliches Adventures Hero-Banner** mit einem Familien-Bike am See, das geführte Erlebnisse fördert, die gemeinsame Erinnerungen schaffen | Besuchen Sie [Bali Surf Camp](https://wknd.enablementadobe.com/us/en/adventures/bali-surf-camp.html){target="wknd"} oder [Gastronomische Marais Tour](https://wknd.enablementadobe.com/us/en/adventures/gastronomic-marais-tour.html){target="wknd"} und kehren Sie dann auf die Homepage zurück | ![Home - Familien-Abenteuer-Held](./assets/live-demo/behavioral-home-family-hero.png){width="200" zoomable="yes"} |
| [Abenteuer](https://wknd.enablementadobe.com/us/en/adventures.html){target="wknd"} | Ein Werbe-Held für den Radsport-**„Free Bike Tune Up** mit „We&#39;ve Got You Covered“-Nachrichten und einem kostenlosen Angebot für die Wartung von Fahrrädern von WKND-Experten-Partnern | Besuchen Sie alle mit dem Fahrrad verbundenen Abenteuer (z. B. [Radfahren Toskana](https://wknd.enablementadobe.com/us/en/adventures/cycling-tuscany.html){target="wknd"}) und navigieren Sie dann zur Seite Adventures . | ![Abenteuer - Free Bike Tune Up Hero](./assets/live-demo/behavioral-adventures-bike-hero.png){width="200" zoomable="yes"} |
| [Abenteuer](https://wknd.enablementadobe.com/us/en/adventures.html){target="wknd"} | Ein Camping-Thema **Gear Collection Hero** präsentiert wichtige Camping-Ausrüstung (Schlafsäcke, Jacken, Stiefel) mit der Botschaft „Dein nächstes Abenteuer beginnt mit dem richtigen Zahnrad“ | Besuchen Sie alle Campingabenteuer (z. B. [Yosemite Backpacking](https://wknd.enablementadobe.com/us/en/adventures/yosemite-backpacking.html){target="wknd"}) und navigieren Sie dann zur Seite Adventures . | ![Adventures - Camp Gear Collection Hero](./assets/live-demo/behavioral-adventures-camp-hero.png){width="200" zoomable="yes"} |
| [Magazin](https://wknd.enablementadobe.com/us/en/magazine.html){target="wknd"} | Eine zeitkritische **Magazin-Verkaufsaktion** mit gerollten WKND-Magazinen mit hervorgehobenem „SALE!“ Abzeichen und spezielle Leserpreise für Anfragen und Outdoor-Kollektionen | Lesen Sie einen oder mehrere Zeitschriftenartikel (z. B[ „Skitouren](https://wknd.enablementadobe.com/us/en/magazine/ski-touring.html){target="wknd"}) und navigieren Sie dann zur Landingpage für das Magazin | ![Magazin - Verkaufsheld](./assets/live-demo/behavioral-magazine-sale-hero.png){width="200" zoomable="yes"} |

### Adventures- und Magazin-Seiten (Batch)

Diese Erlebnisse basieren auf einem früheren Verhalten und erscheinen bei Ihrem nächsten Besuch oder später am selben Tag (Batch-Personalisierung). Die Daten werden aggregiert, zu Profilattributen verarbeitet und für Adobe Experience Platform Edge Network aktiviert.

| Seite   | Was Sie sehen werden | Testen | Erfahrung |
|------|-----------------|-------------|------------|
| [Abenteuer](https://wknd.enablementadobe.com/us/en/adventures.html){target="wknd"} | Ein Held zum Surfen mit **bunten Surfbrettern unter Palmen** mit „Your Surf Journey Starts Here“-Botschaften und kuratierten Surfzielinhalten, die auf Ihren Interessen basieren | Besuchen Sie mehrere [surfbezogene Adventures](https://wknd.enablementadobe.com/us/en/adventures.html#tabs-b4210c6ff3-item-b411b19941-tab){target="wknd"} und kehren Sie dann am nächsten Tag zur Adventures-Seite zurück | ![Abenteuer - Surfhelden](./assets/live-demo/behavioral-adventures-surfing-hero.png){width="200" zoomable="yes"} |
| [Magazin](https://wknd.enablementadobe.com/us/en/magazine.html){target="wknd"} | Ein **personalisiertes Zeitschriftenabonnement** mit Weltreisezielen mit einem klassischen VW-Transporter, der „Ihr personalisiertes Zeitschriftenerlebnis“ mit exklusiven Abonnentenvorteilen betont | Lesen Sie 3 oder mehr [Zeitschriftenartikel](https://wknd.enablementadobe.com/us/en/magazine.html){target="wknd"} und kehren Sie dann am nächsten Tag zur Landingpage des Magazins zurück | ![Magazine - Abonnieren Sie Hero](./assets/live-demo/behavioral-magazine-subscribe-hero.png){width="200" zoomable="yes"} |

**Weitere Informationen:** Bereit, das Behavioral Targeting auf Ihrer eigenen AEM-Site zu implementieren? Beginnen Sie mit dem [Tutorial zum Verhalten beim Targeting](./use-cases/behavioral-targeting.md), um mehr über den vollständigen Einrichtungsprozess zu erfahren.

## Personalisierung für bekannte Benutzende

Personalisierte Erlebnisse basierend auf vollständigen Kundenprofilen, die aus Daten über mehrere Systeme hinweg erstellt wurden, einschließlich Kaufverlauf und Kundenlebenszyklusphase. Adobe Experience Platform Edge Network wird verwendet, um Entscheidungen zur Personalisierung in Echtzeit zu treffen.

### Startseiten-Held

Das Hero-Banner der WKND-Startseite wird basierend auf authentifizierten Benutzerprofilen personalisiert. Testen Sie mit diesen Demo-Konten, um ein personalisiertes Erlebnis zu sehen:

| Seite   | Was Sie sehen werden | Testen | Profilkontext | Erfahrung |
|------|-----------------|-------------|-----------------|------------|
| [Home](https://wknd.enablementadobe.com/de/de.html){target="wknd"} | Ein Skishop-Interieur mit **Premium-Skiausrüstung mit „EXTRA 25% RABATT“** Aktion, mit erfahrenen Packtipps zur Vorbereitung auf das bevorstehende Skiabenteuer | Melden Sie sich mit `rwilson/rwilson` an und aktualisieren Sie die Seite | Kürzlich erworbene Ski-Abenteuer, damit Upsell Skiausrüstung | ![Startseite - Skiausrüstung Upsell](./assets/live-demo/known-user-ski-gear-hero.png){width="200" zoomable="yes"} |

**Weitere Informationen:** Sind Sie bereit, die Personalisierung bekannter Benutzender auf Ihrer eigenen AEM-Site zu implementieren? Beginnen Sie mit dem [Personalization-Tutorial für bekannte Benutzende](./use-cases/known-user-personalization.md) um mehr über den vollständigen Einrichtungsprozess zu erfahren.

## A/B-Tests (Experiment)

Testen Sie verschiedene Inhaltsvarianten, um festzustellen, welche am besten für Ihre Geschäftsziele geeignet ist. Adobe Target stellt nach dem Zufallsprinzip verschiedene Varianten für Besucher und Tracks bereit, die eine bessere Leistung erzielen. Dies wird häufig als &quot;_&quot;_.

### Artikel zum Thema Startseite

Auf der WKND-Homepage wird ein aktiver A/B-Test mit drei Varianten des Artikels _Camping in Western Australia_ durchgeführt. Jeder Besucher wird nach dem Zufallsprinzip zugewiesen, um eine dieser Varianten zu sehen:

| Seite   | Was Sie sehen werden | Testen | Erfahrung |
|------|-----------------|-------------|------------|
| [Home](https://wknd.enablementadobe.com/de/de.html){target="wknd"} | Eine von drei zufällig zugewiesenen Artikelvarianten im Abschnitt „Unsere vorgestellten“: **„Off the Grid: Epic Camping Routes Across Western Australia“** oder **„Wandering the Wild: Camping Adventures in Western Australia“** (oder eine dritte Variante), jeweils mit einzigartigen Bildern und Botschaften, um zu testen, welche am besten Resonanz finden | Besuchen Sie die Homepage in verschiedenen Browsern, verwenden Sie den inkognito/privaten Modus oder löschen Sie Cookies, um verschiedene Varianten zu sehen | ![A/B-Testvarianten](./assets/live-demo/ab-test-variations.png){width="200" zoomable="yes"} |

**Weitere Informationen:** Sie A/B-Tests auf Ihrer eigenen AEM-Site implementieren? Beginnen Sie mit dem [Tutorial zum Experimentieren (A/B](./use-cases/experimentation.md)Test), um mehr über den vollständigen Einrichtungsprozess zu erfahren.


## Nächste Schritte

Sind Sie bereit, die Personalisierung auf Ihrer eigenen AEM-Site zu implementieren? Beginnen Sie mit der [Übersicht über ](./overview.md) Personalization, um mehr über den vollständigen Einrichtungsprozess zu erfahren.



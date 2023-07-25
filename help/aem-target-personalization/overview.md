---
title: Erste Schritte mit AEM und Adobe Target
description: Ein Tutorial, das von Anfang bis Ende zeigt, wie mithilfe von Adobe Experience Manager und Adobe Target personalisierte Erlebnisse erstellt und bereitgestellt werden können. In diesem Tutorial erfahren Sie außerdem über verschiedene Rollen, die am End-to-End-Prozess beteiligt sind, und darüber, wie sie miteinander zusammenarbeiten.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
exl-id: b632883f-65fd-4f89-bf39-ec2bce352d2d
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '842'
ht-degree: 99%

---

# Integrieren von AEM Sites und Adobe Target {#getting-started-with-aem-target}

Sowohl AEM als auch Target sind leistungsstarke Lösungen mit sich scheinbar überschneidenden Funktionen. Kundinnen und Kunden haben manchmal Schwierigkeiten damit, zu verstehen, wie und wann diese Produkte zusammen verwendet werden, um personalisierte Erlebnisse bereitzustellen. Um für alle Endbenutzenden ein optimiertes Erlebnis zu bieten, sollten verschiedene Teams in Ihrer Organisation eng zusammenarbeiten und definieren, wer was tut.

In diesem Tutorial behandeln wir drei verschiedene Szenarien für AEM und Target, anhand derer Sie nachvollziehen können, was für Ihre Organisation am besten funktioniert und wie verschiedene Teams zusammenarbeiten.

* Szenario 1: Personalisierung mit AEM Experience Fragments
* Szenario 2: Personalisierung mit Visual Experience Composer
* Szenario 3: Personalisierung vollständiger Web-Seiten-Erlebnisse

## Personalisierung mit AEM Experience Fragments {#personalization-using-aem-experience-fragment}

Für dieses Szenario werden wir AEM und Target verwenden. Beide Produkte haben eindeutig ihre eigenen Stärken. Wenn es darum geht, den Personen, die Ihre Site benutzen, personalisierte Erlebnisse bereitzustellen, benötigen Sie **personalisierte Inhalte (Inhalte aus AEM)** und eine **intelligente Methode (Target)**, um diese Inhalte basierend auf einer bestimmten Person bereitzustellen.

AEM hilft Ihnen bei der Erstellung personalisierter Inhalte, indem all Ihre Inhalte und Assets an einem zentralen Ort zusammengeführt werden, um Ihre Personalisierungsstrategie zu unterstützen. Mit AEM können Sie mühelos Inhalte für PCs, Tablets und Mobilgeräte an einem Ort erstellen, ohne Code zu schreiben. Es ist nicht erforderlich, Seiten für jedes Gerät zu erstellen – AEM passt jedes Erlebnis automatisch an Ihre Inhalte an. Sie können Inhalte auch auf Tastendruck als Angebote von AEM nach Adobe Target exportieren.

Wir verfügen jetzt über personalisierte Inhalte in Form von Angeboten aus AEM in Target. Mit Target können Sie diese Angebote skaliert bereitstellen, basierend auf einer Kombination aus regelbasierten und KI-gestützten Ansätzen des maschinellen Lernens, die Verhaltens-, Kontext- und Offline-Variablen beinhalten.  Mit Target können Sie mühelos A/B- und Multivarianz-Aktivitäten (MVT) einrichten und ausführen, um die besten Angebote, Inhalte und Erlebnisse zu ermitteln.

**Experience Fragments** stellt einen großen Schritt nach vorne dar, um die Erstellerinnen und Ersteller von Inhalten/Erlebnissen mit den Personalisierungsexperten zu verknüpfen, die Geschäftsergebnisse mit Target optimieren.

* Mit dem AEM-Inhaltseditor werden personalisierte Inhalte als Experience Fragments und deren Varianten verfasst.
* AEM exportiert Experience Fragment-HTML in Target
* Target verwendet AEM Experience Fragment-Markup als Angebote in Aktivitäten
* Target liefert Experience Fragment-HTML, AEM stellt referenzierte Bilder bereit

  ![Diagramm zur Personalisierung mit Experience Fragments](assets/personalization-use-case-1/use-case-1-diagram.png)

**Um dieses Szenario zu implementieren, ist Folgendes erforderlich:**

* [Integrieren von AEM und Adobe Target mithilfe von Launch und Adobe I/O](./implementation.md#integrating-aem-target-options)
* [AEM und Adobe Target, die Legacy-Cloud-Services verwenden](./implementation.md#integrating-aem-target-options)

***Nach der Implementierung der oben genannten Integrationen sehen wir uns jetzt das [Szenario im Detail](./personalization-use-case-1.md) an.***

## Personalisierung mit Visual Experience Composer

Marketing-Fachleute können mit dem Adobe Target Visual Experience Composer (VEC) schnelle Änderungen an ihrer Website vornehmen, um einen Test durchzuführen, ohne Code zu ändern. Der VEC ist eine WYSIWYG-Benutzeroberfläche (What you see is what you get), mit der Sie personalisierte Erlebnisse und Angebote einfach im Site-Kontext erstellen und testen können. Sie können Erlebnisse und Angebote für Target-Aktivitäten erstellen, indem Sie das Layout und den Inhalt einer Web-Seite (oder eines Angebots) oder einer mobilen Web-Seite per Drag-and-Drop austauschen und ändern.

VEC ist eine der Hauptfunktionen von Adobe Target. Mit VEC können Marketing- und Design-Fachleute Inhalte über eine visuelle Benutzeroberfläche erstellen und ändern. Viele Design-Entscheidungen können getroffen werden, ohne dass eine direkte Bearbeitung des Codes erforderlich ist. Die Bearbeitung von HTML und JavaScript ist auch mit den im Composer verfügbaren Bearbeitungsoptionen möglich.

* Inhalte befinden sich in AEM, und die Seiten der Website werden durch Content-Editor erstellt und verwaltet.
* Target nutzt von AEM gehostete Website-Seiten für Test- und Personalisierungszwecke.
* Target stellt personalisierte Inhalte bereit.
* Neue Inhalte werden mit Adobe Target VEC erstellt
* Gilt sowohl für in AEM gehostete als auch für nicht in AEM gehostete Sites

  ![Diagramm zur Personalisierung mit Visual Experience Composer](assets/personalization-use-case-3/use-case-diagram-3.png)

**Um dieses Szenario zu implementieren, ist Folgendes erforderlich:**

* [Integrieren von AEM und Adobe Target mithilfe von Launch und Adobe I/O](./implementation.md#integrating-aem-target-options)

***Nach der Implementierung der oben genannten Integration sehen wir uns nun das [Szenario im Detail an.](./personalization-use-case-3.md)***

## Personalisierung vollständiger Web-Seiten-Erlebnisse

Durch die Integration von Adobe Experience Manager mit Adobe Target können Sie Ihren Site-Benutzenden ein personalisiertes Erlebnis bieten. Darüber hinaus erhalten Sie damit ein besseres Verständnis darüber, welche Versionen Ihrer Website-Inhalte Ihre Konversionen während eines bestimmten Testzeitraums am besten verbessern. Ein A/B-Test vergleicht beispielsweise zwei oder mehr Versionen Ihres Website-Inhalts, um festzustellen, welche Version Ihre Konversionen, Verkäufe oder andere von Ihnen identifizierten Metriken am meisten erhöht. Marketing-Fachleute können in Adobe Target Aktivitäten erstellen, um zu verstehen, wie Benutzende mit dem Inhalt Ihrer Website interagieren und wie sich dies auf Ihre Website-Metriken auswirkt.

* Inhalte befinden sich in AEM, und die Seiten der Website werden durch Content-Editor erstellt und verwaltet.
* Target nutzt von AEM gehostete Website-Seiten für Test- und Personalisierungszwecke.
* Target stellt personalisierte Inhalte bereit.
* Hier wird kein neuer Inhalt erstellt.
* Gilt sowohl für AEM. als auch für AEM-fremde Websites.

  ![Diagramm](assets/personalization-use-case-2/use-case-2-diagram.png)

**Um dieses Szenario zu implementieren, müssen Sie folgenden Vorgang durchführen:**

* [Integrieren von AEM und Adobe Target mithilfe von Launch und Adobe I/O](./implementation.md#integrating-aem-target-options)

***Nach Implementierung der oben genannten Integration sollten wir nun das [Szenario im Detail](./personalization-use-case-2.md)*** ansehen.

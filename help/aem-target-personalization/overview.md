---
title: Erste Schritte mit AEM und Adobe Target
description: Ein durchgehendes Tutorial, in dem gezeigt wird, wie mit Adobe Experience Manager und Adobe Target personalisierte Erlebnisse erstellt und bereitgestellt werden. In diesem Tutorial erfahren Sie außerdem über verschiedene Rollen, die am End-to-End-Prozess beteiligt sind, und darüber, wie sie miteinander zusammenarbeiten.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: b632883f-65fd-4f89-bf39-ec2bce352d2d
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: tm+mt
source-wordcount: '840'
ht-degree: 1%

---

# Erste Schritte mit AEM und Adobe Target {#getting-started-with-aem-target}

Sowohl AEM als auch Target sind leistungsstarke Lösungen mit scheinbar überlappenden Funktionen. Kunden haben manchmal Schwierigkeiten damit, zu verstehen, wie und wann diese Produkte verwendet werden, um personalisierte Erlebnisse bereitzustellen. Um für jeden Endbenutzer ein optimiertes Erlebnis zu bieten, sollten verschiedene Teams in Ihrer Organisation eng zusammenarbeiten und definieren, wer was tut.

In diesem Tutorial decken wir drei verschiedene Szenarien für AEM und Target ab, anhand derer Sie nachvollziehen können, was für Ihre Organisation am besten funktioniert und wie verschiedene Teams zusammenarbeiten.

* Szenario 1 : Personalisierung mit AEM Experience Fragments
* Szenario 2 : Personalisierung mit Visual Experience Composer
* Szenario 3 : Personalisierung vollständiger Webseiten-Erlebnisse

## Personalisierung mit AEM Experience Fragments {#personalization-using-aem-experience-fragment}

Für dieses Szenario werden wir AEM und Target verwenden. Beide Produkte haben eindeutig ihre eigenen Stärken. Wenn es darum geht, den Benutzern Ihrer Site personalisierte Erlebnisse bereitzustellen, benötigen Sie **personalisierter Inhalt (Inhalt aus AEM)** und **intelligente Methode (Target)** , um diese Inhalte basierend auf einem bestimmten Benutzer bereitzustellen.

AEM hilft Ihnen bei der Erstellung personalisierter Inhalte, indem Sie all Ihre Inhalte und Assets an einem zentralen Ort zusammenführen, um Ihre Personalisierungsstrategie zu unterstützen. Mit AEM können Sie mühelos Inhalte für Desktops, Tablets und Mobilgeräte an einem Ort erstellen, ohne Code zu schreiben. Es ist nicht erforderlich, Seiten für jedes Gerät zu erstellen. AEM passt jedes Erlebnis automatisch an Ihren Inhalt an. Sie können den Inhalt auch als Angebote mit Push-Benachrichtigung von AEM nach Adobe Target exportieren.

Wir verfügen jetzt über personalisierte Inhalte in Form von Angeboten aus AEM in Target. Mit Target können Sie diese Angebote skaliert bereitstellen, basierend auf einer Kombination aus regelbasierten und KI-gestützten Ansätzen des maschinellen Lernens, die Verhaltens-, Kontext- und Offline-Variablen beinhalten.  Mit Target können Sie mühelos A/B- und Multivarianz-Aktivitäten (MVT) einrichten und ausführen, um die besten Angebote, Inhalte und Erlebnisse zu ermitteln.

**Experience Fragments** einen großen Schritt vorwärts darstellen, um die Ersteller von Inhalten/Erlebnissen mit den Personalisierungsexperten zu verknüpfen, die Geschäftsergebnisse mit Target optimieren.

* AEM Inhaltseditor-Autoren personalisierte Inhalte als Experience Fragments und deren Varianten
* AEM exportiert Experience Fragment-HTML in Target-&#x200B;
* Target &#x200B; verwendet AEM Experience Fragment-Markup als Angebote in Aktivitäten
* Target stellt Experience Fragment-HTML bereit, AEM referenzierte Bilder bereitstellt

   ![Diagramm zur Personalisierung mit Experience Fragments](assets/personalization-use-case-1/use-case-1-diagram.png)

**Um dieses Szenario zu implementieren, müssen Sie:**

* [Integrieren von AEM und Adobe Target mithilfe von Launch und Adobe I/O](./implementation.md#integrating-aem-target-options)
* [AEM und Adobe Target mit veralteten Cloud Services](./implementation.md#integrating-aem-target-options)

***Nachdem Sie die oben genannten Integrationen implementiert haben, sollten Sie die [detailliert](./personalization-use-case-1.md).***

## Personalisierung mit Visual Experience Composer

Marketingexperten können schnelle Änderungen an ihrer Website vornehmen, ohne Code zu ändern, um einen Test mit dem Adobe Target Visual Experience Composer (VEC) durchzuführen. Der VEC ist die WYSIWYG-Benutzeroberfläche (was Sie sehen ist, was Sie erhalten), mit der Sie einfach personalisierte Erlebnisse und Angebote im Site-Kontext erstellen und testen können. Sie können Erlebnisse und Angebote für Target-Aktivitäten erstellen, indem Sie das Layout und den Inhalt einer Web-Seite (oder eines Angebots) oder einer mobilen Web-Seite per Drag &amp; Drop austauschen und ändern.

VEC ist eine der Hauptfunktionen von Adobe Target. Mit VEC können Marketing-Experten und Designer Inhalte über eine visuelle Benutzeroberfläche erstellen und ändern. Viele Entwurfsentscheidungen können getroffen werden, ohne dass eine direkte Bearbeitung des Codes erforderlich ist. Die Bearbeitung von HTML und JavaScript ist auch mit den im Composer verfügbaren Bearbeitungsoptionen möglich.

* Inhalte befinden sich in AEM und Content Editors erstellen und verwalten die Seiten der Site
* Target verwendet AEM gehostete Site-Seiten, um Tests und Personalisierung auszuführen
* Target stellt personalisierte Inhalte bereit
* Neue Inhalte werden mit Adobe Target VEC erstellt.
* Gilt sowohl für AEM gehostete als auch für nicht AEM gehostete Sites

   ![Personalisierung mit Visual Experience Composer-Diagramm](assets/personalization-use-case-3/use-case-diagram-3.png)

**Um dieses Szenario zu implementieren, müssen Sie:**

* [Integrieren von AEM und Adobe Target mithilfe von Launch und Adobe I/O](./implementation.md#integrating-aem-target-options)

***Nach der Implementierung der oben genannten Integration sollten Sie die [detailliert beschrieben.](./personalization-use-case-3.md)***

## Personalisierung vollständiger Webseiten-Erlebnisse

Durch die Integration von Adobe Experience Manager mit Adobe Target können Sie Ihren Site-Benutzern ein personalisiertes Erlebnis bieten. Darüber hinaus erhalten Sie damit ein besseres Verständnis darüber, welche Versionen Ihrer Website-Inhalte Ihre Konversionen während eines bestimmten Testzeitraums am besten verbessern. Ein A/B-Test vergleicht beispielsweise zwei oder mehr Versionen Ihres Website-Inhalts, um festzustellen, welche Version Ihre Konversionen, Verkäufe oder anderen von Ihnen identifizierten Metriken am meisten erhöht. Marketingexperten können in Adobe Target Aktivitäten erstellen, um zu verstehen, wie Benutzer mit dem Inhalt Ihrer Site interagieren und wie er sich auf Ihre Site-Metriken auswirkt.

* Inhalte befinden sich in AEM und Content Editors erstellen und verwalten die Seiten der Site
* Target verwendet AEM gehostete Site-Seiten, um Tests und Personalisierung auszuführen
* Target stellt personalisierte Inhalte bereit
* Hier wird kein neuer Inhalt erstellt
* Gilt sowohl für AEM als auch für Nicht-AEM-Sites

   ![Diagramm](assets/personalization-use-case-2/use-case-2-diagram.png)

**Um dieses Szenario zu implementieren, müssen Sie:**

* [Integrieren von AEM und Adobe Target mithilfe von Launch und Adobe I/O](./implementation.md#integrating-aem-target-options)

***Nach der Implementierung der oben genannten Integration sollten Sie die [detailliert beschrieben.](./personalization-use-case-2.md)***

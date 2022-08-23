---
title: Namenskonventionen und Best Practices für die Erstellung adaptiver Formulare
description: Namenskonventionen und Best Practices für die Erstellung adaptiver Formulare
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: fbfc74d7-ba7c-495a-9e3b-63166a3025ab
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 8%

---

# Best Practices

Mit Adobe Experience Manager (AEM) Forms können Sie komplexe Transaktionen in einfache, beeindruckende digitale Erlebnisse umwandeln. Im folgenden Dokument werden einige zusätzliche Best Practices beschrieben, die bei der Entwicklung von Adaptive Forms befolgt werden müssen. Dieses Dokument soll zusammen mit [dieses Dokuments](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)

## Benennungskonventionen

* **Bedienfelder**
   * Bereichsnamen sind Binnenmajuskeln, die mit einem Großbuchstaben beginnen.

* **Formularfelder**
   * Feldnamen sind Binnenmajuskeln, die mit einem Kleinbuchstaben beginnen.
   * Starten Sie Feldnamen nicht mit Zahlen
   * Fügen Sie in Ihren Namen keine Bindestriche &quot;-&quot;ein. Diese entsprechen einem Minuszeichen in Ihrem Code und fungieren als Operatoren in Ihrem Code.
   * Namen können Buchstaben, Ziffern, Unterstriche und Dollarzeichen enthalten.
   * Namen müssen mit einem Brief beginnen
   * Bei Namen wird zwischen Groß- und Kleinschreibung unterschieden
   * Reservierte Wörter (wie JavaScript-Schlüsselwörter) können nicht als Namen verwendet werden. Achten Sie auf andere AF-spezifische reservierte Wörter wie &quot;panel&quot;,&quot;name&quot;.
   * Fügen Sie in Ihren Namen keine Bindestriche &quot;-&quot;ein.
* **Entwickeln von Forms**
   * Formularfragmente sollten bei der Entwicklung großer Formulare berücksichtigt werden. Lazy Loading von Formularfragmenten für schnellere Ladezeiten aktivieren
   * **DataModel**
      * Es wird empfohlen, das adaptive Formular mit dem entsprechenden Datenmodell zu verknüpfen
   * **Objektereignisse**
      * Code, der sich auf die Sichtbarkeit eines Objekts bezieht, sollte immer im Sichtbarkeitsereignis dieses Objekts platziert werden.
   * **Script**
      * Wenn der Code, den Sie in einem adaptiven Formular schreiben, über fünf sichtbare Zeilen hinausgeht, müssen Sie Ihren Code in eine Client-Bibliothek verschieben. Fügen Sie im Idealfall Ihre Funktion zur Client-Bibliothek hinzu und fügen Sie dann die entsprechenden jsdoc-Tags hinzu, damit die Funktion im Regeleditor für adaptive Formulare sichtbar ist.

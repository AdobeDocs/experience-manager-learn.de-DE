---
title: Namenskonventionen und Best Practices beim Erstellen adaptiver Formulare
description: Namenskonventionen und Best Practices beim Erstellen adaptiver Formulare
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: fbfc74d7-ba7c-495a-9e3b-63166a3025ab
last-substantial-update: 2020-09-10T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: ht
source-wordcount: '287'
ht-degree: 100%

---

# Best Practices

Mit Adobe Experience Manager(AEM)-Formularen können Sie komplexe Transaktionen in einfache, beeindruckende digitale Erlebnisse umwandeln. Im folgenden Dokument werden einige zusätzliche Best Practices beschrieben, die bei der Entwicklung von adaptiven Formularen zu befolgen sind. Dieses Dokument soll zusammen mit [diesem Dokument](https://helpx.adobe.com/de/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview) verwendet werden.

## Benennungskonventionen

* **Panels**
   * Die Namen von Panels werden mit Binnenmajuskeln geschrieben und beginnen mit einem Großbuchstaben.

* **Formularfelder**
   * Feldnamen werden mit Binnenmajuskeln geschrieben und beginnen mit einem Kleinbuchstaben.
   * Beginnen Sie Feldnamen nicht mit einer Zahl.
   * Fügen Sie in Ihren Namen keine Bindestriche („-“) ein. Diese würden in Ihrem Code einem Minuszeichen entsprechen und dort als Operatoren fungieren.
   * Namen dürfen Buchstaben, Ziffern, Unterstriche und Dollarzeichen enthalten.
   * Namen müssen mit einem Buchstaben beginnen.
   * Bei Namen wird zwischen Groß- und Kleinschreibung unterschieden.
   * Reservierte Wörter (wie JavaScript-Keywords) können nicht als Namen verwendet werden. Achten Sie auf andere AF-spezifische reservierte Wörter wie „panel“ oder „name“.
   * Fügen Sie in Ihren Namen keine Bindestriche („-“) ein.
* **Entwickeln von Formularen**
   * Formularfragmente sollten bei der Entwicklung großer Formulare berücksichtigt werden. Aktivieren Sie Lazy Loading (verzögertes Laden) von Formularfragmenten für schnellere Ladezeiten.
   * **Datenmodell**
      * Es wird empfohlen, das adaptive Formular mit dem entsprechenden Datenmodell zu verknüpfen.

   * **Objektereignisse**
      * Code, der sich auf die Sichtbarkeit eines Objekts bezieht, sollte immer im Sichtbarkeitsereignis dieses Objekts platziert werden.
   * **Skript**
      * Wenn der Code, den Sie in einem adaptiven Formular schreiben, über fünf sichtbare Zeilen hinausgeht, müssen Sie Ihren Code in eine Client-Bibliothek verschieben. Nehmen Sie im Idealfall Ihre Funktion in die Client-Bibliothek auf und fügen Sie dann die entsprechenden jsdoc-Tags hinzu, damit die Funktion im Regeleditor für adaptive Formulare sichtbar ist.

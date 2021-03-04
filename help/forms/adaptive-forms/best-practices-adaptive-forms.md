---
title: Benennungskonventionen und Best Practices beim Erstellen adaptiver Formulare
seo-title: Benennungskonventionen und Best Practices beim Erstellen adaptiver Formulare
description: Benennungskonventionen und Best Practices beim Erstellen adaptiver Formulare
seo-description: Benennungskonventionen und Best Practices beim Erstellen adaptiver Formulare
feature: Adaptive Formulare
topics: best-practices
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
topic: Entwicklung
role: Entwickler
level: Anfänger
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 8%

---

# Best Practices

Mit Adobe Experience Manager (AEM) Forms können Sie komplexe Transaktionen in einfache, beeindruckende digitale Erlebnisse umwandeln. Im folgenden Dokument werden einige weitere Best Practices beschrieben, die bei der Entwicklung des adaptiven Forms befolgt werden müssen. Dieses Dokument ist in Verbindung mit [diesem Dokument](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)

## Benennungskonventionen

* **Bedienfelder**
   * Bereichsnamen sind Kamelschreibungen, die mit einem Großbuchstaben beginnen.

* **Formularfelder**
   * Feldnamen sind Kamelschreibungen, die mit einem Kleinbuchstaben beginnen.
   * Beginn von Feldnamen mit Zahlen nicht
   * Fügen Sie keine Bindestriche &quot;-&quot;in Ihre Namen ein. Diese entsprechen einem Minuszeichen in Ihrem Code und fungieren als Operatoren in Ihrem Code.
   * Namen können Buchstaben, Ziffern, Unterstriche und Dollarzeichen enthalten.
   * Namen müssen mit einem Brief beginnen
   * Bei Namen muss die Groß-/Kleinschreibung beachtet werden
   * Reservierte Wörter (wie JavaScript-Suchbegriffe) können nicht als Namen verwendet werden. Achten Sie auf andere AF-spezifische reservierte Wörter wie   als &quot;panel&quot;,&quot;name&quot;.
   * Fügen Sie keine Bindestriche &quot;-&quot;in Ihren Namen ein
* **Forms entwickeln**
   * Formularfragmente sollten bei der Entwicklung großer Formulare berücksichtigt werden. Verzögertes Laden von Formularfragmenten für schnelleres Laden aktivieren   times
   * **DataModel**
      * Es wird empfohlen, adaptives Formular mit dem entsprechenden Datenmodell zu verknüpfen
   * **Objekt-Ereignis**
      * Der Code zur Sichtbarkeit eines Objekts sollte immer im Sichtbarkeitscode des betreffenden Ereignisses platziert werden.
   * **Script**
      * Wenn der Code, den Sie in ein adaptives Formular schreiben, über 5 sichtbare Zeilen hinausgeht, müssen Sie den Code in eine Client-Bibliothek verschieben. Fügen Sie im Idealfall Ihre Funktion der Client-Bibliothek hinzu und fügen Sie dann die entsprechenden jsdoc-Tags hinzu, damit die Funktion im Regeleditor für adaptive Formulare sichtbar ist.



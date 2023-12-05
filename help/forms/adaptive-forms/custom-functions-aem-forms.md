---
title: Benutzerdefinierte Funktionen in AEM Forms
description: Erstellen und Verwenden benutzerdefinierter Funktionen in einem adaptiven Formular
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
jira: KT-9685
exl-id: 07fed661-0995-41ab-90c4-abde35a14a4c
last-substantial-update: 2021-06-09T00:00:00Z
duration: 322
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 100%

---

# Benutzerdefinierte Funktionen

AEM Forms 6.5 bietet nun die Möglichkeit, JavaScript-Funktionen zu definieren, mit denen komplexe Geschäftsregeln mithilfe des Regeleditors definiert werden können.
AEM Forms bietet eine Reihe solcher benutzerdefinierter Funktionen standardmäßig, aber Sie müssen Ihre eigenen benutzerdefinierten Funktionen definieren und sie in mehreren Formularen verwenden.

Gehen Sie wie folgt vor, um Ihre erste benutzerdefinierte Funktion zu definieren:
* [Melden Sie sich bei CRX an.](http://localhost:4502/crx/de/index.jsp#/apps/experience-league/clientlibs)
* Erstellen Sie unter „Apps“ einen neuen Ordner mit der Bezeichnung „experience-league“ (dieser Ordnername kann ein Name Ihrer Wahl sein).
* Speichern Sie Ihre Änderungen.
* Erstellen Sie unter dem Ordner „experience-league“ einen neuen Knoten des Typs „cq:ClientLibraryFolder“ mit dem Namen „clientlibs“.
* Wählen Sie den neu erstellten Ordner „clientlibs“ aus, fügen Sie die Eigenschaften „allowProxy“ und „categories“ wie im Screenshot gezeigt hinzu und speichern Sie Ihre Änderungen.

![client-lib](assets/custom-functions.png)
* Erstellen Sie einen Ordner mit dem Namen **js** unter dem Ordner **clientlibs**.
* Erstellen Sie eine Datei mit dem Namen **function.js** unter dem Ordner **js**.
* Erstellen Sie eine Datei mit dem Namen **js.txt** unter dem Ordner **clientlibs**. Speichern Sie Ihre Änderungen.
* Ihre Ordnerstruktur sollte wie im folgenden Screenshot aussehen.

![Regeleditor](assets/folder-structure.png)

* Doppelklicken Sie auf „functions.js“, um den Editor zu öffnen.
Kopieren Sie den folgenden Code in die Datei „functions.js“ und speichern Sie Ihre Änderungen.

```javascript
/**
* Get List of County names
* @name getCountyNamesList Get list of county names
* @return {OPTIONS} drop down options 
 */
function getCountyNamesList()
{
    var countyNames= [];
    countyNames[0] = "Santa Clara";
    countyNames[1] = "Alameda";
    countyNames[2] = "Buxor";
    countyNames[3] = "Contra Costa";
    countyNames[4] = "Merced";

    return countyNames;

}
/**
* Covert UTC to Local Time
* @name convertUTC Convert UTC Time to Local Time
* @param {string} strUTCString in Stringformat
* @return {string}
*/
function convertUTC(strUTCString)
{
    var dt = new Date(strUTCString);
    console.log(dt.toLocaleString());
    return dt.toLocaleString();
}
```

Bitte [beziehen Sie sich auf jsdoc](https://jsdoc.app/index.html) für weitere Informationen zum Kommentieren von JavaScript-Funktionen.
Der obige Code hat zwei Funktionen:
**getCountyNamesList** – gibt ein Array von Zeichenfolge zurück
**convertUTC** – konvertiert den UTC-Zeitstempel in die lokale Zeitzone

Öffnen Sie die js.txt, fügen Sie den folgenden Code ein und speichern Sie Ihre Änderungen.

```javascript
#base=js
functions.js
```

Die Zeile „#base=js“ gibt an, in welchem Verzeichnis sich die JavaScript-Dateien befinden.
Die folgenden Zeilen geben den Speicherort der JavaScript-Datei relativ zum Basisspeicherort an.

Wenn Sie Probleme beim Erstellen der benutzerdefinierten Funktionen haben, können Sie [dieses Paket herunterladen und in Ihrer AEM-Instanz installieren](assets/custom-functions.zip).

## Verwenden benutzerdefinierter Funktionen

Das folgende Video führt Sie durch die Schritte, die bei der Verwendung einer benutzerdefinierten Funktion im Regeleditor eines adaptiven Formulars erforderlich sind:
>[!VIDEO](https://video.tv.adobe.com/v/340305?quality=12&learn=on)

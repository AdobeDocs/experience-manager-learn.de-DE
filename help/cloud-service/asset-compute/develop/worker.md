---
title: Entwickeln eines Asset compute-Workers
description: asset compute-Worker sind der Kern eines Asset compute-Projekts, da sie benutzerdefinierte Funktionen bereitstellen, mit denen die an einem Asset geleistete Arbeit zur Erstellung einer neuen Darstellung ausgeführt oder orchestriert wird.
feature: asset compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6282
thumbnail: KT-6282.jpg
topic: Integrationen, Entwicklung
role: Entwickler
level: Vermittelt, erfahren
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1429'
ht-degree: 0%

---


# Entwickeln eines Asset compute-Workers

asset compute-Worker sind der Kern eines Asset compute-Projekts, da sie benutzerdefinierte Funktionen bereitstellen, mit denen die an einem Asset geleistete Arbeit zur Erstellung einer neuen Darstellung ausgeführt wird oder orchestriert wird.

Das Asset compute-Projekt generiert automatisch einen einfachen Arbeiter, der die ursprüngliche Binärdatei des Assets ohne Konvertierungen in eine benannte Darstellung kopiert. In diesem Tutorial werden wir diesen Arbeiter ändern, um eine interessantere Darstellung zu machen, um die Macht der Asset compute-Arbeiter zu veranschaulichen.

Wir erstellen einen Asset compute-Worker, der eine neue horizontale Bilddarstellung generiert, die leeren Raum links und rechts neben der Asset-Darstellung mit einer verschwommenen Version des Assets abdeckt. Die Breite, Höhe und Weichzeichnung der endgültigen Darstellung werden parametrisiert.

## Logischer Fluss eines Asset compute-Worker-Aufrufs

asset compute-Worker implementieren den Asset compute SDK-Worker-API-Vertrag in der Funktion `renditionCallback(...)`, die konzeptionell Folgendes beinhaltet:

+ __Eingabe:__ Originalparameter des binären und verarbeitenden Profils eines AEM Assets
+ __Ausgabe:__ Eine oder mehrere Darstellungen, die dem AEM Asset hinzugefügt werden sollen

![Logischer asset compute](./assets/worker/logical-flow.png)

1. Der AEM Author-Dienst ruft den Asset compute-Worker auf und stellt den Parameter __(1a)__ der Originalbinärdatei (`source`) und __(1b)__ (`rendition.instructions`) bereit.
1. Das Asset compute SDK orchestriert die Ausführung der `renditionCallback(...)`-Funktion des benutzerdefinierten Asset compute-Metadaten-Workers und generiert eine neue binäre Darstellung, basierend auf der ursprünglichen Binärdatei __(1a)__ und allen Parametern __(1b)__.

   + In diesem Tutorial wird die Darstellung &quot;in Bearbeitung&quot;erstellt, d. h. der Worker erstellt die Darstellung, die Quellbinäre kann jedoch auch zur Generierung der Darstellung an andere Webdienst-APIs gesendet werden.

1. Der Asset compute Worker speichert die Binärdaten der neuen Darstellung in `rendition.path`.
1. Die Binärdaten, die in `rendition.path` geschrieben wurden, werden über das Asset compute SDK an den AEM Author-Dienst übertragen und als __(4a)__ eine Texterstellung und __(4b)__ auf dem Metadaten-Knoten des Assets beibehalten.

Das obige Diagramm zeigt die Bedenken der Asset compute-Entwickler und den logischen Fluss zum Asset compute-Worker-Aufruf. Für neugierig sind die [internen Details der Asset compute-Ausführung](https://docs.adobe.com/content/help/en/asset-compute/using/extend/custom-application-internals.html) verfügbar, es können jedoch nur die öffentlichen Asset compute SDK-API-Verträge angefordert werden.

## Anatomie eines Arbeitnehmers

Alle Asset compute-Arbeiter folgen der gleichen Grundstruktur und dem gleichen Input/Output-Vertrag.

```javascript
'use strict';

// Any npm module imports used by the worker
const { worker, SourceCorruptError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

/**
Exports the worker implemented by a custom rendition callback function, which parametrizes the input/output contract for the worker.
 + `source` represents the asset's original binary used as the input for the worker.
 + `rendition` represents the worker's output, which is the creation of a new asset rendition.
 + `params` are optional parameters, which map to additional key/value pairs, including a sub `auth` object that contains Adobe I/O access credentials.
**/
exports.main = worker(async (source, rendition, params) => {
    // Perform any necessary source (input) checks
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        // Throw appropriate errors whenever an erring condition is met
        throw new SourceCorruptError('source file is empty');
    }

    // Access any custom parameters provided via the Processing Profile configuration
    let param1 = rendition.instructions.exampleParam;

    /** 
    Perform all work needed to transform the source into the rendition.
    
    The source data can be accessed:
        + In the worker via a file available at `source.path`
        + Or via a presigned GET URL at `source.url`
    **/
    if (success) {
        // A successful worker must write some data back to `renditions.path`. 
        // This example performs a trivial 1:1 copy of the source binary to the rendition
        await fs.copyFile(source.path, rendition.path);
    } else {
        // Upon failure an Asset Compute Error (exported by @adobe/asset-compute-commons) should be thrown.
        throw new GenericError("An error occurred!", "example-worker");
    }
});

/**
Optionally create helper classes or functions the worker's rendition callback function invokes to help organize code.

Code shared across workers, or to complex to be managed in a single file, can be broken out across supporting JavaScript files in the project and imported normally into the worker. 
**/
function customHelperFunctions() { ... }
```

## Öffnen der Datei &quot;index.js&quot;

![Automatisch generierte index.js](./assets/worker/autogenerated-index-js.png)

1. Stellen Sie sicher, dass das Asset compute-Projekt im VS-Code geöffnet ist.
1. Navigieren Sie zum Ordner `/actions/worker`
1. Öffnen Sie die Datei `index.js`

Dies ist die JavaScript-Arbeitsdatei, die wir in diesem Lernprogramm ändern werden.

## Installieren und Importieren unterstützender NPM-Module

Da Node.js auf Asset compute-Projekten basiert, profitieren sie vom robusten Modul-Ökosystem [npm. ](https://npmjs.com) Um npm-Module nutzen zu können, müssen wir sie zunächst in unserem Asset compute-Projekt installieren.

In diesem Worker verwenden wir das Jimp [jimp](https://www.npmjs.com/package/jimp), um das Darstellungsbild direkt im Code Node.js zu erstellen und zu bearbeiten.

>[!WARNING]
>
>Nicht alle NPM-Module zur Asset-Manipulation werden von Asset compute unterstützt. npm-Module, die auf die Existenz von Anwendungen wie ImageMagick oder anderen betriebssystemabhängigen Bibliotheken angewiesen sind, werden nicht unterstützt. Es ist am besten, die Verwendung von NPM-Modulen mit JavaScript zu beschränken.

1. Öffnen Sie die Befehlszeile im Stammverzeichnis Ihres Asset compute-Projekts (dies kann über __Terminal > New Terminal__ im VS-Code erfolgen) und führen Sie den Befehl aus:

   ```
   $ npm install jimp
   ```

1. Importieren Sie das Modul `jimp` in den Arbeitscode, damit es über das JavaScript-Objekt `Jimp` verwendet werden kann.
Aktualisieren Sie die `require`-Direktiven am oberen Rand des Workers `index.js`, um das `Jimp`-Objekt aus dem `jimp`-Modul zu importieren:

   ```javascript
   'use strict';
   
   const { Jimp } = require('jimp');
   const { worker, SourceCorruptError } = require('@adobe/asset-compute-sdk');
   const fs = require('fs').promises;
   
   exports.main = worker(async (source, rendition, params) => {
       // Check handle a corrupt input source
       const stats = await fs.stat(source.path);
       if (stats.size === 0) {
           throw new SourceCorruptError('source file is empty');
       }
   
       // Do work here
   });
   ```

## Parameter lesen

asset compute-Workers können Parameter lesen, die über verarbeitende Profil, die in AEM als Cloud Service-Autorendienst definiert sind, übergeben werden können. Die Parameter werden über das `rendition.instructions`-Objekt an den Worker übergeben.

Diese können gelesen werden, indem Sie auf `rendition.instructions.<parameterName>` im Arbeitscode zugreifen.

Hier lesen wir die konfigurierbaren Darstellungen `SIZE`, `BRIGHTNESS` und `CONTRAST`, die Standardwerte angeben, wenn keine über das Profil &quot;Verarbeitung&quot;bereitgestellt wurde. Beachten Sie, dass `renditions.instructions` als Zeichenfolgen übergeben werden, wenn AEM als Cloud Service-Verarbeitungscode-Profile aufgerufen wird. Stellen Sie daher sicher, dass diese in die richtigen Datentypen im Arbeitscode umgewandelt werden.

```javascript
'use strict';

const { Jimp } = require('jimp');
const { worker, SourceCorruptError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

exports.main = worker(async (source, rendition, params) => {
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        throw new SourceCorruptError('source file is empty');
    }

    // Read in parameters and set defaults if parameters are provided
    // Processing Profiles pass in instructions as Strings, so make sure to parse to correct data types
    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    // Do work here
}
```

## Ablauffehler{#errors}

asset compute-Workers können auf Situationen stoßen, die zu Fehlern führen. Das Adobe Asset compute SDK stellt [eine Reihe vordefinierter Fehler](https://github.com/adobe/asset-compute-commons#asset-compute-errors) bereit, die bei Auftreten solcher Situationen ausgegeben werden können. Wenn kein spezifischer Fehlertyp angewendet wird, kann das `GenericError` verwendet werden oder es können spezifische `ClientErrors` benutzerdefinierte  definiert werden.

Bevor Sie mit der Verarbeitung der Darstellung beginnen, überprüfen Sie, ob alle Parameter gültig und im Kontext dieses Workers unterstützt sind:

+ Vergewissern Sie sich, dass die Parameter für die Darstellungsanweisung für `SIZE`, `CONTRAST` und `BRIGHTNESS` gültig sind. Andernfalls wird ein benutzerdefinierter Fehler `RenditionInstructionsError` ausgegeben.
   + Eine benutzerdefinierte `RenditionInstructionsError`-Klasse, die `ClientError` erweitert, wird am Ende dieser Datei definiert. Die Verwendung eines spezifischen, benutzerdefinierten Fehlers ist nützlich, wenn [Tests für den Arbeiter schreiben](../test-debug/test.md).

```javascript
'use strict';

const { Jimp } = require('jimp');
// Import the Asset Compute SDK provided `ClientError` 
const { worker, SourceCorruptError, ClientError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

exports.main = worker(async (source, rendition, params) => {
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        throw new SourceCorruptError('source file is empty');
    }

    // Read in parameters and set defaults if parameters are provided
    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    if (SIZE <= 10 || SIZE >= 10000) {
        // Ensure size is within allowable bounds
        throw new RenditionInstructionsError("'size' must be between 10 and 1,0000");
    } else if (CONTRAST <= -1 || CONTRAST >= 1) {
        // Ensure contrast is valid value
        throw new RenditionInstructionsError("'contrast' must between -1 and 1");
    } else if (BRIGHTNESS <= -1 || BRIGHTNESS >= 1) {
        // Ensure contrast is valid value
        throw new RenditionInstructionsError("'brightness' must between -1 and 1");
    }

    // Do work here
}

// Create a new ClientError to handle invalid rendition.instructions values
class RenditionInstructionsError extends ClientError {
    constructor(message) {
        // Provide a:
        // + message: describing the nature of this erring condition
        // + name: the name of the error; usually same as class name
        // + reason: a short, searchable, unique error token that identifies this error
        super(message, "RenditionInstructionsError", "rendition_instructions_error");

        // Capture the strack trace
        Error.captureStackTrace(this, RenditionInstructionsError);
    }
}
```

## Erstellen der Darstellung

Wenn die Parameter gelesen, bereinigt und validiert werden, wird Code geschrieben, um die Darstellung zu generieren. Der Pseudo-Code für die Generierung der Darstellung lautet wie folgt:

1. Erstellen Sie eine neue Arbeitsfläche `renditionImage` in quadratischen Dimensionen, die über den Parameter `size` angegeben wird.
1. `image`-Objekt aus der Binärdatei des Quellassets erstellen
1. Verwenden Sie die Bibliothek __Jimp__, um das Bild zu transformieren:
   + Zuschneiden des Originalbilds auf ein zentriertes Quadrat
   + Kreis aus der Mitte des &quot;quadrierten&quot; Bilds ausschneiden
   + Passend zu den durch den Parameterwert `SIZE` definierten Dimensionen skalieren
   + Anpassen des Kontrasts basierend auf dem Parameterwert `CONTRAST`
   + Helligkeit basierend auf dem Parameterwert `BRIGHTNESS` anpassen
1. Platzieren Sie das transformierte `image` in die Mitte des `renditionImage`, das einen transparenten Hintergrund hat.
1. Schreiben Sie das zusammengesetzte Element `renditionImage` in `rendition.path`, damit es als Asset-Darstellung AEM gespeichert werden kann.

Dieser Code verwendet die [Jimp-APIs](https://github.com/oliver-moran/jimp#jimp), um diese Bildtransformationen durchzuführen.

asset compute-Arbeiter müssen ihre Arbeit synchron beenden, und `rendition.path` muss vollständig zurückgeschrieben werden, bevor `renditionCallback` des Arbeitnehmers abgeschlossen ist. Dies erfordert, dass asynchrone Funktionsaufrufe mit dem Operator `await` synchron ausgeführt werden. Wenn Sie mit den asynchronen JavaScript-Funktionen nicht vertraut sind und wie diese synchron ausgeführt werden sollen, machen Sie sich mit dem Warten-Operator [JavaScript vertraut.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)

Der fertige Worker `index.js` sollte wie folgt aussehen:

```javascript
'use strict';

const Jimp = require('jimp');
const { worker, SourceCorruptError, ClientError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

exports.main = worker(async (source, rendition, params) => {
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        throw new SourceCorruptError('source file is empty');
    }

    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    if (SIZE <= 10 || SIZE >= 10000) {
        throw new RenditionInstructionsError("'size' must be between 10 and 10,000");
    } else if (CONTRAST <= -1 || CONTRAST >= 1) {
        throw new RenditionInstructionsError("'contrast' must between -1 and 1");
    } else if (BRIGHTNESS <= -1 || BRIGHTNESS >= 1) {
        throw new RenditionInstructionsError("'brightness' must between -1 and 1");
    }

    // Create target rendition image of the target size with a transparent background (0x0)
    let renditionImage =  new Jimp(SIZE, SIZE, 0x0);

    // Read and perform transformations on the source binary image
    let image = await Jimp.read(source.path);

    // Crop a circle from the source asset, and then apply contrast and brightness using Jimp
    image.crop(
            image.bitmap.width < image.bitmap.height ? 0 : (image.bitmap.width - image.bitmap.height) / 2,
            image.bitmap.width < image.bitmap.height ? (image.bitmap.height - image.bitmap.width) / 2 : 0,
            image.bitmap.width < image.bitmap.height ? image.bitmap.width : image.bitmap.height,
            image.bitmap.width < image.bitmap.height ? image.bitmap.width : image.bitmap.height
        )   
        .circle()
        .scaleToFit(SIZE, SIZE)
        .contrast(CONTRAST)
        .brightness(BRIGHTNESS);

    // Place the transformed image onto the transparent renditionImage to save as PNG
    renditionImage.composite(image, 0, 0)

    // Write the final transformed image to the asset's rendition
    renditionImage.write(rendition.path);
});

// Custom error used for renditions.instructions parameter checking
class RenditionInstructionsError extends ClientError {
    constructor(message) {
        super(message, "RenditionInstructionsError", "rendition_instructions_error");
        Error.captureStackTrace(this, RenditionInstructionsError);
    }
}
```

## Ausführen des Arbeitnehmers

Nachdem der Worker-Code abgeschlossen ist und zuvor im [manifest.yml](./manifest.md) registriert und konfiguriert wurde, kann er mit dem lokalen Asset compute Development Tool ausgeführt werden, um die Ergebnisse anzuzeigen.

1. Aus der Wurzel des Asset compute-Projekts
1. Starte `aio app run`
1. Warten Sie, bis das Asset compute Development Tool in einem neuen Fenster geöffnet wird
1. Wählen Sie im Ordner __eine Datei aus...__ Dropdown-Liste wählen Sie ein zu verarbeitendes Beispielbild aus
   + Wählen Sie eine Beispielbilddatei aus, die als Quellelement-Binärdatei verwendet werden soll
   + Wenn noch keine vorhanden ist, tippen Sie auf die __(+)__-Datei nach links, laden Sie ein [Beispielbild](../assets/samples/sample-file.jpg) hoch und aktualisieren Sie das Browserfenster der Entwicklungstools
1. Aktualisieren Sie `"name": "rendition.png"` als dieser Worker, um eine transparente PNG-Datei zu erstellen.
   + Beachten Sie, dass dieser Parameter &quot;name&quot;nur für das Entwicklungstool verwendet wird und nicht darauf angewiesen sein sollte.

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.png"
           }
       ]
   }
   ```
1. Tippen Sie auf __Ausführen__ und warten Sie, bis die Darstellung generiert wird.
1. Im Abschnitt __Ausgabeformate__ wird die generierte Darstellung Vorschau. Tippen Sie auf die Vorschau Darstellung, um die vollständige Darstellung herunterzuladen

   ![Standard-PNG-Darstellung](./assets/worker/default-rendition.png)

### Ausführen des Workers mit Parametern

Parameter, die über Konfigurationen von Verarbeitungsparametern weitergegeben werden, können in Asset compute Development Tools simuliert werden, indem sie als Schlüssel/Wert-Paare im Darstellungsparameter JSON bereitgestellt werden.

>[!WARNING]
>
>Während der lokalen Entwicklung können Werte mit verschiedenen Datentypen weitergegeben werden, wenn sie von AEM als Cloud Service-Verarbeitungselemente als Zeichenfolgen übergeben werden. Achten Sie daher darauf, dass bei Bedarf die richtigen Datentypen analysiert werden.
> Zum Beispiel erfordert die Funktion `crop(width, height)` von Jimp, dass ihre Parameter `int`&#39;s sind. Wenn `parseInt(rendition.instructions.size)` nicht mit einer int analysiert wird, schlägt der Aufruf von `jimp.crop(SIZE, SIZE)` fehl, da die Parameter inkompatiblen &quot;String&quot;-Typ sind.

Unser Code akzeptiert Parameter für:

+ `size` definiert die Größe der Darstellung (Höhe und Breite als Ganzzahlen)
+ `contrast` definiert die Kontrasteinstellung, muss zwischen -1 und 1 liegen, als Floats
+ `brightness`  definiert die helle Einstellung, muss zwischen -1 und 1 liegen, als Floats

Diese werden im Worker `index.js` gelesen über:

+ `const SIZE = parseInt(rendition.instructions.size) || 800`
+ `const CONTRAST = parseFloat(rendition.instructions.contrast) || 0`
+ `const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0`

1. Aktualisieren Sie die Darstellungsparameter, um Größe, Kontrast und Helligkeit anzupassen.

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.png",
               "size": "450",
               "contrast": "0.30",
               "brightness": "0.15"
           }
       ]
   }
   ```

1. Tippen Sie erneut auf __Ausführen__
1. Tippen Sie auf die Vorschau Darstellung, um die generierte Darstellung herunterzuladen und zu überprüfen. Beachten Sie die Abmessungen und die Art und Weise, wie Kontrast und Helligkeit im Vergleich zur Standarddarstellung verändert wurden.

   ![Parametrisierte PNG-Darstellung](./assets/worker/parameterized-rendition.png)

1. Laden Sie andere Bilder in das Dropdown-Feld __Quelldatei__ hoch und versuchen Sie, den Worker mit anderen Parametern auszuführen!

## Worker index.js auf Github

Das endgültige `index.js` ist auf Github verfügbar unter:

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Fehlerbehebung

+ [Ausgabe teilweise gezeichnet/beschädigt zurückgegeben](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)

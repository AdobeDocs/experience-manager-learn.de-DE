---
title: Entwickeln eines Asset compute-Sekundärs
description: asset compute-Sekundäre bilden den Kern eines Asset compute-Projekts, da sie benutzerdefinierte Funktionen bereitstellen, mit denen die an einem Asset durchgeführten Arbeiten zur Erstellung einer neuen Ausgabedarstellung ausgeführt oder orchestriert werden.
feature: asset compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6282
thumbnail: KT-6282.jpg
topic: Integrationen, Entwicklung
role: Developer
level: Intermediate, Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1424'
ht-degree: 0%

---


# Entwickeln eines Asset compute-Sekundärs

asset compute-Sekundäre bilden den Kern eines Asset compute-Projekts, da sie benutzerdefinierte Funktionen bereitstellen, mit denen die an einem Asset durchgeführten Arbeiten zur Erstellung einer neuen Ausgabedarstellung ausgeführt oder orchestriert werden.

Das Asset compute-Projekt generiert automatisch einen einfachen Worker, der die ursprüngliche Binärdatei des Assets in eine benannte Ausgabedarstellung kopiert, ohne dass Konvertierungen vorgenommen werden müssen. In diesem Tutorial werden wir diesen Worker ändern, um eine interessantere Ausgabedarstellung zu erstellen, um die Macht von Asset compute-Arbeitern zu veranschaulichen.

Wir erstellen einen Asset compute Worker, der eine neue horizontale Bilddarstellung generiert, die leeren Raum links und rechts neben der Asset-Ausgabedarstellung mit einer verschwommenen Version des Assets abdeckt. Die Breite, Höhe und Unschärfe der endgültigen Ausgabedarstellung werden parametrisiert.

## Logischer Ablauf eines Asset compute Worker-Aufrufs

asset compute-Worker implementieren den Asset compute SDK Worker API-Vertrag in der `renditionCallback(...)`-Funktion, die konzeptionell lautet:

+ __Eingabe:__ Die ursprünglichen binären Parameter und Verarbeitungsprofil eines AEM Assets
+ __Ausgabe:__ Mindestens eine Ausgabedarstellung, die dem AEM Asset hinzugefügt werden soll

![Logischer asset compute Worker-Fluss](./assets/worker/logical-flow.png)

1. Der AEM-Autorendienst ruft den Asset compute Worker auf und stellt den Parameter __(1a)__ der ursprünglichen Binärdatei (`source` Parameter) des Assets sowie __(1b)__ alle im Verarbeitungsprofil definierten Parameter (`rendition.instructions` Parameter) bereit.
1. Das Asset compute-SDK orchestriert die Ausführung der `renditionCallback(...)`-Funktion des benutzerdefinierten Asset compute-Metadaten-Sekundärs und generiert eine neue binäre Ausgabedarstellung, basierend auf der ursprünglichen Binärdatei des Assets __(1a)__ und allen Parametern __(1b)__.

   + In diesem Tutorial wird die Ausgabedarstellung &quot;in Bearbeitung&quot;erstellt, d. h. der Worker stellt die Ausgabedarstellung zusammen, die Quellbinäre kann jedoch auch zur Ausgabedarstellungsgenerierung an andere Webdienst-APIs gesendet werden.

1. Der Asset compute Worker speichert die Binärdaten der neuen Ausgabedarstellung in `rendition.path`.
1. Die Binärdaten, die in `rendition.path` geschrieben wurden, werden über das Asset compute-SDK an den AEM-Autorendienst übertragen und als __(4a)__ Textausgabe und __(4b)__ im Metadatenknoten des Assets gespeichert.

Das obige Diagramm zeigt die Asset compute-Entwickleranliegen und den logischen Fluss zum Asset compute Worker-Aufruf. Für das Merkwürdige sind die [internen Details der Asset compute-Ausführung](https://experienceleague.adobe.com/docs/asset-compute/using/extend/custom-application-internals.html) verfügbar, es können jedoch nur die öffentlichen Asset compute SDK-API-Verträge angefordert werden.

## Anatomie eines Arbeitnehmers

Alle Asset compute-Workers folgen der gleichen Grundstruktur und dem gleichen Eingabe-/Ausgabevertrag.

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

## Öffnen der Worker-Datei index.js

![Automatisch generierte index.js](./assets/worker/autogenerated-index-js.png)

1. Stellen Sie sicher, dass das Asset compute-Projekt in VS Code geöffnet ist.
1. Navigieren Sie zum Ordner `/actions/worker` .
1. Öffnen Sie die Datei `index.js` .

Dies ist die JavaScript-Worker-Datei, die wir in diesem Tutorial ändern werden.

## Installieren und Importieren unterstützender NPM-Module

Da Asset compute-Projekte auf Node.js basieren, profitieren sie vom robusten [npm-Modulsystem](https://npmjs.com). Um npm-Module zu nutzen, müssen wir sie zunächst in unserem Asset compute-Projekt installieren.

In diesem Worker verwenden wir [jimp](https://www.npmjs.com/package/jimp), um das Ausgabedarstellungsbild direkt im Code von Node.js zu erstellen und zu bearbeiten.

>[!WARNING]
>
>Nicht alle npm-Module für die Asset-Manipulation werden von Asset compute unterstützt. npm-Module, die auf die Existenz von Anwendungen wie ImageMagick oder anderen betriebssystemabhängigen Bibliotheken angewiesen sind, werden nicht unterstützt. Es empfiehlt sich, die Verwendung von reinen JavaScript-npm-Modulen zu beschränken.

1. Öffnen Sie die Befehlszeile im Stammverzeichnis Ihres Asset compute-Projekts (dies kann im VS-Code über __Terminal > Neues Terminal__ durchgeführt werden) und führen Sie den Befehl aus:

   ```
   $ npm install jimp
   ```

1. Importieren Sie das Modul `jimp` in den Worker-Code, damit es über das JavaScript-Objekt `Jimp` verwendet werden kann.
Aktualisieren Sie die `require`-Direktiven oben im `index.js` des Sekundärs, um das `Jimp`-Objekt vom `jimp`-Modul zu importieren:

   ```javascript
   'use strict';
   
   const Jimp = require('jimp');
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

asset compute-Sekundäre können Parameter lesen, die über Verarbeitungsprofile übergeben werden können, die in AEM als Cloud Service-Autorendienst definiert sind. Die Parameter werden über das `rendition.instructions` -Objekt an den Worker übergeben.

Diese können gelesen werden, indem Sie auf `rendition.instructions.<parameterName>` im Worker-Code zugreifen.

Hier lesen wir die Konfigurations-Ausgabeformate `SIZE`, `BRIGHTNESS` und `CONTRAST` und geben Standardwerte an, wenn keine über das Verarbeitungsprofil bereitgestellt wurden. Beachten Sie, dass `renditions.instructions` als Zeichenfolgen übergeben werden, wenn von AEM als Cloud Service-Verarbeitungsprofile aufgerufen wird. Stellen Sie daher sicher, dass sie in die richtigen Datentypen im Worker-Code umgewandelt werden.

```javascript
'use strict';

const Jimp = require('jimp');
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

## Zurückgeben von Fehlern{#errors}

asset compute-Workers können auf Fehlersituationen stoßen. Das Adobe Asset compute SDK stellt [eine Suite vordefinierter Fehler](https://github.com/adobe/asset-compute-commons#asset-compute-errors) bereit, die bei Auftreten solcher Situationen ausgegeben werden können. Wenn kein bestimmter Fehlertyp zutrifft, kann `GenericError` verwendet oder ein spezifischer benutzerdefinierter `ClientErrors`-Typ definiert werden.

Bevor Sie mit der Verarbeitung der Ausgabedarstellung beginnen, überprüfen Sie, ob alle Parameter im Kontext dieses Sekundärs gültig und unterstützt sind:

+ Stellen Sie sicher, dass die Parameter für die Ausgabedarstellungsanweisungen für `SIZE`, `CONTRAST` und `BRIGHTNESS` gültig sind. Wenn nicht, geben Sie einen benutzerdefinierten Fehler `RenditionInstructionsError` aus.
   + Eine benutzerdefinierte `RenditionInstructionsError`-Klasse, die `ClientError` erweitert, wird am unteren Rand dieser Datei definiert. Die Verwendung eines bestimmten, benutzerdefinierten Fehlers ist nützlich, wenn [Tests](../test-debug/test.md) für den Worker geschrieben werden.

```javascript
'use strict';

const Jimp = require('jimp');
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

## Erstellen der Ausgabedarstellung

Mit den gelesenen, bereinigten und validierten Parametern wird Code geschrieben, um die Ausgabedarstellung zu generieren. Der Pseudo-Code für die Generierung von Ausgabedarstellungen lautet wie folgt:

1. Erstellen Sie eine neue Arbeitsfläche `renditionImage` in quadratischen Dimensionen, die über den Parameter `size` angegeben wird.
1. Erstellen Sie ein `image`-Objekt aus der Binärdatei des Quell-Assets
1. Verwenden Sie die Bibliothek __Jimp__ , um das Bild umzuwandeln:
   + Zuschneiden des Originalbilds auf ein zentriertes Quadrat
   + Kreis aus der Mitte des &quot;Quadrats&quot;-Bildes ausschneiden
   + Skalierung, um in die Dimensionen zu passen, die durch den Parameterwert `SIZE` definiert wurden
   + Passen Sie den Kontrast basierend auf dem Parameterwert `CONTRAST` an.
   + Helligkeit basierend auf dem Parameterwert `BRIGHTNESS` anpassen
1. Platzieren Sie das umgewandelte `image` in die Mitte des `renditionImage` mit transparentem Hintergrund.
1. Schreiben Sie das zusammengestellte `renditionImage` in `rendition.path`, damit es als Asset-Ausgabedarstellung wieder in AEM gespeichert werden kann.

Dieser Code verwendet die [Jimp-APIs](https://github.com/oliver-moran/jimp#jimp), um diese Bildtransformationen durchzuführen.

asset compute-Sekundäre müssen ihre Arbeit synchron abschließen und `rendition.path` müssen vollständig zurück in geschrieben werden, bevor die `renditionCallback` des Arbeiters abgeschlossen ist. Dies erfordert, dass asynchrone Funktionsaufrufe mit dem Operator `await` synchron ausgeführt werden. Wenn Sie nicht mit asynchronen JavaScript-Funktionen vertraut sind und wissen, wie diese synchron ausgeführt werden können, machen Sie sich mit dem Warten-Operator [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await) vertraut.

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

    // Read/parse and validate parameters
    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    if (SIZE <= 10 || SIZE >= 10000) {
        throw new RenditionInstructionsError("'size' must be between 10 and 1,0000");
    } else if (CONTRAST <= -1 || CONTRAST >= 1) {
        throw new RenditionInstructionsError("'contrast' must between -1 and 1");
    } else if (BRIGHTNESS <= -1 || BRIGHTNESS >= 1) {
        throw new RenditionInstructionsError("'brightness' must between -1 and 1");
    }

    // Create target rendition image 
    let renditionImage =  new Jimp(SIZE, SIZE, 0x0);

    // Read and perform transformations on the source binary image
    let image = await Jimp.read(source.path);

    // Crop a circle from the source asset, and then apply contrast and brightness
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
    await renditionImage.writeAsync(rendition.path);
});

// Custom error used for renditions.instructions parameter checking
class RenditionInstructionsError extends ClientError {
    constructor(message) {
        super(message, "RenditionInstructionsError", "rendition_instructions_error");
        Error.captureStackTrace(this, RenditionInstructionsError);
    }
}
```

## Laufen des Sekundärs

Nachdem der Worker-Code abgeschlossen ist und zuvor im [manifest.yml](./manifest.md) registriert und konfiguriert wurde, kann er mit dem lokalen Asset compute Development Tool ausgeführt werden, um die Ergebnisse anzuzeigen.

1. Aus dem Stammverzeichnis des Asset compute-Projekts
1. Starte `aio app run`
1. Warten Sie, bis das Asset compute Development Tool in einem neuen Fenster geöffnet wird
1. Wählen Sie im __Datei auswählen...__ Dropdown-Liste ein Beispielbild für die Verarbeitung auswählen
   + Auswählen einer Beispielbilddatei zur Verwendung als Quell-Asset-Binärdatei
   + Wenn noch keine vorhanden sind, tippen Sie auf die Datei __(+)__ links, laden Sie ein [Beispielbild](../assets/samples/sample-file.jpg) hoch und aktualisieren Sie das Browserfenster der Entwicklungs-Tools.
1. Aktualisieren Sie `"name": "rendition.png"` als dieses Sekundärprogramm, um ein transparentes PNG zu generieren.
   + Beachten Sie, dass dieser &quot;name&quot;-Parameter nur für das Entwicklungstool verwendet wird und sich nicht auf ihn verlassen sollte.

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

1. Tippen Sie auf __Ausführen__ und warten Sie, bis die Ausgabedarstellung generiert wird.
1. Im Abschnitt __Ausgabeformate__ wird eine Vorschau der generierten Ausgabedarstellung angezeigt. Tippen Sie auf die Vorschau der Ausgabedarstellung, um die vollständige Ausgabedarstellung herunterzuladen.

   ![PNG-Standardausgabe](./assets/worker/default-rendition.png)

### Ausführen des Sekundärs mit Parametern

Parameter, die über Verarbeitungsprofilkonfigurationen übergeben werden, können in Asset compute Development Tools simuliert werden, indem sie als Schlüssel/Wert-Paare im JSON-Ausgabeparameter bereitgestellt werden.

>[!WARNING]
>
>Während der lokalen Entwicklung können Werte mithilfe verschiedener Datentypen übergeben werden, wenn sie von AEM als Cloud Service-Verarbeitungsprofile als Zeichenfolgen übergeben werden. Achten Sie daher darauf, bei Bedarf die richtigen Datentypen zu analysieren.
> Beispielsweise erfordert die Funktion `crop(width, height)` von Jimp, dass die Parameter `int` sind. Wenn `parseInt(rendition.instructions.size)` nicht auf ein int geparst wird, schlägt der Aufruf von `jimp.crop(SIZE, SIZE)` fehl, da die Parameter den inkompatiblen Typ &quot;String&quot;aufweisen.

Unser Code akzeptiert Parameter für:

+ `size` definiert die Größe der Ausgabedarstellung (Höhe und Breite als Ganzzahlen)
+ `contrast` definiert die Kontrastanpassung, muss zwischen -1 und 1 liegen, da Floats verwendet werden
+ `brightness`  definiert die helle Einstellung, muss zwischen -1 und 1 liegen, da Floats verwendet werden

Diese werden im Worker `index.js` über Folgendes gelesen:

+ `const SIZE = parseInt(rendition.instructions.size) || 800`
+ `const CONTRAST = parseFloat(rendition.instructions.contrast) || 0`
+ `const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0`

1. Aktualisieren Sie die Ausgabeparameter, um Größe, Kontrast und Helligkeit anzupassen.

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

1. Tippen Sie erneut auf __Ausführen__ .
1. Tippen Sie auf die Vorschau der Ausgabedarstellung, um die generierte Ausgabedarstellung herunterzuladen und zu überprüfen. Beachten Sie die Dimensionen und wie Kontrast und Helligkeit im Vergleich zur Standarddarstellung geändert wurden.

   ![Parametrisierte PNG-Wiedergabe](./assets/worker/parameterized-rendition.png)

1. Laden Sie andere Bilder in das Dropdown-Menü __Quelldatei__ hoch und versuchen Sie, den Worker mit verschiedenen Parametern auszuführen!

## Worker index.js auf Github

Das endgültige `index.js` ist auf Github verfügbar unter:

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Fehlerbehebung

+ [Wiedergabe teilweise gezeichnet/beschädigt zurückgegeben](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)

---
title: Entwickeln eines Asset compute-Sekundärs
description: asset compute-Sekundäre bilden den Kern eines Asset compute-Projekts, da sie benutzerdefinierte Funktionen bereitstellen, mit denen die an einem Asset durchgeführten Arbeiten zur Erstellung einer neuen Ausgabedarstellung ausgeführt oder orchestriert werden.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6282
thumbnail: KT-6282.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 7d51ec77-c785-4b89-b717-ff9060d8bda7
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1416'
ht-degree: 0%

---

# Entwickeln eines Asset compute-Sekundärs

asset compute-Sekundäre bilden den Kern eines Asset compute-Projekts, da sie benutzerdefinierte Funktionen bereitstellen, mit denen die an einem Asset durchgeführten Arbeiten zur Erstellung einer neuen Ausgabedarstellung ausgeführt oder orchestriert werden.

Das Asset compute-Projekt generiert automatisch einen einfachen Worker, der die ursprüngliche Binärdatei des Assets in eine benannte Ausgabedarstellung kopiert, ohne dass Konvertierungen vorgenommen werden müssen. In diesem Tutorial werden wir diesen Worker ändern, um eine interessantere Ausgabedarstellung zu erstellen, um die Macht von Asset compute-Arbeitern zu veranschaulichen.

Wir erstellen einen Asset compute Worker, der eine neue horizontale Bilddarstellung generiert, die leeren Raum links und rechts neben der Asset-Ausgabedarstellung mit einer verschwommenen Version des Assets abdeckt. Die Breite, Höhe und Unschärfe der endgültigen Ausgabedarstellung werden parametrisiert.

## Logischer Ablauf eines Asset compute Worker-Aufrufs

asset compute-Sekundäre implementieren den Asset compute SDK Worker API-Vertrag im `renditionCallback(...)` -Funktion, die konzeptionell lautet:

+ __Eingabe:__ Die ursprünglichen binären Parameter und Verarbeitungsprofil eines AEM Assets
+ __Ausgabe:__ Mindestens eine Ausgabedarstellung, die dem AEM Asset hinzugefügt werden soll

![Logischer asset compute Worker-Fluss](./assets/worker/logical-flow.png)

1. Der AEM-Autorendienst ruft den Asset compute Worker auf und stellt die __Absatz 1a)__ original binary (`source` Parameter) und __(1b)__ alle im Verarbeitungsprofil definierten Parameter (`rendition.instructions` -Parameter).
1. Das Asset compute SDK orchestriert die Ausführung des benutzerdefinierten Asset compute-Metadaten-Sekundärs `renditionCallback(...)` Funktion, die eine neue binäre Ausgabedarstellung generiert, die auf der ursprünglichen Binärdatei des Assets basiert __Absatz 1a)__ und beliebigen Parametern __(1b)__.

   + In diesem Tutorial wird die Ausgabedarstellung &quot;in Bearbeitung&quot;erstellt, d. h. der Worker stellt die Ausgabedarstellung zusammen, die Quellbinäre kann jedoch auch zur Ausgabedarstellungsgenerierung an andere Webdienst-APIs gesendet werden.

1. Der Asset compute Worker speichert die Binärdaten der neuen Ausgabedarstellung in `rendition.path`.
1. Die Binärdaten, die in `rendition.path` wird über das Asset compute SDK an den AEM-Autorendienst übertragen und als __(4a)__ eine Textausgabe und __(4b)__ im Metadatenknoten des Assets beibehalten.

Das obige Diagramm zeigt die Asset compute-Entwickleranliegen und den logischen Fluss zum Asset compute Worker-Aufruf. Für die Neugierigen wird die [interne Details der Asset compute-Ausführung](https://experienceleague.adobe.com/docs/asset-compute/using/extend/custom-application-internals.html) verfügbar sind, können jedoch nur die öffentlichen Asset compute SDK-API-Verträge abhängig gemacht werden.

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
1. Navigieren Sie zum `/actions/worker` Ordner
1. Öffnen Sie die `index.js` file

Dies ist die JavaScript-Worker-Datei, die wir in diesem Tutorial ändern werden.

## Installieren und Importieren unterstützender NPM-Module

Da Asset compute-Projekte auf Node.js basieren, profitieren sie von der robusten [npm-Modul-Ökosystem](https://npmjs.com). Um npm-Module zu nutzen, müssen wir sie zunächst in unserem Asset compute-Projekt installieren.

In diesem Worker nutzen wir die [jimp](https://www.npmjs.com/package/jimp) , um das Ausgabedarstellungsbild direkt im Code von Node.js zu erstellen und zu bearbeiten.

>[!WARNING]
>
>Nicht alle npm-Module für die Asset-Manipulation werden von Asset compute unterstützt. npm-Module, die auf die Existenz von Anwendungen wie ImageMagick oder anderen betriebssystemabhängigen Bibliotheken angewiesen sind, werden nicht unterstützt. Es empfiehlt sich, die Verwendung von reinen JavaScript-npm-Modulen zu beschränken.

1. Öffnen Sie die Befehlszeile im Stammverzeichnis Ihres Asset compute-Projekts (dies kann im VS-Code über durchgeführt werden). __Terminal > Neues Terminal__) und führen Sie den Befehl aus:

   ```
   $ npm install jimp
   ```

1. Importieren Sie die `jimp` -Modul in den Worker-Code ein, damit er über die `Jimp` JavaScript-Objekt.
Aktualisieren Sie die `require` Direktiven am Anfang des `index.js` , um `Jimp` -Objekt aus `jimp` -Modul:

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

asset compute-Sekundäre können Parameter lesen, die über Verarbeitungsprofile übergeben werden können, die im as a Cloud Service Autorendienst AEM definiert sind. Die Parameter werden über die `rendition.instructions` -Objekt.

Diese können gelesen werden, indem Sie auf `rendition.instructions.<parameterName>` im Worker-Code.

Hier lesen wir im `SIZE`, `BRIGHTNESS` und `CONTRAST`, die Standardwerte angibt, wenn keine über das Verarbeitungsprofil bereitgestellt wurden. Beachten Sie Folgendes: `renditions.instructions` werden als Zeichenfolgen übergeben, wenn über AEM as a Cloud Service Verarbeitungsprofile aufgerufen wird. Stellen Sie daher sicher, dass sie in die richtigen Datentypen im Worker-Code umgewandelt werden.

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

asset compute-Workers können auf Fehlersituationen stoßen. Das Adobe Asset compute SDK bietet [eine Suite vordefinierter Fehler](https://github.com/adobe/asset-compute-commons#asset-compute-errors) die bei solchen Situationen ausgelöst werden können. Wenn kein bestimmter Fehlertyp zutrifft, wird die `GenericError` kann verwendet werden oder spezifische benutzerdefinierte `ClientErrors` definiert werden.

Bevor Sie mit der Verarbeitung der Ausgabedarstellung beginnen, überprüfen Sie, ob alle Parameter im Kontext dieses Sekundärs gültig und unterstützt sind:

+ Stellen Sie die Ausgabeformate-Anweisungsparameter für sicher. `SIZE`, `CONTRAST`und `BRIGHTNESS` gültig sind. Wenn nicht, wird ein benutzerdefinierter Fehler ausgegeben `RenditionInstructionsError`.
   + Benutzerdefiniert `RenditionInstructionsError` -Klasse, die erweitert `ClientError` wird am Ende dieser Datei definiert. Die Verwendung eines spezifischen, benutzerdefinierten Fehlers ist nützlich, wenn [Schreibtests](../test-debug/test.md) für den Arbeitnehmer.

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

1. Erstellen Sie eine neue `renditionImage` Arbeitsfläche in quadratischen Dimensionen, die über die `size` Parameter.
1. Erstellen Sie eine `image` -Objekt aus der Binärdatei des Quell-Assets
1. Verwenden Sie die __Jimp__ -Bibliothek, um das Bild zu transformieren:
   + Zuschneiden des Originalbilds auf ein zentriertes Quadrat
   + Kreis aus der Mitte des &quot;Quadrats&quot;-Bildes ausschneiden
   + Skalierung zur Anpassung an die Abmessungen, die durch die `SIZE` Parameterwert
   + Passen Sie den Kontrast basierend auf der `CONTRAST` Parameterwert
   + Passen Sie die Helligkeit basierend auf der `BRIGHTNESS` Parameterwert
1. Platzieren Sie die umgewandelte `image` in die Mitte des `renditionImage` mit transparentem Hintergrund
1. Schreiben Sie die Komposition, `renditionImage` nach `rendition.path` sodass sie als Asset-Ausgabedarstellung wieder in AEM gespeichert werden kann.

Dieser Code verwendet die [Jimp-APIs](https://github.com/oliver-moran/jimp#jimp) um diese Bildtransformationen durchzuführen.

asset compute-Sekundäre müssen ihre Arbeit synchron beenden und die `rendition.path` vor dem `renditionCallback` abgeschlossen. Dies erfordert, dass asynchrone Funktionsaufrufe mit der `await` Operator. Wenn Sie nicht mit asynchronen JavaScript-Funktionen vertraut sind und wissen, wie diese synchron ausgeführt werden können, sollten Sie sich mit [Warten-Operator von JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await).

Der Endarbeiter `index.js` sollte wie folgt aussehen:

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

Nachdem der Worker-Code abgeschlossen ist und zuvor im [manifest.yml](./manifest.md), kann es mit dem lokalen Asset compute-Entwicklungstool ausgeführt werden, um die Ergebnisse anzuzeigen.

1. Aus dem Stammverzeichnis des Asset compute-Projekts
1. Starte `aio app run`
1. Warten Sie, bis das Asset compute Development Tool in einem neuen Fenster geöffnet wird
1. Im __Datei auswählen...__ Dropdown-Liste ein Beispielbild für die Verarbeitung auswählen
   + Auswählen einer Beispielbilddatei zur Verwendung als Quell-Asset-Binärdatei
   + Wenn noch keine vorhanden sind, tippen Sie auf die __(+)__ auf die linke Seite klicken und eine [Beispielbild](../assets/samples/sample-file.jpg) und aktualisieren Sie das Browserfenster der Entwicklungs-Tools.
1. Aktualisieren `"name": "rendition.png"` als dieser Arbeitnehmer ein transparentes PNG generiert.
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

1. Tippen __Ausführen__ und warten, bis die Ausgabedarstellung generiert wird
1. Die __Ausgabeformate__ zeigt eine Vorschau der generierten Ausgabedarstellung an. Tippen Sie auf die Vorschau der Ausgabedarstellung, um die vollständige Ausgabedarstellung herunterzuladen.

   ![PNG-Standardausgabe](./assets/worker/default-rendition.png)

### Ausführen des Sekundärs mit Parametern

Parameter, die über Verarbeitungsprofilkonfigurationen übergeben werden, können in Asset compute Development Tools simuliert werden, indem sie als Schlüssel/Wert-Paare im JSON-Ausgabeparameter bereitgestellt werden.

>[!WARNING]
>
>Während der lokalen Entwicklung können Werte mithilfe verschiedener Datentypen übergeben werden, wenn sie von AEM als Cloud Service-Verarbeitungsprofile als Zeichenfolgen übergeben werden. Achten Sie daher darauf, bei Bedarf die richtigen Datentypen zu analysieren.
> Beispiel: Jimp&#39;s `crop(width, height)` -Funktion erfordert, dass ihre Parameter `int`&quot;s. Wenn `parseInt(rendition.instructions.size)` nicht auf ein int geparst wird, wird der Aufruf von `jimp.crop(SIZE, SIZE)` schlägt fehl, da die Parameter den inkompatiblen Typ &quot;String&quot;aufweisen.

Unser Code akzeptiert Parameter für:

+ `size` definiert die Größe der Ausgabedarstellung (Höhe und Breite als Ganzzahlen)
+ `contrast` definiert die Kontrastanpassung, muss zwischen -1 und 1 liegen, da Floats verwendet werden
+ `brightness`  definiert die helle Einstellung, muss zwischen -1 und 1 liegen, da Floats verwendet werden

Diese werden im Worker gelesen `index.js` über:

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

1. Tippen __Ausführen__ repeat
1. Tippen Sie auf die Vorschau der Ausgabedarstellung, um die generierte Ausgabedarstellung herunterzuladen und zu überprüfen. Beachten Sie die Dimensionen und wie Kontrast und Helligkeit im Vergleich zur Standarddarstellung geändert wurden.

   ![Parametrisierte PNG-Wiedergabe](./assets/worker/parameterized-rendition.png)

1. Hochladen anderer Bilder in die __Quelldatei__ und versuchen Sie, den Worker mit verschiedenen Parametern gegen sie auszuführen!

## Worker index.js auf Github

Das endgültige `index.js` ist auf Github verfügbar unter:

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Fehlerbehebung

+ [Wiedergabe teilweise gezeichnet/beschädigt zurückgegeben](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)

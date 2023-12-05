---
title: Entwickeln eines Asset Compute-Sekundärs
description: Asset Compute-Sekundäre bilden den Kern eines Asset Compute-Projekts, da sie eine benutzerdefinierte Funktion bereitstellen, mit der die an einem Asset durchgeführten Arbeiten zur Erstellung einer neuen Ausgabedarstellung ausgeführt oder orchestriert werden.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6282
thumbnail: KT-6282.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 7d51ec77-c785-4b89-b717-ff9060d8bda7
duration: 652
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1386'
ht-degree: 100%

---

# Entwickeln eines Asset Compute-Sekundärs

Asset Compute-Sekundäre bilden den Kern eines Asset Compute-Projekts, da sie eine benutzerdefinierte Funktion bereitstellen, mit der die an einem Asset durchgeführten Arbeiten zur Erstellung einer neuen Ausgabedarstellung ausgeführt oder orchestriert werden.

Das Asset Compute-Projekt generiert automatisch einen einfachen Sekundär, der die ursprüngliche Binärdatei des Assets in eine benannte Ausgabedarstellung kopiert, ohne dass Transformationen erfolgen. In diesem Tutorial werden wir diesen Sekundär ändern, um eine interessantere Ausgabedarstellung zu erstellen und damit die Macht von Asset Compute-Sekundären zu veranschaulichen.

Wir erstellen einen Asset Compute-Sekundär, der eine neue horizontale Bilddarstellung generiert, die leeren Raum links und rechts neben der Asset-Ausgabedarstellung mit einer verschwommenen Version des Assets abdeckt. Die Breite, Höhe und Unschärfe der endgültigen Ausgabedarstellung sind parametrisiert.

## Logischer Ablauf eines Asset Compute-Sekundäraufrufs

Asset Compute-Sekundäre implementieren den API-Vertrag für den Asset Compute-SDK-Sekundär in der Funktion `renditionCallback(...)`, für die konzeptionell Folgendes gilt:

+ __Eingabe:__ Die originale Binärdatei und die Verarbeitungsprofil-Parameter eines AEM Assets
+ __Ausgabe:__ Eine oder mehrere Ausgabedarstellungen, die dem AEM Asset hinzugefügt werden sollen

![Logischer Fluss eines Asset Compute-Sekundärs](./assets/worker/logical-flow.png)

1. Der AEM-Author-Service ruft den Asset Compute-Sekundär auf und stellt __(1a)__ die originale Binärdatei (Parameter `source`) und __(1b)__ alle im Verarbeitungsprofil definierten Parameter (Parameter `rendition.instructions`) bereit.
1. Das Asset Ccompute-SDK orchestriert die Ausführung der Funktion `renditionCallback(...)` des benutzerdefinierten Asset Compute-Metadaten-Sekundärs, wodurch eine neue binäre Ausgabedarstellung entsteht, die auf der ursprünglichen Binärdatei des Assets __(1a)__ und allen Parametern __(1b)__ basiert.

   + In diesem Tutorial wird die Ausgabedarstellung „in Bearbeitung“ erstellt, d. h., der Sekundär stellt die Ausgabedarstellung zusammen. Die Quellbinärdatei kann jedoch zur Ausgabedarstellungsgenerierung auch an andere Web-Dienst-APIs gesendet werden.

1. Der Asset Compute-Sekundär speichert die Binärdaten der neuen Ausgabedarstellung in `rendition.path`.
1. Die Binärdaten, die in `rendition.path` geschrieben sind, werden über das Asset Compute-SDK an den AEM-Author-Service übertragen und __(4a)__ als Textausgabe angezeigt und __(4b)__ im Metadatenknoten des Assets beibehalten.

Das obige Diagramm zeigt die Asset Compute-Entwickleranliegen und den logischen Fluss des Asset-Compute-Sekundäraufrufs. Für Neugierige sind die [internen Details der Asset Compute-Ausführung](https://experienceleague.adobe.com/docs/asset-compute/using/extend/custom-application-internals.html?lang=de) verfügbar, jedoch sind nur die öffentlichen API-Verträge für Asset Compute-SDK zuverlässig.

## Anatomie eines Sekundärs

Alle Asset Compute-Sekundäre folgen der gleichen Grundstruktur und dem gleichen Eingabe-/Ausgabevertrag.

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

## Öffnen der Sekundärdatei index.js

![Automatisch generierte index.js](./assets/worker/autogenerated-index-js.png)

1. Stellen Sie sicher, dass das Asset Compute-Projekt in VS Code geöffnet ist.
1. Navigieren Sie zum Ordner `/actions/worker`. 
1. Öffnen Sie die Datei `index.js`.

Dies ist die JavaScript-Sekundärdatei, die wir in diesem Tutorial ändern werden.

## Installieren und Importieren von unterstützenden npm-Modulen

Da Asset Compute-Projekte auf Node.js basieren, profitieren sie vom leistungsstarken [npm-Modul-Ökosystem](https://npmjs.com). Um npm-Module zu nutzen, müssen wir sie zunächst in unserem Asset Compute-Projekt installieren.

In diesem Sekundär nutzen wir [jimp](https://www.npmjs.com/package/jimp), um das Ausgabedarstellungsbild direkt im Code von Node.js zu erstellen und zu bearbeiten.

>[!WARNING]
>
>Nicht alle npm-Module für die Asset-Manipulation werden von Asset Compute unterstützt. npm-Module, die auf die Existenz von Anwendungen wie ImageMagick oder anderen betriebssystemabhängigen Bibliotheken angewiesen sind, werden nicht unterstützt. Es empfiehlt sich, die Verwendung auf reine JavaScript-npm-Module zu beschränken.

1. Öffnen Sie die Befehlszeile im Stammverzeichnis Ihres Asset Compute-Projekts (dies kann im VS-Code über __Terminal > Neues Terminal__ durchgeführt werden) und führen Sie den folgenden Befehl aus:

   ```
   $ npm install jimp
   ```

1. Importieren Sie das `jimp`-Modul in den Code des Sekundärs, damit es über das `Jimp`-JavaScript-Objekt genutzt werden kann.
Aktualisieren Sie die `require`-Anweisungen am Anfang der `index.js` des Sekundärs, um das `Jimp`-Objekt aus dem `jimp`-Modul zu importieren:

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

## Lesen der Parameter

Asset Compute-Sekundäre können Parameter lesen, die über Verarbeitungsprofile übergeben werden können, die im Author-Service von AEM as a Cloud Service definiert sind. Die Parameter werden über das Objekt `rendition.instructions` an den Sekundär übertragen.

Diese können durch Zugreifen auf `rendition.instructions.<parameterName>` im Code des Sekundärs gelesen werden.

Hier geben wir die einstellbaren Werte für `SIZE`, `BRIGHTNESS` und `CONTRAST` der Ausgabedarstellung ein, wobei wir Standardwerte angeben, wenn keine über das Verarbeitungsprofil bereitgestellt wurden. Beachten Sie, dass `renditions.instructions` als Zeichenfolgen übergeben werden, wenn sie über Verarbeitungsprofile von AEM as a Cloud Service aufgerufen werden. Stellen Sie daher sicher, dass sie in die richtigen Datentypen im Code des Sekundärs umgewandelt werden.

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

## Ausgabe von Fehlern{#errors}

Asset Compute-Sekundäre können auf Fehlersituationen stoßen. Das Adobe Asset Compute-SDK bietet [eine Suite vordefinierter Fehler](https://github.com/adobe/asset-compute-commons#asset-compute-errors), die bei solchen Situationen ausgelöst werden können. Wenn kein bestimmter Fehlertyp zutrifft, kann der `GenericError` verwendet werden oder es können spezifische benutzerdefinierte `ClientErrors` definiert werden.

Bevor Sie mit der Verarbeitung der Ausgabedarstellung beginnen, überprüfen Sie, ob alle Parameter im Kontext dieses Sekundärs gültig sind und unterstützt werden:

+ Stellen Sie sicher, dass die Anweisungsparameter für `SIZE`, `CONTRAST` und `BRIGHTNESS` der Ausgabedarstellung gültig sind. Wenn nicht, wird ein benutzerdefinierter Fehler `RenditionInstructionsError` ausgegeben.
   + Eine benutzerdefinierte `RenditionInstructionsError`-Klasse, die `ClientError` erweitert, wird am Ende dieser Datei definiert. Die Verwendung eines spezifischen, benutzerdefinierten Fehlers ist nützlich beim [Schreiben von Tests](../test-debug/test.md) für den Sekundär.

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

Mit den gelesenen, bereinigten und validierten Parametern wird ein Code geschrieben, um die Ausgabedarstellung zu generieren. Der Pseudo-Code für die Generierung von Ausgabedarstellungen lautet wie folgt:

1. Erstellen Sie eine neue `renditionImage`-Arbeitsfläche in quadratischen Dimensionen, die über die `size`-Parameter definiert werden.
1. Erstellen Sie ein `image`-Objekt aus der Binärdatei des Quell-Assets
1. Verwenden Sie die __Jimp__-Bibliothek, um das Bild zu transformieren:
   + Schneiden Sie das Originalbild auf ein zentriertes Quadrat zu
   + Schneiden Sie einen Kreis aus der Mitte des quadratischen Bildes aus
   + Skalieren Sie es so, dass es in die Abmessungen passt, die durch den `SIZE`-Parameterwert definiert werden
   + Passen Sie den Kontrast basierend auf dem `CONTRAST`-Parameterwert an
   + Passen Sie die Helligkeit basierend auf dem `BRIGHTNESS`-Parameterwert an
1. Platzieren Sie das umgewandelte `image` in die Mitte des `renditionImage` mit transparentem Hintergrund
1. Schreiben Sie das zusammengesetzte `renditionImage` in den `rendition.path`, sodass es als Asset-Ausgabedarstellung wieder in AEM gespeichert werden kann.

Dieser Code verwendet die [Jimp-APIs](https://github.com/oliver-moran/jimp#jimp), um diese Bildtransformationen durchzuführen.

Asset Compute-Sekundäre müssen ihre Arbeit synchron beenden und der `rendition.path` muss vollständig zurückgeschrieben worden sein, bevor der `renditionCallback` des Sekundärs beendet ist. Dies erfordert, dass asynchrone Funktionsaufrufe mithilfe des `await`-Operators stattfinden. Wenn Sie nicht mit asynchronen JavaScript-Funktionen vertraut sind und nicht wissen, wie Sie diese synchron ausführen lassen können, sollten Sie sich mit dem [Await-Operator von JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await) vertraut machen.

Die abgeschlossene `index.js` Sekundärs sollte wie folgt aussehen:

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

## Ausführen des Sekundärs

Nachdem der Code des Sekundärs jetzt vollständig ist und zuvor in [manifest.yml](./manifest.md) registriert und konfiguriert wurde, kann er mit dem lokalen Asset Compute-Entwicklungs-Tool ausgeführt werden, um die Ergebnisse anzuzeigen.

1. Navigieren Sie zum Stammverzeichnis des Asset Compute-Projekts.
1. Führen Sie `aio app run` aus.
1. Warten Sie, bis das Asset Compute-Entwicklungs-Tool in einem neuen Fenster geöffnet wird.
1. Wählen Sie in der Dropdown-Liste __Datei auswählen…__ ein Beispielbild für die Verarbeitung.
   + Wählen Sie eine Beispielgrafikdatei zur Verwendung als Quell-Asset-Binärdatei.
   + Wenn noch keine vorhanden ist, tippen Sie auf das __(+)__ auf der linken Seite, laden Sie ein [Beispielbild](../assets/samples/sample-file.jpg) hoch und aktualisieren Sie das Browser-Fenster des Entwicklungs-Tools.
1. Aktualisieren Sie `"name": "rendition.png"`, da dieser Sekundär ein transparentes PNG generiert.
   + Beachten Sie, dass dieser „name“-Parameter nur für das Entwicklungs-Tool verwendet wird und sich nicht darauf verlassen werden sollte.

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

1. Tippen Sie auf __Ausführen__ und warten Sie, bis die Ausgabedarstellung generiert wird
1. Der Abschnitt __Ausgabedarstellungen__ zeigt eine Vorschau der generierten Ausgabedarstellung an. Tippen Sie auf die Vorschau der Ausgabedarstellung, um die vollständige Ausgabedarstellung herunterzuladen.

   ![PNG-Standardausgabedarstellung](./assets/worker/default-rendition.png)

### Ausführen des Sekundärs mit Parametern

Parameter, die über Verarbeitungsprofilkonfigurationen übergeben werden, können in Asset Compute-Entwicklungs-Tools simuliert werden, indem sie als Schlüssel/Wert-Paare in der JSON der Ausgabedarstellungsparameter angegeben werden.

>[!WARNING]
>
>Während der lokalen Entwicklung können Werte mithilfe verschiedener Datentypen übergeben werden, wenn sie von AEM as a Cloud Service-Verarbeitungsprofilen als Zeichenfolgen übergeben werden. Achten Sie daher darauf, bei Bedarf die richtigen Datentypen zu parsen.
> Zum Beispiel erfordert die `crop(width, height)`-Funktion von Jimp, dass ihre Parameter `int`s sind. Wenn `parseInt(rendition.instructions.size)` nicht auf ein int geparst wird, schlägt der Aufruf von `jimp.crop(SIZE, SIZE)` fehl, da die Parameter den inkompatiblen Typ „Zeichenfolge“ aufweisen.

Unser Code akzeptiert Parameter für:

+ `size` definiert die Größe der Ausgabedarstellung (Höhe und Breite als Ganzzahlen)
+ `contrast` definiert die Kontrastanpassung, muss zwischen -1 und 1 liegen, als Fließkommazahl.
+ `brightness` definiert die Helligkeitseinstellung, muss zwischen -1 und 1 liegen, als Fließkommazahl.

Diese werden in der `index.js` des Sekundärs gelesen über:

+ `const SIZE = parseInt(rendition.instructions.size) || 800`
+ `const CONTRAST = parseFloat(rendition.instructions.contrast) || 0`
+ `const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0`

1. Aktualisieren Sie die Parameter der Ausgabedarstellung, um Größe, Kontrast und Helligkeit anzupassen.

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
1. Tippen Sie auf die Vorschau der Ausgabedarstellung, um die generierte Ausgabedarstellung herunterzuladen und zu überprüfen. Achten Sie auf die Dimensionen und darauf, wie Kontrast und Helligkeit im Vergleich zur Standarddarstellung geändert wurden.

   ![Parametrisierte PNG-Wiedergabe](./assets/worker/parameterized-rendition.png)

1. Laden Sie andere Bilder in die __Quelldatei__-Dropdown-Liste hoch und versuchen Sie, für diese den Sekundär mit verschiedenen Parametern auszuführen.

## index.js des Sekundärs auf Github

Die endgültige `index.js` ist auf Github verfügbar unter:

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Fehlerbehebung

+ [Ausgabedarstellung nur teilweise gezeichnet/beschädigt zurückgegeben](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)

---
title: Entwickeln eines Asset compute-Metadaten-Sekundärs
description: Erfahren Sie, wie Sie einen Asset compute-Metadaten-Worker erstellen, der die am häufigsten verwendeten Farben in einem Bild-Asset ableitet und die Farbnamen in die Metadaten des Assets in AEM schreibt.
feature: asset compute Microservices
topics: metadata, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6448
thumbnail: 327313.jpg
topic: Integrationen, Entwicklung
role: Developer
level: Intermediate, Experienced
source-git-commit: dbc0a35ae96594fec1e10f411d57d2a3812c1cf2
workflow-type: tm+mt
source-wordcount: '1439'
ht-degree: 1%

---


# Entwickeln eines Asset compute-Metadaten-Sekundärs

Benutzerdefinierte Asset compute-Sekundäre können XMP (XML) Daten erstellen, die zurück an AEM gesendet und als Metadaten für ein Asset gespeichert werden.

Häufige Anwendungsfälle sind:

+ Integrationen mit Drittanbietersystemen, z. B. einem PIM (Product Information Management System), bei dem zusätzliche Metadaten abgerufen und im Asset gespeichert werden müssen
+ Integrationen mit Adobe-Diensten wie Content and Commerce AI zur Erweiterung von Asset-Metadaten um zusätzliche Attribute für maschinelles Lernen
+ Ableiten von Metadaten zum Asset aus seiner Binärdatei und Speichern als Asset-Metadaten in AEM als Cloud Service

## Was Sie tun werden

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

In diesem Tutorial erstellen wir einen Asset compute Metadaten-Worker, der die am häufigsten verwendeten Farben in einem Bild-Asset ableitet und die Farbnamen in die Metadaten des Assets in AEM schreibt. Obwohl das Sekundärprogramm selbst einfach ist, wird in diesem Tutorial untersucht, wie Asset compute-Sekundärprogramme zum Zurückschreiben von Metadaten in Assets in AEM als Cloud Service verwendet werden können.

## Logischer Ablauf eines Asset compute-Metadaten-Worker-Aufrufs

Der Aufruf von Asset compute-Metadatenarbeitern ist fast identisch mit dem von [binäre Ausgabedarstellung, die Sekundäre](../develop/worker.md) generiert, wobei der Hauptunterschied der Rückgabetyp ist eine XMP (XML)-Ausgabedarstellung, deren Werte auch in die Metadaten des Assets geschrieben werden.

asset compute-Worker implementieren den Asset compute SDK Worker API-Vertrag in der `renditionCallback(...)`-Funktion, die konzeptionell lautet:

+ __Eingabe:__ Die ursprünglichen binären Parameter und Verarbeitungsprofil eines AEM Assets
+ __Ausgabe:__ Eine XMP (XML)-Ausgabedarstellung, die als Ausgabedarstellung am AEM Asset und den Metadaten des Assets beibehalten wird

![Logischer Ablauf des asset compute-Metadaten-Workflows](./assets/metadata/logical-flow.png)

1. Der AEM-Autorendienst ruft den Asset compute-Metadaten-Worker auf und stellt die __(1a)__ ursprüngliche Binärdatei des Assets sowie __(1b)__ alle im Verarbeitungsprofil definierten Parameter bereit.
1. Das Asset compute SDK orchestriert die Ausführung der `renditionCallback(...)`-Funktion des benutzerdefinierten Asset compute-Metadatenarbeiters und leitet eine XMP (XML)-Ausgabedarstellung ab, die auf der binären __(1a)__-Datei des Assets und den Verarbeitungsprofilparametern __(1b)__ basiert.
1. Der Asset compute Worker speichert die XMP (XML)-Darstellung in `rendition.path`.
1. Die XMP (XML)-Daten, die in `rendition.path` geschrieben wurden, werden über das Asset compute SDK an den AEM-Autorendienst übertragen und als __(4a)__ Textausgabe und __(4b)__ im Metadatenknoten des Assets gespeichert.

## Konfigurieren von manifest.yml{#manifest}

Alle Asset compute-Sekundäre müssen in [manifest.yml](../develop/manifest.md) registriert sein.

Öffnen Sie die `manifest.yml` des Projekts und fügen Sie einen Worker-Eintrag hinzu, der den neuen Worker konfiguriert, in diesem Fall `metadata-colors`.

_Beachten Sie, dass  `.yml` Whitespace-Einstellungen berücksichtigt werden._

```
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          timeout: 60000 # in ms
          memorySize: 512 # in MB
          concurrency: 10 
        annotations:
          require-adobe-auth: true
      metadata-colors:
        function: actions/metadata-colors/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          memorySize: 512 # in MB   
```

`function` verweist auf die Worker-Implementierung, die im  [nächsten Schritt](#metadata-worker) erstellt wurde. Benennen Sie Sekundäre semantisch (z. B. `actions/worker/index.js` wurde besser `actions/rendition-circle/index.js` genannt), wie sie in der [Worker-URL](#deploy) angezeigt werden, und bestimmen Sie auch den Ordner-Namen der [Worker-Test-Suite](#test).

Die `limits` und `require-adobe-auth` werden diskret pro Worker konfiguriert. In diesem Worker wird `512 MB` des Speichers zugewiesen, wenn der Code (potenziell) große binäre Bilddaten prüft. Die anderen `limits` werden entfernt, um Standardwerte zu verwenden.

## Entwickeln eines Metadaten-Workers{#metadata-worker}

Erstellen Sie eine neue Metadaten-Worker-JavaScript-Datei im Asset compute-Projekt unter dem Pfad [defined manifest.yml für den neuen Worker](#manifest) unter `/actions/metadata-colors/index.js`

### Installieren von npm-Modulen

Installieren Sie die zusätzlichen NPM-Module ([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions), [get-image-color](https://www.npmjs.com/package/get-image-colors) und [color-namer](https://www.npmjs.com/package/color-namer)), die in diesem Asset compute Worker verwendet werden.

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### Metadaten-Worker-Code

Dieser Worker sieht dem [rendition-generate worker](../develop/worker.md) sehr ähnlich. Der Hauptunterschied besteht darin, XMP (XML) Daten in `rendition.path` zu schreiben, um wieder in AEM gespeichert zu werden.


```javascript
"use strict";

const { worker, SourceCorruptError } = require("@adobe/asset-compute-sdk");
const fs = require("fs").promises;

// Require the @adobe/asset-compute-xmp module to create XMP 
const { serializeXmp } = require("@adobe/asset-compute-xmp");

// Require supporting npm modules to derive image colors from image data
const getColors = require("get-image-colors");
// Require supporting npm modules to convert image colors to color names
const namer = require("color-namer");

exports.main = worker(async (source, rendition, params) => {
  // Perform any necessary source (input) checks
  const stats = await fs.stat(source.path);
  if (stats.size === 0) {
    // Throw appropriate errors whenever an erring condition is met
    throw new SourceCorruptError("source file is empty");
  }
  const MAX_COLORS = 10;
  const DEFAULT_COLORS_FAMILY = 'basic';

  // Read the color family parameter to use to derive the color names
  let colorsFamily = rendition.instructions.colorsFamily || DEFAULT_COLORS_FAMILY;

  if (['basic', 'hex', 'html', 'ntc', 'pantone', 'roygbiv'].indexOf(colorsFamily) === -1) { 
      colorsFamily = DEFAULT_COLORS_FAMILY;
  }
  
  // Use the `get-image-colors` module to derive the most common colors from the image
  let colors = await getColors(source.path, { options: MAX_COLORS });

  // Convert the color Chroma objects to their closest names
  let colorNames = colors.map((color) => getColorName(colorsFamily, color));

  // Serialize the data to XMP metadata
  // These properties are written to the [dam:Asset]/jcr:content/metadata resource
  // This stores
  // - The list of color names is stored in a JCR property named `wknd:colors`
  // - The colors family used to derive the color names is stored in a JCR property named `wknd:colorsFamily`
  const xmp = serializeXmp({
      // Use a Set to de-duplicate color names
      "wknd:colors": [...new Set(colorNames)],
      "wknd:colorsFamily": colorsFamily
    }, {
      // Define any property namespaces used in the above property/value definition
      // These namespaces will be automatically registered in AEM if they do not yet exist
      namespaces: {
        wknd: "https://wknd.site/assets/1.0/",
      },
    }
  );

  // Save the XMP metadata to be written back to the asset's metadata node
  await fs.writeFile(rendition.path, xmp, "utf-8");
});

/**
 * Helper function that derives the closest color name for the color, based on the colors family
 * 
 * @param {*} colorsFamily the colors name family to use
 * @param {*} color the color to convert to a name
 */
function getColorName(colorsFamily, color) {
    if ('hex' === colorsFamily) {  return color; }

    let names = namer(color.rgb())[colorsFamily];

    if (names.length >= 1) { return names[0].name; }
}
```

## Führen Sie den Metadaten-Worker lokal aus{#development-tool}

Wenn der Worker-Code abgeschlossen ist, kann er mit dem lokalen Asset compute Development Tool ausgeführt werden.

Da unser Asset compute-Projekt zwei Sekundärprogramme enthält (das vorherige [Kreisausgabeformat](../develop/worker.md) und dieses `metadata-colors` -Sekundärprogramm), listet die Profildefinition des [Asset compute-Entwicklungstools ](../develop/development-tool.md) Ausführungsprofile für beide Sekundäre auf. Die zweite Profildefinition verweist auf den neuen Worker `metadata-colors` .

![XML-Metadaten-Ausgabe](./assets/metadata/metadata-rendition.png)

1. Aus dem Stammverzeichnis des Asset compute-Projekts
1. Führen Sie `aio app run` aus, um das Asset compute Development Tool zu starten.
1. Wählen Sie im __Datei auswählen...__ Dropdown-Liste wählen Sie ein [Beispielbild](../assets/samples/sample-file.jpg) zur Verarbeitung aus
1. Aktualisieren Sie in der zweiten Profildefinitionskonfiguration, die auf den Worker `metadata-colors` verweist, `"name": "rendition.xml"` , da dieser Worker eine XMP (XML)-Ausgabedarstellung generiert. Optional können Sie einen Parameter `colorsFamily` hinzufügen (unterstützte Werte `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`).

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.xml",
               "colorsFamily": "pantone"
           }
       ]
   }
   ```

1. Tippen Sie auf __Ausführen__ und warten Sie, bis die XML-Ausgabe generiert wird.
   + Da beide Sekundäre in der Profildefinition aufgeführt sind, werden beide Ausgabedarstellungen generiert. Optional kann die obere Profildefinition, die auf den [Kreis-Ausgabedarstellungs-Worker](../develop/worker.md) verweist, gelöscht werden, um die Ausführung nicht über das Entwicklungstool zu vermeiden.
1. Im Abschnitt __Ausgabeformate__ wird eine Vorschau der generierten Ausgabedarstellung angezeigt. Tippen Sie auf `rendition.xml`, um es herunterzuladen, und öffnen Sie es in VS Code (oder Ihrem bevorzugten XML/Texteditor), um es zu überprüfen.

## Testen Sie den Worker.{#test}

Metadaten-Sekundäre können mit dem [gleichen Asset compute-Test-Framework wie binäre Ausgabeformate](../test-debug/test.md) getestet werden. Der einzige Unterschied ist die `rendition.xxx`-Datei im Testfall muss die erwartete XMP (XML)-Ausgabedarstellung sein.

1. Erstellen Sie die folgende Struktur im Asset compute-Projekt:

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. Verwenden Sie die Beispieldatei [a1/> als Testfall `file.jpg`.](../assets/samples/sample-file.jpg)
3. Fügen Sie die folgende JSON zum `params.json` hinzu.

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   Beachten Sie, dass `"fmt": "xml"` erforderlich ist, um die Test-Suite anzuweisen, eine textbasierte Ausgabe zu generieren, die `.xml`.

4. Geben Sie die erwartete XML in die Datei `rendition.xml` ein. Dies erhalten Sie durch:
   + Ausführen der Testeingabedatei über das Entwicklungstool und Speichern der (validierten) XML-Ausgabe.

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. Führen Sie `aio app test` aus dem Stammverzeichnis des Asset compute-Projekts aus, um alle Test-Suites auszuführen.

### Bereitstellen des Sekundärs in Adobe I/O Runtime{#deploy}

Um diesen neuen Metadaten-Worker aus AEM Assets aufzurufen, muss er mithilfe des folgenden Befehls in Adobe I/O Runtime bereitgestellt werden:

```
$ aio app deploy
```

![aio app deploy](./assets/metadata/aio-app-deploy.png)

Beachten Sie, dass dadurch alle im Projekt enthaltenen Arbeiter bereitgestellt werden. Überprüfen Sie die [ungekürzten Bereitstellungsanweisungen](../deploy/runtime.md), um zu erfahren, wie Sie in Staging- und Produktionsarbeitsbereichen bereitstellen.

### Integration mit AEM Verarbeitungsprofilen{#processing-profile}

Rufen Sie den Worker von AEM auf, indem Sie einen neuen oder einen vorhandenen benutzerdefinierten Verarbeitungsprofildienst erstellen, der diesen bereitgestellten Worker aufruft.

![Verarbeitungsprofil](./assets/metadata/processing-profile.png)

1. Melden Sie sich bei AEM als Cloud Service-Autorendienst als __AEM Administrator__ an.
1. Navigieren Sie zu __Tools > Assets > Verarbeitungsprofile__ .
1. ____ Erstellen Sie ein neues oder  ____ bearbeiten Sie vorhandenes Verarbeitungsprofil.
1. Tippen Sie auf die Registerkarte __Benutzerdefiniert__ und dann auf __Neu hinzufügen__
1. Definieren des neuen Dienstes
   + __Erstellen von Metadaten-Ausgabeformaten__: Aktivieren
   + __Endpunkt:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + Dies ist die URL für den Worker, der während des [deploy](#deploy) oder mithilfe des Befehls `aio app get-url` abgerufen wurde. Stellen Sie sicher, dass die URL auf den richtigen Arbeitsbereich verweist, basierend auf der AEM als Cloud Service-Umgebung.
   + __Dienstparameter__
      + Tippen Sie auf __Parameter__ hinzufügen
         + Schlüssel: `colorFamily`
         + Wert: `pantone`
            + Unterstützte Werte: `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`
   + __MIME-Typen__
      + __Umfasst:__ `image/jpeg`,  `image/png`,  `image/gif`,  `image/svg`
         + Dies sind die einzigen MIME-Typen, die von npm-Modulen von Drittanbietern unterstützt werden, die zum Ableiten der Farben verwendet werden.
      + __Schließt Folgendes aus:__ `Leave blank`
1. Tippen Sie oben rechts auf __Speichern__ .
1. Wenden Sie das Verarbeitungsprofil auf einen Ordner &quot;AEM Assets&quot;an, falls nicht bereits geschehen.

### Aktualisieren des Metadatenschemas{#metadata-schema}

Um die Farbmetadaten zu überprüfen, ordnen Sie zwei neue Felder im Metadatenschema des Bildes den neuen Metadaten-Eigenschaften zu, die der Worker füllt.

![Metadatenschema](./assets/metadata/metadata-schema.png)

1. Navigieren Sie im AEM-Autorendienst zu __Tools > Assets > Metadatenschemata__
1. Navigieren Sie zu __default__, wählen Sie __image__ aus und bearbeiten Sie sie und fügen Sie schreibgeschützte Formularfelder hinzu, um die generierten Farbmetadaten anzuzeigen.
1. Fügen Sie einen __einzeiligen Text__ hinzu.
   + __Feldbezeichnung__: `Colors Family`
   + __Zu Eigenschaft zuordnen__: `./jcr:content/metadata/wknd:colorsFamily`
   + __Regeln > Feld > Bearbeitung__ deaktivieren: Aktiviert
1. Fügen Sie einen __Mehrwerttext__ hinzu.
   + __Feldbezeichnung__: `Colors`
   + __Zu Eigenschaft zuordnen__: `./jcr:content/metadata/wknd:colors`
1. Tippen Sie oben rechts auf __Speichern__ .

## Verarbeiten von Assets

![Asset-Details](./assets/metadata/asset-details.png)

1. Navigieren Sie im AEM-Autorendienst zu __Assets > Dateien__
1. Navigieren Sie zum Ordner oder Unterordner, auf den das Verarbeitungsprofil angewendet wird.
1. Laden Sie ein neues Bild (JPEG, PNG, GIF oder SVG) in den Ordner hoch oder verarbeiten Sie vorhandene Bilder mit dem aktualisierten [Verarbeitungsprofil](#processing-profile) erneut.
1. Wenn die Verarbeitung abgeschlossen ist, wählen Sie das Asset aus und tippen Sie in der oberen Aktionsleiste auf __properties__ , um dessen Metadaten anzuzeigen
1. Überprüfen Sie die Felder `Colors Family` und `Colors` [Metadaten](#metadata-schema) für die Metadaten, die vom benutzerdefinierten Asset compute-Metadaten-Worker zurückgeschrieben wurden.

Nachdem die Farbmetadaten in die Metadaten des Assets geschrieben wurden, werden diese Metadaten in der Ressource `[dam:Asset]/jcr:content/metadata` indiziert und können mithilfe dieser Begriffe über die Suche besser erkannt werden. Sie können sogar zurück in die Binärdatei des Assets geschrieben werden, wenn dann der Workflow __DAM Metadata Writeback__ aufgerufen wird.

### Metadaten-Ausgabedarstellung in AEM Assets

![AEM Assets-Metadaten-Ausgabedarstellungsdatei](./assets/metadata/cqdam-metadata-rendition.png)

Die tatsächliche XMP-Datei, die vom Asset compute-Metadaten-Worker generiert wurde, wird ebenfalls als eigenständiges Ausgabeformat für das Asset gespeichert. Diese Datei wird im Allgemeinen nicht verwendet. Stattdessen werden die auf den Metadatenknoten des Assets angewendeten Werte verwendet, aber die XML-Rohausgabe des Workers ist in AEM verfügbar.

## Metadaten-Farben-Workercode auf Github

Das endgültige `metadata-colors/index.js` ist auf Github verfügbar unter:

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

Die endgültige `test/asset-compute/metadata-colors` Test-Suite ist auf Github verfügbar unter:

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-color](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)

---
title: Verwenden der API „usagerights“
description: Beispiel-Code zum Anwenden von Verwendungsrechten auf die bereitgestellte PDF
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: a4e2132b-3cfd-4377-8998-6944365edec5
source-git-commit: 1a76256677d06aaffd142c46dc9167a669ac6455
workflow-type: ht
source-wordcount: '280'
ht-degree: 100%

---

# Tätigen eines API-Aufrufs

## Anwenden von Verwendungsrechten

Sobald Sie über das Zugriffs-Token verfügen, besteht der nächste Schritt darin, eine API-Anfrage zu stellen, um Verwendungsrechte auf die angegebene PDF anzuwenden. Dazu gehört das Aufnehmen des Zugriffs-Tokens in die Anfrage-Kopfzeile, um den Aufruf zu authentifizieren und eine sichere und autorisierte Verarbeitung des Dokuments sicherzustellen.

Die folgende Funktion wendet die Verwendungsrechte an

```java
public void applyUsageRights(String accessToken,String endPoint) {

            String host = "https://" + BUCKET + ".adobeaemcloud.com";
            String url = host + endPoint;
            String usageRights = "{\"comments\":true,\"embeddedFiles\":true,\"formFillIn\":true,\"formDataExport\":true}";

            logger.info("Request URL: {}", url);
            logger.info("Access Token: {}", accessToken);

            ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
            URL pdfFile = classLoader.getResource("pdffiles/withoutusagerights.pdf");

            if (pdfFile == null) {
                logger.error("PDF file not found!");
                return;
            }

            File fileToApplyRights = new File(pdfFile.getPath());
            CloseableHttpClient httpClient = null;
            CloseableHttpResponse response = null;
            InputStream generatedPDF = null;
            FileOutputStream outputStream = null;
            
            try {
                httpClient = HttpClients.createDefault();
                byte[] fileContent = FileUtils.readFileToByteArray(fileToApplyRights);
                MultipartEntityBuilder builder = MultipartEntityBuilder.create();
                builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"),fileToApplyRights.getName());
                builder.addTextBody("usageRights", usageRights, ContentType.APPLICATION_JSON);
                
                HttpPost httpPost = new HttpPost(url);
                httpPost.addHeader("Authorization", "Bearer " + accessToken);
                httpPost.addHeader("X-Adobe-Accept-Experimental", "1");
                httpPost.setEntity(builder.build());
                
                response = httpClient.execute(httpPost);
                generatedPDF = response.getEntity().getContent();
                byte[] bytes = IOUtils.toByteArray(generatedPDF);

                outputStream = new FileOutputStream(SAVE_LOCATION + File.separator + "ReaderExtended.pdf");
                outputStream.write(bytes);
                logger.info("ReaderExtended File is  saved at "+SAVE_LOCATION);
            } catch (IOException e) {
                logger.error("Error applying usage rights", e);
            } finally {
                try {
                    if (generatedPDF != null) generatedPDF.close();
                    if (response != null) response.close();
                    if (httpClient != null) httpClient.close();
                    if (outputStream != null) outputStream.close();
                } catch (IOException e) {
                    logger.error("Error closing resources", e);
                }
            }
        }
```

## Funktionsaufschlüsselung:



* **Einrichten von API-Endpunkt und Payload**
   * Erstellt die API-URL unter Verwendung des bereitgestellten `endPoint` und eines vordefinierten `BUCKET`.
   * Definiert eine JSON-Zeichenfolge (`usageRights`), die die anzuwendenden Rechte angibt, z. B.:
      * Kommentare
      * Eingebettete Dateien
      * Ausfüllen von Formularen
      * Formulardatenexport

* **Laden der PDF-Datei**
   * Ruft die Datei `withoutusagerights.pdf` aus dem Verzeichnis `pdffiles` ab.
   * Protokolliert einen Fehler, wenn die Datei nicht gefunden wird, und beendet den Vorgang.

* **Vorbereiten der HTTP-Anfrage**
   * Liest die PDF-Datei in ein Byte-Array.
   * Verwendet `MultipartEntityBuilder` zum Erstellen einer mehrteiligen Anfrage mit:
      * der PDF-Datei als Hauptteil der Binärdatei.
      * der JSON `usageRights` als Textkörper.
   * Richtet eine HTTP-`POST`-Anfrage mit Headern ein:
      * `Authorization: Bearer <accessToken>` für die Authentifizierung.
      * `X-Adobe-Accept-Experimental: 1` (möglicherweise für die API-Kompatibilität erforderlich).

* **Senden der Anfrage und Verarbeiten der Antwort**
   * Führt die HTTP-Anfrage mithilfe von `httpClient.execute(httpPost)` aus.
   * Liest die Antwort (erwartet wird die aktualisierte PDF mit angewendeten Verwendungsrechten).
   * Schreibt die erhaltenen PDF-Inhalte in **„ReaderExtended.pdf“** unter `SAVE_LOCATION`.

* **Fehlerverarbeitung und Bereinigung**
   * Erfasst und protokolliert alle Fehler vom Typ `IOException`.
   * Stellt sicher, dass alle Ressourcen (Streams, HTTP-Client, Antwort) im Block `finally` ordnungsgemäß geschlossen werden.

Im Folgenden finden Sie den main.java-Code, der die Funktion „applyUsageRights“ aufruft.

```java
package com.aemformscs.communicationapi;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Main {
    private static final Logger logger = LoggerFactory.getLogger(Main.class);

    public static void main(String[] args) {
        try {
            String accessToken = new AccessTokenService().getAccessToken();
            DocumentGeneration docGen = new DocumentGeneration();

            docGen.applyUsageRights(accessToken, "/adobe/document/assure/usagerights");

            // Uncomment as needed
            // docGen.extractPDFProperties(accessToken, "/adobe/document/extract/pdfproperties");
            // docGen.mergeDataWithXdpTemplate(accessToken, "/adobe/document/generate/pdfform");

        } catch (Exception e) {
            logger.error("Error occurred: {}", e.getMessage(), e);
        }
    }
}
```

Die `main`-Methode wird initialisiert, indem `getAccessToken()` über den `AccessTokenService` aufgerufen wird, von dem erwartet wird, dass er ein gültiges Token zurückgibt.

* Anschließend ruft sie `applyUsageRights()` über die Klasse `DocumentGeneration` auf und übergibt:
   * das abgerufene `accessToken`
   * den API-Endpunkt zum Anwenden der Verwendungsrechte.


## Nächste Schritte

[Bereitstellen des Beispielprojekts](sample-project.md)

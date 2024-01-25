---
title: Erstellen Ihres ersten OSGi-Dienstes mit AEM Forms
description: Erstellen Ihres ersten OSGi-Dienstes mit AEM Forms
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 2f15782e-b60d-40c6-b95b-6c7aa8290691
last-substantial-update: 2021-04-23T00:00:00Z
duration: 103
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 100%

---

# OSGi-Dienst

Ein OSGi-Dienst ist eine Java-Klasse- oder Dienstschnittstelle mit einer Reihe von Diensteigenschaften als Name-Wert-Paare. Die Diensteigenschaften unterscheiden sich zwischen verschiedenen Dientanbietern, die Dienste mit derselben Dienstschnittstelle bereitstellen.

Ein OSGi-Dienst wird semantisch durch seine Dienstschnittstelle definiert und als Dienstobjekt implementiert. Die Funktionalität eines Dienstes wird durch die Schnittstellen definiert, die er implementiert. Dadurch können unterschiedliche Anwendungen denselben Dienst implementieren. Dienstschnittstellen ermöglichen Bundles die Interaktion über Bindungsschnittstellen, nicht über Implementierungen. Eine Dienstschnittstelle sollte mit so wenigen Implementierungsdetails wie möglich angegeben werden.

## Definieren der Schnittstelle

Eine einfache Schnittstelle mit einer Methode zum Zusammenführen von Daten mit der <span class="x x-first x-last">XDP</span>-Vorlage:

```java
package com.mysite.samples;

import com.adobe.aemfd.docmanager.Document;

public interface MyfirstInterface
{
    public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument);
}
 
```

## Implementieren der Schnittstelle.

Erstellen Sie ein neues Paket mit dem Namen `com.mysite.samples.impl`, um die Implementierung der Schnittstelle zu speichern.

```java
package com.mysite.samples.impl;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.OutputServiceException;
import com.mysite.samples.MyfirstInterface;
@Component(service = MyfirstInterface.class)
public class MyfirstInterfaceImpl implements MyfirstInterface {
  @Reference
  OutputService outputService;

  private static final Logger log = LoggerFactory.getLogger(MyfirstInterfaceImpl.class);

  @Override
  public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument) {
    com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
    pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
    try {
      return outputService.generatePDFOutput(xdpTemplate, xmlDocument, pdfOptions);

    } catch (OutputServiceException e) {

      log.error("Failed to merge data with XDP Template", e);

    }

    return null;
  }

}
```

Die Anmerkung `@Component(...)` in Zeile 10 markiert diese Java-Klasse als OSGi-Komponente und registriert sie als OSGi-Dienst.

Die Anmerkung `@Reference` ist Teil der deklarativen OSGi-Dienste und wird verwendet, um einen Verweis auf den [Output-Dienst](https://helpx.adobe.com/de/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) in die Variable `outputService` einzufügen.


## Erstellen und Bereitstellen des Bundles

* Öffnen Sie ein **Eingabeaufforderungsfenster**.
* Navigieren Sie zu `c:\aemformsbundles\mysite\core`.
* Führen Sie den Befehl `mvn clean install -PautoInstallBundle` aus.
* Mit dem obigen Befehl wird das Bundle automatisch erstellt und für Ihre auf „localhost:4502“ ausgeführte AEM-Instanz bereitgestellt.

Das Bundle ist auch am folgenden Speicherort verfügbar: `C:\AEMFormsBundles\mysite\core\target`. Das Bundle kann auch mithilfe der [Felix-Web-Konsole](http://localhost:4502/system/console/bundles) in AEM bereitgestellt werden.

## Verwenden des Dienstes

Sie können den Dienst jetzt auf Ihrer JSP-Seite verwenden. Das folgende Codesnippet zeigt, wie Sie Zugriff auf Ihren Dienst erhalten und die vom Dienst implementierten Methoden verwenden.

```java
MyFirstAEMFormsService myFirstAEMFormsService = sling.getService(com.mysite.samples.MyFirstAEMFormsService.class);
com.adobe.aemfd.docmanager.Document generatedDocument = myFirstAEMFormsService.mergeDataWithXDPTemplate(xdp_or_pdf_template,xmlDocument);
```

Das Beispielpaket mit der JSP-Seite kann [hier heruntergeladen werden](assets/learning_aem_forms.zip).

[Das vollständige Bundle kann hier heruntergeladen werden.](assets/mysite.core-1.0.0-SNAPSHOT.jar)

## Testen des Pakets

Importieren und installieren Sie das Paket mit [Package Manager](http://localhost:4502/crx/packmgr/index.jsp) in AEM.

Verwenden Sie Postman, um einen POST-Aufruf durchzuführen und die Eingabeparameter anzugeben, wie im Screenshot unten dargestellt.
![Postman](assets/test-service-postman.JPG)

## Nächste Schritte

[Erstellen von Sling-Servlet](./create-servlet.md)


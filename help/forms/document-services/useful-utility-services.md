---
title: Nützliche Dienstprogrammdienste
description: Einige nützliche Dienstprogrammdienste für AEM Forms-Entwickelnde
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: add06b73-18bb-4963-b91f-d8e1eb144842
last-substantial-update: 2020-07-07T00:00:00Z
duration: 56
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 100%

---

# Nützliche Dienstprogrammdienste

Dieses Beispielpaket bietet nützliche Dienstprogrammdienste für AEM Forms-Entwicklerinnen und -Entwickler. Folgende Dienste sind verfügbar.


```java
package aemformsutilityfunctions.core;
import java.util.Map;
import com.adobe.aemfd.docmanager.Document;
public interface AemFormsUtilities
{
public abstract com.adobe.aemfd.docmanager.Document createDDXFromMapOfDocuments(Map<String, com.adobe.aemfd.docmanager.Document> paramMap);
public abstract org.w3c.dom.Document w3cDocumentFromStrng(String xmlString);
public abstract com.adobe.aemfd.docmanager.Document orgw3cDocumentToAEMFDDocument(org.w3c.dom.Document xmlDocument);
public abstract String saveDocumentInCrx(String jcrPath,String fileExtension, Document documentToSave);

}
```

Das Beispiel-Bundle kann [hier](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar) heruntergeladen werden.

## Beispiel-Code für die Verwendung von Dienstprogrammdiensten

Im Folgenden finden Sie den Code, der auf der JSP-Seite verwendet wurde, um „org.w3c.dom.Document“ aus der Zeichenfolge zu erstellen und das Dokument zu konvertieren und im CRX-Repository zu speichern, wie im folgenden Code-Snippet gezeigt.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## Voraussetzungen


Sie müssen [DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) bereitstellen und das Bundle starten.


Wenn Sie Dokumente mithilfe dieses Dienstprogramms im CRX-Repository speichern möchten, folgen Sie dem [Artikel zum Entwickeln mit Dienstbenutzenden](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=de#adaptive-forms). Stellen Sie sicher, dass Sie der fd-service-Benutzerin bzw. dem fd-service-Benutzer die [erforderlichen Berechtigungen](http://localhost:4502/useradmin) für die entsprechenden CRX-Ordner erteilen.

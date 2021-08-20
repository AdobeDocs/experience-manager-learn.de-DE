---
title: Nützliche Dienstprogramme
description: Einige nützliche Dienstprogrammdienste für AEM Forms-Entwickler
feature: Adaptive Formulare
version: 6.4,6.5
topic: Entwicklung
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 7%

---


# Nützliche Dienstprogramme

Dieses Beispielpaket bietet nützliche Dienstprogrammdienste, die von einem AEM Forms-Entwickler verwendet werden können. Folgende Dienste sind verfügbar.


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

Das Beispielpaket kann [von hier heruntergeladen werden](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)

## Beispielcode für die Verwendung der Dienstprogramme

Im Folgenden finden Sie den Code, der auf der JSP-Seite verwendet wurde, um org.w3c.dom.Document aus der Zeichenfolge zu erstellen und das Dokument zu konvertieren und im CRX-Repository zu speichern, wie im folgenden Codefragment gezeigt.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## Voraussetzungen


Sie müssen [DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) bereitstellen und das Bundle starten.


Wenn Sie Dokumente mit diesem Dienstprogrammdienst im CRX-Repository speichern möchten, befolgen Sie den Artikel [Entwickeln mit Dienstbenutzern](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms). Stellen Sie sicher, dass Sie die [erforderlichen Berechtigungen](http://localhost:4502/useradmin) für die entsprechenden CRX-Ordner dem Benutzer fd-service bereitstellen.


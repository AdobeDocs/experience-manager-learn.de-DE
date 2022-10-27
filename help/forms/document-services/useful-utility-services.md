---
title: Nützliche Dienstprogramme
description: Einige nützliche Dienstprogrammdienste für AEM Forms-Entwickler
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: add06b73-18bb-4963-b91f-d8e1eb144842
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 5%

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

Das Beispielpaket kann [heruntergeladen von hier](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)

## Beispielcode für die Verwendung der Dienstprogramme

Im Folgenden finden Sie den Code, der auf der JSP-Seite verwendet wurde, um org.w3c.dom.Document aus der Zeichenfolge zu erstellen und das Dokument zu konvertieren und im CRX-Repository zu speichern, wie im folgenden Codefragment gezeigt.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## Voraussetzungen


Sie müssen [Entwickeln mitServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) und starten Sie das Bundle.


Wenn Sie Dokumente mithilfe dieses Dienstprogramms im CRX-Repository speichern möchten, folgen Sie dem [Artikel zum Entwickeln mit Dienstbenutzern](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms). Stellen Sie sicher, dass Sie die [erforderliche Berechtigungen](http://localhost:4502/useradmin) in den entsprechenden CRX-Ordnern an den fd-service-Benutzer.

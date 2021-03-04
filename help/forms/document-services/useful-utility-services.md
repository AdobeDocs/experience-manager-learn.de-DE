---
title: Nützliche Dienstprogramme
description: Einige nützliche Dienstprogramme für AEM Forms-Entwickler
feature: Dokument-Services
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 5%

---


# Nützliche Dienstprogramme

Dieses Beispielpaket bietet nützliche Dienstprogramme, die von einem AEM Forms-Entwickler verwendet werden können. Folgende Dienste sind verfügbar.


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

## Beispielcode zur Verwendung der Dienstprogramme

Der folgende Code wurde auf der JSP-Seite verwendet, um org.w3c.dom.Dokument aus einer Zeichenfolge zu erstellen und das Dokument zu konvertieren und im CRX-Repository zu speichern, wie im folgenden Codefragment dargestellt.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## Voraussetzungen


Sie müssen [DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) bereitstellen und das Bundle Beginn ausführen.


Wenn Sie Dokumente mit diesem Dienstprogrammdienst im CRX-Repository speichern möchten, befolgen Sie die Anweisungen unter [Entwickeln mit Dienstbenutzer-Artikel](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms). Stellen Sie sicher, dass Sie die erforderlichen Berechtigungen für [in den entsprechenden CRX-Ordnern dem fd-service-Benutzer bereitstellen.](http://localhost:4502/useradmin)


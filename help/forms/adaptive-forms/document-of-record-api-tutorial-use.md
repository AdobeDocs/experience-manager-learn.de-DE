---
title: Verwenden der API zum Generieren des Datensatzdokuments mit AEM Forms
description: Programmgesteuerte Generierung des Datensatzdokuments (DOR)
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 9a3b2128-a383-46ea-bcdc-6015105c70cc
last-substantial-update: 2023-01-26T00:00:00Z
duration: 74
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 100%

---

# Verwenden der API zum Generieren des Datensatzdokuments in AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}

Programmgesteuerte Generierung des Datensatzdokuments (DOR)

Dieser Artikel veranschaulicht die Nutzung von `com.adobe.aemds.guide.addon.dor.DoRService API` zur programmatischen Generierung des **Dokumentensatzdokuments**. Das [Datensatzdokument](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html?lang=de) ist eine PDF-Version der Daten, die im adaptiven Formular erfasst werden.

1. Im Folgenden finden Sie das Code-Snippet. Die erste Zeile erhält den DOR-Dienst.
1. Legen Sie die DoROptions fest.
1. Rufen Sie die Render-Methode des DoRService auf und übergeben Sie das DoROptions-Objekt an die Render-Methode

```java
String dataXml = request.getParameter("data");
System.out.println("Got " + dataXml);
Session session;
com.adobe.aemds.guide.addon.dor.DoRService dorService = sling.getService(com.adobe.aemds.guide.addon.dor.DoRService.class);
System.out.println("Got ... DOR Service");
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
System.out.println("Got aem DemoListings");
resourceResolver = aemDemoListings.getFormsServiceResolver();
session = resourceResolver.adaptTo(Session.class);
resource = resourceResolver.getResource("/content/forms/af/sandbox/1201-borrower-payments");
com.adobe.aemds.guide.addon.dor.DoROptions dorOptions = new com.adobe.aemds.guide.addon.dor.DoROptions();
dorOptions.setData(dataXml);
dorOptions.setFormResource(resource);
java.util.Locale locale = new java.util.Locale("en");
dorOptions.setLocale(locale);
com.adobe.aemds.guide.addon.dor.DoRResult dorResult = dorService.render(dorOptions);
byte[] fileBytes = dorResult.getContent();
com.adobe.aemfd.docmanager.Document dorDocument = new com.adobe.aemfd.docmanager.Document(fileBytes);
resource = resourceResolver.getResource("/content/usergenerated/content/aemformsenablement");
Node paydotgov = resource.adaptTo(Node.class);
java.util.Random r = new java.util.Random();
String nodeName = Long.toString(Math.abs(r.nextLong()), 36);
Node fileNode = paydotgov.addNode(nodeName + ".pdf", "nt:file");

System.out.println("Created file Node...." + fileNode.getPath());
Node contentNode = fileNode.addNode("jcr:content", "nt:resource");
Binary binary = session.getValueFactory().createBinary(dorDocument.getInputStream());
contentNode.setProperty("jcr:data", binary);
JSONWriter writer = new JSONWriter(response.getWriter());
writer.object();
writer.key("filePath");
writer.value(fileNode.getPath());
writer.endObject();
session.save();
```

Gehen Sie wie folgt vor, um dies auf Ihrem lokalen System zu testen:

1. [Herunterladen und Installieren von Artikel-Assets mithilfe des Package Managers](assets/dor-with-api.zip)
1. Stellen Sie sicher, dass Sie das DevelopingWithServiceUser-Bundle installiert und gestartet haben, das als Teil des [Artikels zum Erstellen von Dienstbenutzenden](service-user-tutorial-develop.md) bereitgestellt wird.
1. [Bei configMgr anmelden](http://localhost:4502/system/console/configMgr)
1. Suchen Sie nach Apache Sling Service User Mapper Service
1. Vergewissern Sie sich, dass Sie den folgenden Eintrag erhalten: _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ im Abschnitt „Dienstzuordnungen“
1. [Öffnen Sie das Formular](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. Füllen Sie das Formular aus und klicken Sie auf „PDF anzeigen“.
1. Sie sollten DOR auf einer neuen Registerkarte in Ihrem Browser sehen


**Tipps zur Fehlerbehebung**

PDF wird auf der neuen Browser-Registerkarte nicht angezeigt:

1. Vergewissern Sie sich, dass Sie Popups in Ihrem Browser nicht blockieren
1. Stellen Sie sicher, dass Sie den AEM-Server als Administrator starten (zumindest unter Windows)
1. Stellen Sie sicher, dass sich das Bundle „DevelopingWithServiceUser“ im *aktiven Zustand* befindet.
1. [Stellen Sie sicher, dass die Systembenutzerin bzw. der Systembenutzer](http://localhost:4502/useradmin) „fd-service“ Lese-, Änderungs- und Erstellungsberechtigungen für den folgenden Knoten hat `/content/usergenerated/content/aemformsenablement`

---
title: Verwenden der API zum Generieren des Datensatzdokuments mit AEM Forms
seo-title: Verwenden der API zum Generieren des Datensatzdokuments mit AEM Forms
description: Generieren des Datensatzdokuments (DOR) programmgesteuert
seo-description: Verwenden der API zum Generieren des Datensatzdokuments mit AEM Forms
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 94ac3b13-01b4-4198-af81-e5609c80324c
discoiquuid: ba91d9df-dc61-47d8-8e0a-e3f66cae6a87
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 5%

---


# Verwenden der API zum Generieren des Datensatzdokuments in AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}

Generieren des Datensatzdokuments (DOR) programmgesteuert

Dieser Artikel veranschaulicht die Verwendung von `com.adobe.aemds.guide.addon.dor.DoRService API` zum programmgesteuerten Generieren von **Datensatzdokument**. [Das Dokument ](https://docs.adobe.com/content/help/en/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) Recordis ist eine PDF-Version der im adaptiven Formular erfassten Daten.

1. Im Folgenden finden Sie das Code-Snippet. Die erste Zeile erhält den DOR-Dienst.
1. Legen Sie die DoROptions fest.
1. Rufen Sie die Render-Methode des DoRService auf und übergeben Sie das DoROptions-Objekt an die Render-Methode

```java
com.adobe.aemds.guide.addon.dor.DoRService dorService = sling.getService(com.adobe.aemds.guide.addon.dor.DoRService.class);
com.adobe.aemds.guide.addon.dor.DoROptions dorOptions =  new com.adobe.aemds.guide.addon.dor.DoROptions();
 dorOptions.setData(dataXml);
 dorOptions.setFormResource(resource);
 java.util.Locale locale = new java.util.Locale("en");
 dorOptions.setLocale(locale);
 com.adobe.aemds.guide.addon.dor.DoRResult dorResult = dorService.render(dorOptions);
 byte[] fileBytes = dorResult.getContent();
 com.adobe.aemfd.docmanager.Document dorDocument = new com.adobe.aemfd.docmanager.Document(fileBytes);
```

Gehen Sie wie folgt vor, um dies auf Ihrem lokalen System zu testen:

1. [Herunterladen und Installieren von Artikel-Assets mithilfe des Paketmanagers](assets/dor-with-api.zip)
1. Stellen Sie sicher, dass Sie das DevelopingWithServiceUser-Bundle installiert und gestartet haben, das als Teil von [Artikel zum Erstellen von Dienstbenutzern](service-user-tutorial-develop.md) bereitgestellt wird.
1. [Bei configMgr anmelden](http://localhost:4502/system/console/configMgr)
1. Suchen Sie nach Apache Sling Service User Mapper Service .
1. Stellen Sie sicher, dass Sie den folgenden Eintrag _DevelopingWithServiceUser.core:getformsresourceResolver=fd-service_ im Abschnitt Dienstzuordnungen eingeben.
1. [Öffnen Sie das Formular](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. Füllen Sie das Formular aus und klicken Sie auf &quot;PDF anzeigen&quot;
1. Sie sollten DOR auf einer neuen Registerkarte in Ihrem Browser sehen


**Tipps zur Fehlerbehebung**

PDF wird nicht auf der neuen Registerkarte des Browsers angezeigt:

1. Vergewissern Sie sich, dass Sie keine Popups in Ihrem Browser blockieren
1. Vergewissern Sie sich, dass Sie die in diesem [Artikel](service-user-tutorial-develop.md) beschriebenen Schritte ausgeführt haben.
1. Stellen Sie sicher, dass sich das Bundle &quot;DevelopingWithServiceUser&quot;im aktiven Status *a1/> befindet.*
1. Stellen Sie sicher, dass die Daten des Systembenutzers über Lese-, Änderungs- und Erstellungsberechtigungen für den folgenden Knoten verfügen: `/content/usergenerated/content/aemformsenablement`


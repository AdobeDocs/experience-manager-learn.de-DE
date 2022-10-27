---
title: Verwenden der API zum Generieren des Datensatzdokuments mit AEM Forms
description: Generieren des Datensatzdokuments (DOR) programmgesteuert
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 9a3b2128-a383-46ea-bcdc-6015105c70cc
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 5%

---

# Verwenden der API zum Generieren des Datensatzdokuments in AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}

Generieren des Datensatzdokuments (DOR) programmgesteuert

Dieser Artikel veranschaulicht die Verwendung der `com.adobe.aemds.guide.addon.dor.DoRService API` zu generieren **Datensatzdokument** programmgesteuert. [Datensatzdokument](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) ist eine PDF-Version der im adaptiven Formular erfassten Daten.

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
1. Stellen Sie sicher, dass Sie das DevelopingWithServiceUser-Bundle installiert und gestartet haben, das als Teil von [Artikel zum Erstellen von Dienstbenutzern](service-user-tutorial-develop.md)
1. [Bei configMgr anmelden](http://localhost:4502/system/console/configMgr)
1. Suchen Sie nach Apache Sling Service User Mapper Service .
1. Vergewissern Sie sich, dass Sie den folgenden Eintrag erhalten: _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ im Abschnitt &quot;Dienstzuordnungen&quot;
1. [Öffnen Sie das Formular](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. Füllen Sie das Formular aus und klicken Sie auf PDF anzeigen .
1. Sie sollten DOR auf einer neuen Registerkarte in Ihrem Browser sehen


**Tipps zur Fehlerbehebung**

PDF wird in der neuen Browser-Registerkarte nicht angezeigt:

1. Vergewissern Sie sich, dass Sie keine Popups in Ihrem Browser blockieren
1. Befolgen Sie die in diesem Abschnitt beschriebenen Schritte. [Artikel](service-user-tutorial-develop.md)
1. Stellen Sie sicher, dass das Bundle &quot;DevelopingWithServiceUser&quot;in *aktiver Status*
1. Stellen Sie sicher, dass die Daten des Systembenutzers über Lese-, Änderungs- und Erstellungsberechtigungen für den folgenden Knoten verfügen `/content/usergenerated/content/aemformsenablement`

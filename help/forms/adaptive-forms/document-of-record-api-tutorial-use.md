---
title: Verwenden der API zum Generieren eines Dokuments aus Datensatz mit AEM Forms
seo-title: Verwenden der API zum Generieren eines Dokuments aus Datensatz mit AEM Forms
description: Dokument aus Datensatz (DOR) programmgesteuert generieren
seo-description: Verwenden der API zum Generieren eines Dokuments aus Datensatz mit AEM Forms
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 94ac3b13-01b4-4198-af81-e5609c80324c
discoiquuid: ba91d9df-dc61-47d8-8e0a-e3f66cae6a87
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 4%

---


# Verwenden der API zum Generieren eines Dokuments aus Datensatz in AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}

Dokument aus Datensatz (DOR) programmgesteuert generieren

In diesem Artikel wird die Verwendung des `com.adobe.aemds.guide.addon.dor.DoRService API` zur programmatischen Generierung von **Dokument of Record** veranschaulicht. [Dokument of Record](https://docs.adobe.com/content/help/en/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) ist eine PDF-Version der im adaptiven Formular erfassten Daten.

1. Im Folgenden finden Sie das Codefragment. Die erste Zeile ruft den DOR-Dienst ab.
1. Legen Sie die DoROptions fest.
1. Rufen Sie die Methode render des DoRService auf und übergeben Sie das DoROptions-Objekt an die Methode render

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

Gehen Sie wie folgt vor, um dies auf Ihrem lokalen System auszuprobieren

1. [Herunterladen und Installieren der Artikelelemente mit dem Package Manager](assets/dor-with-api.zip)
1. Vergewissern Sie sich, dass Sie das DevelopingWithServiceUser-Bundle installiert und gestartet haben, das als Teil des Artikels &quot;Dienstbenutzer [erstellen&quot;bereitgestellt wurde](service-user-tutorial-develop.md)
1. [Bei configMgr anmelden](http://localhost:4502/system/console/configMgr)
1. Suchen Sie nach Apache Sling Service User Mapper Service .
1. Stellen Sie sicher, dass Sie im Abschnitt &quot;Dienstzuordnungen&quot;den folgenden Eintrag _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ haben:
1. [Öffnen Sie das Formular](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. Füllen Sie das Formular aus und klicken Sie auf Ansicht PDF
1. Sie sollten DOR in der neuen Registerkarte Ihres Browsers sehen


**Tipps zur Fehlerbehebung**

PDF wird auf der neuen Registerkarte des Browsers nicht angezeigt:

1. Vergewissern Sie sich, dass Sie keine Popups in Ihrem Browser blockieren
1. Befolgen Sie die Schritte, die in diesem [Artikel beschrieben werden](service-user-tutorial-develop.md)
1. Stellen Sie sicher, dass das Bundle &quot;DevelopingWithServiceUser&quot;sich im *aktiven Status befindet.*
1. Vergewissern Sie sich, dass die Systembenutzerdaten über die Berechtigungen Lesen, Ändern und Erstellen für den folgenden Knoten verfügen `/content/usergenerated/content/aemformsenablement`


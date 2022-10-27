---
title: Zertifizieren von Dokumenten in AEM Forms
description: Zertifizieren von PDF-Dokumenten mit dem DocAssurance-Dienst in AEM Forms
feature: Document Security
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1471929f-d269-4adc-88ad-2ad3682305e1
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 3%

---

# Zertifizieren von Dokumenten in AEM Forms

Ein zertifiziertes Dokument bietet PDF-Dokumenten- und Formularempfängern zusätzliche Garantien hinsichtlich ihrer Authentizität und Integrität.

Zum Zertifizieren eines Dokuments können Sie Acrobat DC auf dem Desktop oder AEM Forms Document Services als Teil eines automatisierten Prozesses auf einem Server verwenden.

Dieser Artikel enthält ein OSGI-Beispielpaket zum Zertifizieren von PDF-Dokumenten mit AEM Forms Document Services. Der im Beispiel verwendete Code lautet [hier verfügbar](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Gehen Sie wie folgt vor, um Dokumente mit AEM Forms zu zertifizieren

## Zertifikat zum Trust Store hinzufügen {#adding-certificate-to-trust-store}

Gehen Sie wie folgt vor, um das Zertifikat zum Keystore in AEM hinzuzufügen

* [Globalen Trust Store initialisieren](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Suchen Sie nach fd-service](http://localhost:4502/security/users.html) Benutzer
* **Sie müssen auf der Ergebnisseite scrollen, um alle Benutzer zu laden und den fd-service-Benutzer zu finden.**
* Doppelklicken Sie auf den Benutzer fd-service , um das Fenster mit den Benutzereinstellungen zu öffnen.
* Klicken Sie auf &quot;Privaten Schlüssel aus Keystore-Datei hinzufügen&quot;. Geben Sie den Alias und das Kennwort für Ihr Zertifikat an
   ![add-certificate](assets/adding-certificate-keystore.PNG)
* Speichern Sie Ihre Änderungen

## Erstellen von OSGi-Diensten

Sie können Ihr eigenes OSGi-Bundle schreiben und das AEM Forms Client SDK verwenden, um einen Dienst zum Zertifizieren von PDF-Dokumenten zu implementieren. Die folgenden Links sind nützlich, um Ihr eigenes OSGi-Bundle zu schreiben

* [Erstellen des ersten OSGi-Bundles](https://helpx.adobe.com/de/experience-manager/using/maven_arch13.html)
* [Verwenden der Document Service-API](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Alternativ können Sie das im Rahmen dieses Tutorials enthaltene Beispiel-Bundle verwenden.

>[!NOTE]
>
>Das Beispielpaket verwendet den Alias &quot;ares&quot;, um die Dokumente zu zertifizieren. Stellen Sie also sicher, dass Ihr Alias bei Verwendung dieses Bundles &quot;ares&quot;heißt

## Testen des Musters auf Ihrem lokalen System

* Herunterladen und installieren [Custom Document Services Bundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Herunterladen und installieren [Entwickeln mit Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Stellen Sie sicher, dass Sie den folgenden Eintrag im Apache Sling Service User Mapper Service hinzugefügt haben](http://localhost:4502/system/console/configMgr)

   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** wie im Screenshot unten gezeigt
   ![User-Mapper](assets/user-mapper-service.PNG)
* [Beispiel für adaptives Formular importieren](assets/certify-pdf-af.zip)
* [Importieren und Installieren des benutzerdefinierten Sendevorgangs](assets/custom-submit-certify.zip)
* [Öffnen Sie das adaptive Formular](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* Hochladen von PDF-Dokumenten, die zertifiziert werden müssen
   **optional** - Geben Sie das Signaturfeld an, das Sie zum Zertifizieren des Dokuments verwenden möchten
* Klicken Sie auf Senden.
* Die zertifizierte PDF sollte Ihnen zurückgegeben werden.

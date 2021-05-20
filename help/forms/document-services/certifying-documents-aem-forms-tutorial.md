---
title: Zertifizieren von Dokumenten in AEM Forms
seo-title: Zertifizieren von Dokumenten in AEM Forms
description: Zertifizieren von PDF-Dokumenten mit dem DocAssurance-Dienst in AEM Forms
seo-description: Zertifizieren von PDF-Dokumenten mit dem DocAssurance-Dienst in AEM Forms
uuid: ecb1f9b6-bbb3-43a3-a0e0-4c04411acc9f
feature: Dokumentensicherheit
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Entwicklung
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 2%

---


# Zertifizieren von Dokumenten in AEM Forms

Ein zertifiziertes Dokument bietet PDF-Dokumenten- und Formularempfängern zusätzliche Garantien hinsichtlich ihrer Authentizität und Integrität.

Zum Zertifizieren eines Dokuments können Sie Acrobat DC auf dem Desktop oder AEM Forms Document Services als Teil eines automatisierten Prozesses auf einem Server verwenden.

Dieser Artikel enthält ein Beispiel für ein OSGI-Bundle zum Zertifizieren von PDF-Dokumenten mithilfe von AEM Forms Document Services. Der im Beispiel verwendete Code ist [hier](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html) verfügbar.

Gehen Sie wie folgt vor, um Dokumente mit AEM Forms zu zertifizieren

## Zertifikat zum Trust Store hinzufügen {#adding-certificate-to-trust-store}

Führen Sie die unten beschriebenen Schritte aus, um das Zertifikat zum Keystore in AEM hinzuzufügen.

* [Globalen Trust Store initialisieren](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Suchen Sie nach fd-](http://localhost:4502/security/users.html) serviceUser .
* **Sie müssen auf der Ergebnisseite scrollen, um alle Benutzer zu laden und den fd-service-Benutzer zu finden.**
* Doppelklicken Sie auf den Benutzer fd-service , um das Fenster mit den Benutzereinstellungen zu öffnen.
* Klicken Sie auf &quot;Privaten Schlüssel aus Keystore-Datei hinzufügen&quot;. Geben Sie den Alias und das Kennwort für Ihr Zertifikat an
   ![add-certificate](assets/adding-certificate-keystore.PNG)
* Speichern Sie Ihre Änderungen

## Erstellen von OSGi-Diensten

Sie können Ihr eigenes OSGi-Bundle schreiben und das AEM Forms Client SDK verwenden, um einen Dienst zum Zertifizieren von PDF-Dokumenten zu implementieren. Die folgenden Links sind nützlich, um Ihr eigenes OSGi-Bundle zu schreiben

* [Erstellen des ersten OSGi-Bundles](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [Verwenden der Document Service-API](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Alternativ können Sie das im Rahmen dieses Tutorials enthaltene Beispiel-Bundle verwenden.

>[!NOTE]
>
>Das Beispielpaket verwendet den Alias &quot;ares&quot;, um die Dokumente zu zertifizieren. Stellen Sie also sicher, dass Ihr Alias bei Verwendung dieses Bundles &quot;ares&quot;heißt

## Testen des Musters auf Ihrem lokalen System

* Laden Sie [Custom Document Services Bundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar) herunter und installieren Sie es.
* [Entwickeln mit Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar) herunterladen und installieren
* [Stellen Sie sicher, dass Sie den folgenden Eintrag im Apache Sling Service User Mapper Service hinzugefügt haben](http://localhost:4502/system/console/configMgr)

   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-** servicewie im Screenshot unten dargestellt
   ![User-Mapper](assets/user-mapper-service.PNG)
* [Beispiel für adaptives Formular importieren](assets/certify-pdf-af.zip)
* [Importieren und Installieren des benutzerdefinierten Sendevorgangs](assets/custom-submit-certify.zip)
* [Öffnen Sie das adaptive Formular](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* Hochladen von PDF-Dokumenten, die zertifiziert werden müssen
   **optional**  - Geben Sie das Signaturfeld an, das Sie zum Zertifizieren des Dokuments verwenden möchten
* Klicken Sie auf Senden.
* Zertifizierte PDF-Dateien sollten an Sie zurückgegeben werden.



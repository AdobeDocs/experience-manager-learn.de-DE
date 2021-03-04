---
title: Zertifiziertes Dokument in AEM Forms
seo-title: Zertifiziertes Dokument in AEM Forms
description: Mit dem Document Security-Dienst PDF-Dokumente in AEM Forms zertifizieren
seo-description: Mit dem Document Security-Dienst PDF-Dokumente in AEM Forms zertifizieren
uuid: ecb1f9b6-bbb3-43a3-a0e0-4c04411acc9f
feature: Dokumentensicherheit
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Entwicklung
role: Entwickler
level: Zwischenschaltung
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 2%

---


# Zertifiziertes Dokument in AEM Forms

Ein zertifiziertes Dokument bietet PDF-Dokument- und Forms-Empfängern zusätzliche Garantien hinsichtlich Authentizität und Integrität.

Um ein Dokument zu zertifizieren, können Sie Acrobat DC auf dem Desktop oder AEM Forms Dokument Services als Teil eines automatisierten Serverprozesses verwenden.

In diesem Artikel finden Sie Beispiel-OSGI-Bundle zum Zertifizieren von PDF-Dokumenten mit AEM Forms Dokument Services.Der im Beispiel verwendete Code ist [hier verfügbar](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Zur Zertifizierung von Dokumenten mit AEM Forms müssen die folgenden Schritte durchgeführt werden

## Zertifikat zum Trust Store hinzufügen {#adding-certificate-to-trust-store}

Gehen Sie wie folgt vor, um das Zertifikat zu Keystore in AEM hinzuzufügen

* [Globalen Trust Store initialisieren](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Suchen nach fd-](http://localhost:4502/security/users.html) serviceUser
* **Sie müssen einen Bildlauf auf der Ergebnisseite durchführen, um alle Benutzer zu laden, um den fd-service-Benutzer zu finden.**
* Dublette klicken Sie auf den fd-service-Benutzer, um das Fenster mit den Benutzereinstellungen zu öffnen
* Klicken Sie auf &quot;Hinzufügen privaten Schlüssel aus der Keystore-Datei&quot;.Geben Sie den Alias und das Kennwort für Ihr Zertifikat an
   ![add-certificate](assets/adding-certificate-keystore.PNG)
* Speichern Sie Ihre Änderungen

## OSGI-Dienst erstellen

Sie können Ihr eigenes OSGi-Bundle schreiben und mit dem AEM Forms Client SDK einen Dienst zum Zertifizieren von PDF-Dokumenten implementieren. Die folgenden Links sind nützlich, um Ihr eigenes OSGi-Bundle zu schreiben

* [Erstellen des ersten OSGi-Bundles](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [Dokument Service API verwenden](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Sie können auch das Beispielpaket verwenden, das als Teil dieser Lernelemente enthalten ist.

>[!NOTE]
>
>Das Beispielpaket verwendet den Alias &quot;ares&quot;, um die Dokumente zu zertifizieren. Achten Sie daher bei der Verwendung dieses Bundles darauf, dass Ihr Alias &quot;ares&quot;heißt

## Testen des Musters auf Ihrem lokalen System

* Laden Sie das [Bundle für benutzerdefinierte Dokument-Services herunter und installieren Sie es.](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [Entwickeln mit Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar) herunterladen und installieren
* [Vergewissern Sie sich, dass Sie den folgenden Eintrag im Apache Sling Service User Mapper Service hinzugefügt haben](http://localhost:4502/system/console/configMgr)

   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-** services, wie im folgenden Screenshot gezeigt
   ![User-Mapper](assets/user-mapper-service.PNG)
* [Beispiel für adaptives Formular importieren](assets/certify-pdf-af.zip)
* [Benutzerspez. Senden importieren und installieren](assets/custom-submit-certify.zip)
* [Adaptives Formular öffnen](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* PDF-Dokument hochladen, das zertifiziert werden muss
   **optional**  - Geben Sie das Signaturfeld an, das zum Zertifizieren des Dokuments verwendet werden soll
* Klicken Sie auf Senden.
* Zertifizierte PDF-Dateien sollten an Sie zurückgegeben werden.



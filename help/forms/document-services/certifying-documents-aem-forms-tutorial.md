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
duration: 105
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 100%

---

# Zertifizieren von Dokumenten in AEM Forms

Ein zertifiziertes Dokument bietet den Empfängerinnen und Empfängern von PDF-Dokumenten und -Formularen eine zusätzliche Garantie hinsichtlich seiner Authentizität und Integrität.

Zum Zertifizieren eines Dokuments können Sie Acrobat DC auf dem Desktop oder AEM Forms Document Services als Teil eines automatisierten Prozesses auf einem Server verwenden.

Dieser Artikel enthält ein Beispiel-OSGi-Bundle zum Zertifizieren von PDF-Dokumenten mit AEM Forms Document Services. Der im Beispiel verwendete Code ist [hier verfügbar](https://helpx.adobe.com/de/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Gehen Sie wie folgt vor, um Dokumente mit AEM Forms zu zertifizieren

## Hinzufügen eines Zertifikats zum Trust Store {#adding-certificate-to-trust-store}

Gehen Sie wie folgt vor, um das Zertifikat zum Keystore in AEM hinzuzufügen

* [Den Global Trust Store initialisieren](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Nach fd-service-Benutzer suchen](http://localhost:4502/security/users.html)
* **Sie müssen auf der Ergebnisseite scrollen, um alle Benutzenden zu laden und den fd-service-Benutzer zu finden**
* Auf den fd-service-Benutzer doppelklicken, um das Fenster mit den Benutzereinstellungen zu öffnen
* Auf „Privaten Schlüssel aus Keystore-Datei hinzufügen“ klicken und den Alias und das Kennwort für Ihr Zertifikat angeben
  ![add-certificate](assets/adding-certificate-keystore.PNG)
* Speichern Sie Ihre Änderungen

## Erstellen von OSGi-Diensten

Sie können Ihr eigenes OSGi-Bundle schreiben und mit dem AEM Forms Client SDK einen Dienst implementieren, um PDF-Dokumente zu zertifizieren. Die folgenden Links sind nützlich, um Ihr eigenes OSGi-Bundle zu schreiben

* [Erstellen Ihres ersten OSGi-Bundles](https://helpx.adobe.com/de/experience-manager/using/maven_arch13.html)
* [Verwenden der Document Service-API](https://helpx.adobe.com/de/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Alternativ können Sie das im Rahmen dieses Tutorials enthaltene Beispiel-Bundle verwenden.

>[!NOTE]
>
>Das Beispiel-Bundle verwendet den Alias „ares“ zum Zertifizieren der Dokumente. Stellen Sie also sicher, dass Ihr Alias bei Verwendung dieses Bundles „ares“ heißt

## Testen des Beispiels auf Ihrem lokalen System

* Laden Sie [Custom Document Services Bundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar) herunter und installieren Sie es
* Laden Sie [Developing with Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar) herunter und installieren Sie es
* [Stellen Sie sicher, dass Sie den folgenden Eintrag im Apache Sling Service User Mapper Service hinzugefügt haben](http://localhost:4502/system/console/configMgr)
  **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** wie im Screenshot unten gezeigt
  ![User-Mapper](assets/user-mapper-service.PNG)
* [Importieren Sie ein adaptives Beispielformular](assets/certify-pdf-af.zip)
* [Importieren und installieren Sie den benutzerdefinierten Sendevorgang](assets/custom-submit-certify.zip)
* [Öffnen Sie das adaptive Formular](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* Laden Sie das zu zertifizierende PDF-Dokument hoch
  **optional** – geben Sie das Signaturfeld an, das Sie zum Zertifizieren des Dokuments verwenden möchten
* Klicken Sie auf „Senden“
* Das zertifizierte PDF-Dokument sollte zurückgegeben werden.

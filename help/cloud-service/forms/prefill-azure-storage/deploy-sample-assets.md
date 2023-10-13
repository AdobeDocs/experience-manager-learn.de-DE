---
title: Bereitstellen der Beispiel-Assets
description: Stellen Sie die Beispiel-Assets auf Ihrem lokalen Cloud-fähigen System bereit.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 13717
exl-id: ae8104fa-7af2-49c2-9e6b-704152d49149
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---

# Bereitstellen der Beispiel-Assets

Damit dieses Anwendungsbeispiel auf Ihrem System funktioniert, stellen Sie die folgenden Assets auf Ihrem lokalen Cloud-fähigen System bereit.

* Stellen Sie sicher, dass Sie alle erforderlichen Konfigurationen/Konten erstellt haben, die im[Einführungsdokument](./introduction.md)

* [Installieren Sie die benutzerdefinierte adaptive Formularvorlage und die zugehörige Seitenkomponente](./assets/azure-portal-template-page-component.zip)

* [Installieren des SendGrid-Formulardatenmodells](./assets/send-grid-form-data-model.zip). Sie müssen Ihren API-Schlüssel angeben und SendGrid von der Adresse aus überprüft haben, damit dieses Formulardatenmodell funktioniert. Testen des Formulardatenmodells im Formulardatenmodell-Editor

* [Installieren Sie das Azure-unterstützte Formulardatenmodell](./assets/azure-storage-fdm.zip). Sie müssen Ihre Anmeldedaten für das Azure Storage-Konto angeben, damit das Formulardatenmodell funktioniert. Testen Sie das Formulardatenmodell im Formulardatenmodell-Editor.

* [Importieren des adaptiven Beispielformulars](./assets/credit-applications-af.zip)
* [Client-Bibliothek importieren](./assets/client-lib.zip)
* [Formularvorschau](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/creditapplications/jcr:content?wcmmode=disabled). Geben Sie eine gültige E-Mail ein und klicken Sie auf die Schaltfläche Speichern . Die Formulardaten sollten im Azure-Speicher gespeichert werden und eine E-Mail mit einem Link zum gespeicherten Formular wird an die angegebene E-Mail-Adresse gesendet.

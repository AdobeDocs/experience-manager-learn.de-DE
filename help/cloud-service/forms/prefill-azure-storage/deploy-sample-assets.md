---
title: Bereitstellen der Beispiel-Assets
description: Stellen Sie die Beispiel-Assets auf Ihrem lokalen Cloud-fähigen System bereit.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-13717
exl-id: ae8104fa-7af2-49c2-9e6b-704152d49149
duration: 38
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '188'
ht-degree: 100%

---

# Bereitstellen der Beispiel-Assets

Damit dieser Anwendungsfall auf Ihrem System funktioniert, stellen Sie die folgenden Assets auf Ihrem lokalen Cloud-fähigen System bereit.

* Stellen Sie sicher, dass Sie alle in der [Einführung](./introduction.md) genannten erforderlichen Konfigurationen/Konten erstellt haben.

* [Installieren Sie die benutzerdefinierte adaptive Formularvorlage und die zugehörige Seitenkomponente.](./assets/azure-portal-template-page-component.zip)

* [Installieren Sie das SendGrid-Formulardatenmodell.](./assets/send-grid-form-data-model.zip) Sie müssen Ihren API-Schlüssel und Ihre verifizierte SendGrid-Absenderadresse angeben, damit dieses Formulardatenmodell verwendet werden kann. Testen Sie das Formulardatenmodell im Formulardatenmodell-Editor.

* [Installieren Sie das Azure-basierte Formulardatenmodell.](./assets/azure-storage-fdm.zip) Sie müssen Ihre Anmeldeinformationen für das Azure Storage-Konto angeben, damit das Formulardatenmodell verwendet werden kann. Testen Sie das Formulardatenmodell im Formulardatenmodell-Editor.

* [Importieren Sie das adaptive Beispielformular.](./assets/credit-applications-af.zip)
* [Importieren Sie die Client-Bibliothek.](./assets/client-lib.zip)
* [Zeigen Sie das Formular in einer Vorschau an.](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/creditapplications/jcr:content?wcmmode=disabled) Geben Sie eine gültige E-Mail ein und klicken Sie auf die Schaltfläche „Speichern“. Die Formulardaten sollten in Azure Storage gespeichert werden und eine E-Mail mit einem Link zum gespeicherten Formular wird an die angegebene E-Mail-Adresse gesendet.

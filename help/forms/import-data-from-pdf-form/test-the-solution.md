---
title: Importieren von Daten aus einer PDF-Datei in ein adaptives Formular
description: Tutorial zum Ausfüllen eines adaptiven Formulars durch Importieren einer PDF-Datei
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
source-git-commit: 17ab178f385619b589a9dde6089410bfa4515ffa
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 3%

---

# Bereitstellen der Beispiel-Assets

Sie können Beispiel-Assets bereitstellen, damit diese Lösung in Ihrer lokalen AEM Forms-Instanz funktioniert

* [Importieren Sie die Client-Bibliothek und die benutzerdefinierte Komponente zum Hochladen des PDF-Formulars über Package Manager.](./assets/client-libs-custom-component.zip)
* Laden Sie das Bundle mithilfe der OSGi-Web-Konsole herunter und stellen Sie es bereit[Custom Document Services Bundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Laden Sie das Bundle mithilfe der OSGi-Web-Konsole herunter und stellen Sie es bereit [Entwickeln mit Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Laden Sie das Bundle mithilfe der OSGi-Web-Konsole herunter und stellen Sie es bereit[Daten aus einer PDF-Datei importieren](./assets/onlineToOffline.core-1.0.0-SNAPSHOT.jar)
* Eintrag hinzufügen _**DevelopingWithServiceUser.core:getresourceresolver=data**_ im _**Apache Sling Service User Mapper Service**_ OSGi-Konfigurationskonsole
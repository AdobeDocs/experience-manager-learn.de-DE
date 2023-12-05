---
title: Importieren von Daten aus einer PDF-Datei in ein adaptives Formular
description: Tutorial zum Ausfüllen eines adaptiven Formulars durch Importieren einer PDF-Datei
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f21753b2-f065-4556-add4-b1983fb57031
duration: 36
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 100%

---

# Bereitstellen der Beispiel-Assets

Sie können die Beispiel-Assets bereitstellen, damit diese Lösung in Ihrer lokalen AEM Forms-Instanz funktioniert

* [Importieren Sie die Client-Bibliothek und die benutzerdefinierte Komponente, um das PDF-Formular über Package Manager hochzuladen.](./assets/client-libs-custom-component.zip)
* Laden Sie das Bundle mithilfe der OSGi-Web-Konsole herunter und stellen Sie es bereit[Custom Document Services-Bundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Laden Sie das Bundle mithilfe der OSGi-Web-Konsole herunter und stellen Sie es bereit [Bundle zur Entwicklung mit Dienstbenutzenden](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Laden Sie das Bundle mithilfe der OSGi-Web-Konsole herunter und stellen Sie es bereit[Importieren von Daten aus der PDF-Datei](./assets/onlineToOffline.core-1.0.0-SNAPSHOT.jar)
* Fügen Sie den Eintrag _**DevelopingWithServiceUser.core:getresourceresolver=data**_ in der OSGi-Konfigurationskonsole _**Apache Sling Service User Mapper Service**_ hinzu

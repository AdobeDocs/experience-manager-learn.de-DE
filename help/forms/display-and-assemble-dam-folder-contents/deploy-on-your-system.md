---
title: Lokale Bereitstellung der Assets
description: Bereitstellen der Tutorial-Assets in Ihrer lokalen AEM-Instanz
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
source-git-commit: ddef90067d3ae4a3c6a705b5e109e474bab34f6d
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 3%

---

# Auf Ihrem System bereitstellen

Führen Sie die unten aufgeführten Schritte aus, um dieses Anwendungsbeispiel in Ihrer lokalen AEM-Instanz zu verwenden.

* [Bereitstellen des DevelopingWithServiceUser-Bundles](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip) in der ZIP-Datei enthalten ist.

* Fügen Sie den folgenden Eintrag im Apache Sling Service User Mapper-Dienst hinzu **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** mithilfe der [configMgr](http://localhost:4502/system/console/configMgr).

* [Bereitstellen des Newsletter-Bundles](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). Dieses Bundle enthält den Code zum Auflisten des Ordnerinhalts und Zusammenführen der ausgewählten Newsletter.

* [Importieren Sie das Paket mit Package Manager](assets/newsletter.zip). Dieses Paket enthält Client-Bibliothek und PDF-Beispieldateien zum Testen der Lösung.

* [Importieren des adaptiven Beispielformulars](assets/sample-adaptive-form.zip). In diesem Formular werden die Newsletter aufgelistet, die ausgewählt werden können.

* [Formularvorschau](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
Wählen Sie ein paar Newsletter zum Herunterladen aus. Die ausgewählten Newsletter werden zu einem PDF zusammengefasst und an Sie zurückgegeben.





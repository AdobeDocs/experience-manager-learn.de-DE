---
title: Lokales Bereitstellen der Assets
description: Bereitstellen der Tutorial-Assets in Ihrer lokalen AEM-Instanz
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
exl-id: d23b51ba-1efb-4505-b5b3-44a02177e467
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: ht
source-wordcount: '158'
ht-degree: 100%

---

# Bereitstellen auf Ihrem System

Führen Sie die unten aufgeführten Schritte aus, um diesen Anwendungsfall in Ihrer lokalen AEM-Instanz zu verwenden.

* [Stellen Sie das DevelopingWithServiceUser-Bundle bereit](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=de), das in der ZIP-Datei enthalten ist.

* Fügen Sie den Eintrag **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** mithilfe von [configMgr](http://localhost:4502/system/console/configMgr) zum Apache Sling Service User Mapper Service hinzu.

* [Stellen Sie das Newsletter-Bundle bereit](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). Dieses Bundle enthält den Code zum Auflisten des Ordnerinhalts und zum Zusammenführen der ausgewählten Newsletter.

* [Importieren Sie das Paket mit Package Manager](assets/newsletter.zip). Dieses Paket enthält die Client-Bibliothek und PDF-Beispieldateien zum Testen der Lösung.

* [Importieren Sie das adaptive Beispielformular](assets/sample-adaptive-form.zip). In diesem Formular werden die Newsletter aufgelistet, die zur Auswahl stehen.

* [Zeigen Sie das Formular in einer Vorschau an](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
Wählen Sie ein paar Newsletter zum Herunterladen aus. Die ausgewählten Newsletter werden in einer PDF-Datei zusammengefasst und an Sie zurückgegeben.

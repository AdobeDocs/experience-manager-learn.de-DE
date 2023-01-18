---
title: Lokale Bereitstellung der Assets
description: Bereitstellen der Tutorial-Assets in Ihrer lokalen AEM-Instanz
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
source-git-commit: 7815b1a78949c433f2c53ff752bf39dd55f9ac94
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---

# Auf Ihrem System bereitstellen

Führen Sie die unten aufgeführten Schritte aus, um dieses Anwendungsbeispiel in Ihrer lokalen AEM-Instanz zu verwenden.

* [Konfigurieren Sie die Verwendung des fd-service-Benutzers, indem Sie die in diesem Artikel erwähnten Schritte ausführen.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en). Stellen Sie sicher, dass Sie das DevelopingWithServiceUser-Bundle bereitgestellt haben.

* [Bereitstellen des Newsletter-Bundles](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). Dieses Bundle enthält den Code zum Auflisten des Ordnerinhalts und Zusammenführen der ausgewählten Newsletter.

* [Importieren Sie das Paket mit Package Manager](assets/newsletter.zip). Dieses Paket enthält Client-Bibliothek und PDF-Beispieldateien zum Testen der Lösung.

* [Importieren des adaptiven Beispielformulars](assets/sample-adaptive-form.zip). In diesem Formular werden die Newsletter aufgelistet, die ausgewählt werden können.

* [Formularvorschau](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
Wählen Sie ein paar Newsletter zum Herunterladen aus. Die ausgewählten Newsletter werden zu einem PDF zusammengefasst und an Sie zurückgegeben.





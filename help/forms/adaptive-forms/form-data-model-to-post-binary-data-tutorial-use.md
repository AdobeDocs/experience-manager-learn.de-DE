---
title: Verwenden des Formulardatenmodells zum Posten binärer Daten
description: Posten von Binärdaten an AEM DAM mithilfe des Formulardatenmodells
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9c62a7d6-8846-424c-97b8-2e6e3c1501ec
last-substantial-update: 2021-01-09T00:00:00Z
duration: 106
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 100%

---

# Verwenden des Formulardatenmodells zum Posten binärer Daten{#using-form-data-model-to-post-binary-data}

Ab AEM Forms 6.4 können wir jetzt den Formulardatenmodelldienst als Schritt in AEM-Workflows aufrufen. Dieser Artikel führt Sie durch ein Anwendungsbeispiel zum Posten von Datensatzdokumenten mit dem Formulardatenmodelldienst.

Der Anwendungsfall sieht folgendermaßen aus:

1. Eine Benutzerin oder ein Benutzer füllt das adaptive Formular aus und sendet es.
1. Das adaptive Formular ist so konfiguriert, dass es ein Datensatzdokument generiert.
1. Bei Übermittlung dieser adaptiven Formulare wird ein AEM-Workflow ausgelöst, der den Formulardatenmodelldienst aufruft, um das Datensatzdokument nach AEM DAM zu senden (POST).

![posttodam](assets/posttodamshot1.png)

Registerkarte „Formulardatenmodell“ – Eigenschaften

Auf der Registerkarte „Dienst-Eingabe“ ist Folgendes zugeordnet:

* file: (Das zu speichernde Binärobjekt) mit der Eigenschaft DOR.pdf relativ zur Payload. Das bedeutet, dass beim Senden des adaptiven Formulars das generierte Datensatzdokument in einer Datei namens DOR.pdf relativ zur Payload des Workflows gespeichert wird.**Stellen Sie sicher, dass diese DOR.pdf dieselbe ist, die Sie bei der Konfiguration der Übermittlungseigenschaften des adaptiven Formulars angegeben haben.**

* fileName: Dies ist der Name, mit dem das binäre Objekt in DAM gespeichert wird. Sie möchten also, dass diese Eigenschaft dynamisch generiert wird, sodass jeder fileName pro Übermittlung eindeutig ist. Zu diesem Zweck haben wir den Prozessschritt im Workflow verwendet, um die Metadateneigenschaft „filename“ zu erstellen und ihren Wert auf eine Kombination aus Mitgliedsnamen und Kontonummer der Person festzulegen, die das Formular sendet. Wenn beispielsweise der Mitgliedsname der Person Johann Jakob lautet und seine Kontonummer 9846 ist, lautet der Dateiname Johann Jakob_9846.pdf

![fdmserviceinput](assets/fdminputservice.png)

Dienst-Eingabe

>[!NOTE]
>
>Tipps zur Fehlerbehebung: Wenn DOR.pdf aus irgendeinem Grund nicht in DAM erstellt wird, setzen Sie die Authentifizierungseinstellungen der Datenquelle zurück, indem Sie [hier](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam) klicken. Dies sind die AEM-Authentifizierungseinstellungen, die standardmäßig „admin/admin“ lauten.

Um diese Funktion auf Ihrem Server zu testen, führen Sie die folgenden Schritte aus:

1.[Stellen Sie das Developingwithserviceuser-Bundle bereit](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Laden Sie das setvalue-Bundle herunter und stellen Sie es bereit.](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) Dieses benutzerdefinierte OSGi-Bundle wird verwendet, um Metadateneigenschaften zu erstellen und ihren Wert anhand der übermittelten Formulardaten festzulegen.

1. [Importieren Sie die zu diesem Artikel gehörenden Assets](assets/postdortodam.zip) mit dem Package Manager in AEM. Dann erhalten Sie Folgendes:

   1. Workflow-Modell
   1. Adaptives Formular, das für die Übermittlung an den AEM-Workflow konfiguriert wurde
   1. Datenquelle, die für die Verwendung der Datei „PostToDam.JSON“ konfiguriert ist
   1. Formulardatenmodell, das die Datenquelle verwendet

1. [Öffnen Sie mit Ihrem Browser das adaptive Formular.](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
1. Füllen Sie das Formular aus und senden Sie es ab
1. Überprüfen Sie in der Assets-Anwendung, ob das Datensatzdokument erstellt und gespeichert wurde


[Swagger File](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile), das bei der Erstellung der Datenquelle verwendet wurde, ist zu Ihrer Information verfügbar

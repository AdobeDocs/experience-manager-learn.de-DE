---
title: Verwenden des Formulardatenmodells zum Posten binärer Daten
seo-title: Verwenden des Formulardatenmodells zum Posten binärer Daten
description: Veröffentlichen binärer Daten in AEM DAM mithilfe des Formulardatenmodells
seo-description: Veröffentlichen binärer Daten in AEM DAM mithilfe des Formulardatenmodells
uuid: dd344ed8-69f7-4d63-888a-3c96993fe99d
feature: Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 6e99df7d-c030-416b-83d2-24247f673b33
topic: Entwicklung
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 1%

---


# Verwenden des Formulardatenmodells zum Posten binärer Daten{#using-form-data-model-to-post-binary-data}

Ab AEM Forms 6.4 können wir jetzt den Formulardatenmodelldienst als Schritt in AEM Workflow aufrufen. Dieser Artikel führt Sie durch ein Anwendungsbeispiel zum Posten von Datensatzdokumenten mit dem Formulardatenmodelldienst.

Der Anwendungsfall sieht folgendermaßen aus:

1. Ein Benutzer füllt das adaptive Formular aus und sendet es.
1. Das adaptive Formular ist so konfiguriert, dass es das Datensatzdokument generiert.
1. Bei Übermittlung dieser adaptiven Formulare wird AEM Workflow ausgelöst, der den Formulardatenmodelldienst aufruft, um das Datensatzdokument in AEM DAM POST.

![posttodam](assets/posttodamshot1.png)

Registerkarte &quot;Formulardatenmodell&quot;- Eigenschaften

Auf der Registerkarte Diensteingabe ist Folgendes zugeordnet:

* file(Das binäre Objekt, das gespeichert werden muss) mit der Eigenschaft DOR.pdf relativ zur Payload. Das bedeutet, dass beim Senden des adaptiven Formulars das generierte Datensatzdokument in einer Datei namens DOR.pdf relativ zur Workflow-Nutzlast gespeichert wird.**Stellen Sie sicher, dass diese DOR.pdf-Datei mit der beim Konfigurieren der Sendeeigenschaft des adaptiven Formulars angegebenen übereinstimmt.**

* fileName - Dies ist der Name, mit dem das binäre Objekt in DAM gespeichert wird. Sie möchten also, dass diese Eigenschaft dynamisch generiert wird, sodass jeder fileName pro Übermittlung eindeutig ist. Zu diesem Zweck haben wir den Prozessschritt im Workflow verwendet, um die Metadateneigenschaft &quot;filename&quot;zu erstellen und ihren Wert auf eine Kombination aus &quot;Member Name&quot;und &quot;Account Number&quot;der Person festzulegen, die das Formular sendet. Wenn beispielsweise der Name des Mitglieds der Person John Jacobs lautet und seine Kontonummer 9846 lautet, lautet der Dateiname John Jacobs_9846.pdf

![fdmserviceinput](assets/fdminputservice.png)

Service Input

>[!NOTE]
>
>Tipps zur Fehlerbehebung - Wenn DOR.pdf aus irgendeinem Grund nicht in DAM erstellt wird, setzen Sie die Authentifizierungseinstellungen der Datenquelle zurück, indem Sie auf [hier](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam) klicken. Dies sind die AEM Authentifizierungseinstellungen, die standardmäßig &quot;admin/admin&quot;lauten.

Um diese Funktion auf Ihrem Server zu testen, führen Sie die folgenden Schritte aus:

1.[Stellen Sie das Developing With ServiceUser Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar) bereit.

1. [Laden Sie das SetValue-Bundle herunter und stellen Sie es bereit](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dieses benutzerdefinierte OSGI-Bundle wird verwendet, um Metadateneigenschaften zu erstellen und den Wert aus den gesendeten Formulardaten festzulegen.

1. [Importieren Sie die mit diesem Artikel verknüpften ](assets/postdortodam.zip) Assets mit dem Package Manager in AEM. Sie erhalten Folgendes

   1. Workflow-Modell
   1. Adaptives Formular für die Übermittlung an den AEM Workflow konfiguriert
   1. Datenquelle, die für die Verwendung der Datei PostToDam.JSON konfiguriert ist
   1. Formulardatenmodell, das die Datenquelle verwendet

1. Zeigen Sie auf Ihren [Browser, um das adaptive Formular](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled) zu öffnen.
1. Füllen Sie das Formular aus und senden Sie es.
1. Überprüfen Sie die Anwendung Assets , ob das Datensatzdokument erstellt und gespeichert wurde.


[Die Swagger-](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile) Datei, die beim Erstellen der Datenquelle verwendet wird, steht für Ihre Referenz zur Verfügung

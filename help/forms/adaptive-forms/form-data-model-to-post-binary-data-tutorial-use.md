---
title: Verwenden des Formulardatenmodells zum Posten binärer Daten
description: Veröffentlichen binärer Daten in AEM DAM mithilfe des Formulardatenmodells
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9c62a7d6-8846-424c-97b8-2e6e3c1501ec
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 0%

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
>Tipps zur Fehlerbehebung - Wenn DOR.pdf aus irgendeinem Grund nicht in DAM erstellt wird, setzen Sie die Authentifizierungseinstellungen der Datenquelle zurück, indem Sie auf [here](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam). Dies sind die AEM Authentifizierungseinstellungen, die standardmäßig &quot;admin/admin&quot;lauten.

Um diese Funktion auf Ihrem Server zu testen, führen Sie die folgenden Schritte aus:

1.[Bereitstellen des Entwicklungs-mit-Service-Benutzer-Bundles](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Herunterladen und Bereitstellen des setvalue-Bundles](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar).Dieses benutzerdefinierte OSGi-Bundle wird verwendet, um Metadateneigenschaften zu erstellen und den Wert aus den gesendeten Formulardaten festzulegen.

1. [Importieren der Assets](assets/postdortodam.zip) mit diesem Artikel in AEM mithilfe des Paketmanagers verknüpft sind. Sie erhalten Folgendes

   1. Workflow-Modell
   1. Adaptives Formular für die Übermittlung an den AEM Workflow konfiguriert
   1. Datenquelle, die für die Verwendung der Datei PostToDam.JSON konfiguriert ist
   1. Formulardatenmodell, das die Datenquelle verwendet

1. Verweisen Sie auf [Browser zum Öffnen des adaptiven Formulars](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
1. Füllen Sie das Formular aus und senden Sie es.
1. Überprüfen Sie die Anwendung Assets , ob das Datensatzdokument erstellt und gespeichert wurde.


[Swagger-Datei](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile) zum Erstellen der Datenquelle verwendet wird, ist für Ihre Referenz verfügbar

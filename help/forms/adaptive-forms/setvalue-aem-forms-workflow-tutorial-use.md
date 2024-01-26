---
title: Verwenden von setValue im AEM Forms-Workflow
description: Festlegen des Werts eines Elements in von Adaptive Forms übermittelten Daten in AEM Forms OSGI
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 3919efee-6998-48e8-85d7-91b6943d23f9
last-substantial-update: 2020-01-09T00:00:00Z
duration: 134
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 100%

---

# Verwenden von setValue im AEM Forms-Workflow

Festlegen des Werts eines XML-Elements in von Adaptive Forms übermittelten Daten im AEM Forms OSGi-Workflow.

![SetValue](assets/setvalue.png)

LiveCycle verfügte früher über eine Komponente mit festgelegtem Wert, mit der Sie den Wert eines XML-Elements festlegen konnten.

Basierend auf diesem Wert können Sie bestimmte Felder oder Bedienfelder des Formulars ausblenden/deaktivieren, wenn das Formular mit der XML ausgefüllt ist.

In AEM Forms OSGi müssen wir ein benutzerdefiniertes OSGi-Bundle schreiben, um den Wert in der XML festzulegen. Das Bundle wird im Rahmen dieses Tutorials bereitgestellt.
Wir verwenden den Prozessschritt in AEM-Workflow. Wir verknüpfen diesen Prozessschritt mit dem OSGi-Bundle „Set Value of Element in XML“.
Wir müssen zwei Argumente an das Bundle zum Festlegen der Werte übergeben. Das erste Argument ist der XPath des XML-Elements, dessen Wert festgelegt werden soll. Das zweite Argument ist der Wert, der festgelegt werden soll.
Im obigen Screenshot ist beispielsweise für das initialStep-Element der Wert „N“ festgelegt.
Basierend auf diesem Wert werden bestimmte Bedienfelder in den adaptiven Formularen ausgeblendet bzw. angezeigt.
In unserem Beispiel haben wir einen einfachen Urlaubsantrag vorliegen. Die Person, die dieses Formular aufgerufen hat, gibt ihren Namen und die Urlaubstage ein. Bei Übermittlung wird dieses Formular zur Überprüfung an „admin“ gesendet. Wenn Administrierende das Formular öffnen, sind die Felder im ersten Bedienfeld deaktiviert. Dies liegt daran, dass wir den Wert des initialStep-Elements in der XML auf „N“ festgelegt haben.

Basierend auf dem Wert des initialStep-Felds zeigen wir das zweite Bedienfeld, in dem „admin“ die Anfrage genehmigen oder ablehnen kann

Sehen Sie sich die mit dem Regeleditor festgelegten Regeln im Feld „Ausfallzeit angefordert von“ an.

Gehen Sie wie folgt vor, um die Assets auf Ihrem lokalen System bereitzustellen:

* [Stellen Sie das Bundle „DevelopingWithServiceUser“ bereit.](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Bereitstellen des Beispielpakets](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dies ist das benutzerdefinierte OSGi-Bundle, mit dem Sie die Werte eines Elements in den übermittelten XML-Daten festlegen können

* [Laden Sie die ZIP-Datei herunter und extrahieren Sie deren Inhalt.](assets/setvalueassets.zip)
* Lassen Sie Ihren Browser auf [Package Manager](http://localhost:4502/crx/packmgr/index.jsp) verweisen
* Importieren und installieren Sie setValueWorkflow.zip. Dies weist das Beispiel-Workflow-Modell auf.
* Lassen Sie Ihren Browser auf [Formulare und Dokumente](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) verweisen
* Klicken Sie auf „Erstellen“ > „Datei hochladen“.
* Hochladen von TimeOfRequestForm.zip
* Öffnen Sie [TimeOffRequestform](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* Füllen Sie die drei erforderlichen Felder aus und senden Sie es
* Melden Sie sich als „admin“ bei AEM an (falls noch nicht geschehen)
* Navigieren Sie zu [AEM-Posteingang](http://localhost:4502/aem/inbox)
* Öffnen Sie das Formular „Ausfallzeit-Anfrage prüfen“
* Beachten Sie, dass die Felder im ersten Bedienfeld deaktiviert sind. Dies ist der Fall, wenn Formulare von Überprüfenden geöffnet werden. Beachten Sie außerdem, dass das Bedienfeld für die Genehmigung oder Ablehnung der Anfrage jetzt sichtbar ist

>[!NOTE]
>
>Sie können die Debugging-Protokollierung aktivieren, indem Sie die Protokollfunktion für
>com.aemforms.setvalue.core.SetValueinXml
>durch Verweis Ihres Browsers auf http://localhost:4502/system/console/slinglog

>[!NOTE]
>
>Stellen Sie sicher, dass der Datendateipfad in den Sendeoptionen des adaptiven Formulars auf „Data.xml“ festgelegt ist. Dies liegt daran, dass der Prozessschritt nach einer Datei namens „Data.xml“ im Ordner „Payload“ sucht.

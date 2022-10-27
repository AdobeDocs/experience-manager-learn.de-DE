---
title: Verwenden von setValue im AEM Forms-Workflow
description: Wert des Elements in von Adaptive Forms übermittelten Daten in AEM Forms OSGI festlegen
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 3919efee-6998-48e8-85d7-91b6943d23f9
last-substantial-update: 2020-01-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 2%

---

# Verwenden von setValue im AEM Forms-Workflow

Wert eines XML-Elements in von Adaptive Forms gesendeten Daten im AEM Forms OSGI-Workflow festlegen.

![Set Value](assets/setvalue.png)

LiveCycle verfügte früher über eine Komponente mit festgelegtem Wert, mit der Sie den Wert eines XML-Elements festlegen konnten.

Basierend auf diesem Wert können Sie bestimmte Felder oder Bereiche des Formulars ausblenden/deaktivieren, wenn das Formular mit der XML-Datei ausgefüllt ist.

In AEM Forms OSGi müssen wir ein benutzerdefiniertes OSGi-Bundle schreiben, um den Wert in der XML festzulegen. Das Bundle wird im Rahmen dieses Tutorials bereitgestellt.
Wir verwenden Prozessschritt in AEM Workflow. Wir verknüpfen dieses Prozessschritt mit dem OSGi-Bundle &quot;Set Value of Element in XML&quot;.
Wir müssen zwei Argumente an das Set Value Bundle übergeben. Das erste Argument ist der XPath des XML-Elements, dessen Wert festgelegt werden muss. Das zweite Argument ist der festzulegende Wert.
Im obigen Screenshot legen wir beispielsweise den Wert des ersten Schritts auf &quot;N&quot;fest.
Basierend auf diesem Wert werden bestimmte Bedienfelder im Adaptiven Forms ausgeblendet oder angezeigt.
In unserem Beispiel haben wir ein einfaches Antragsformular für die Zeit vor der Abreise. Der Initiator dieses Formulars gibt seinen Namen und die Uhrzeit der Veröffentlichung ein. Bei Übermittlung wird dieses Formular zur Überprüfung an &quot;admin&quot;gesendet. Wenn der Administrator das Formular öffnet, sind die Felder im ersten Bedienfeld deaktiviert. Dies liegt daran, dass wir den Wert des Elements des ersten Schritts in der XML auf &quot;N&quot;festgelegt haben.

Basierend auf dem Wert der ursprünglichen Schrittfelder zeigen wir das zweite Fenster, in dem der &quot;Administrator&quot;die Anfrage genehmigen oder ablehnen kann

Sehen Sie sich die mit dem Regeleditor festgelegten Regeln im Feld &quot;Zeit von angefordert von&quot;an.

Gehen Sie wie folgt vor, um die Assets auf Ihrem lokalen System bereitzustellen:

* [Bereitstellen des Entwicklungs-mit-Service-Benutzer-Bundles](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Bereitstellen des Beispielpakets](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dies ist das benutzerdefinierte OSGi-Bundle, mit dem Sie die Werte eines Elements in den gesendeten XML-Daten festlegen können.

* [Herunterladen und Extrahieren des Inhalts der ZIP-Datei](assets/setvalueassets.zip)
* Zeigen Sie Ihren Browser auf [Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* Importieren und installieren Sie setValueWorkflow.zip. Dies weist das Beispiel-Workflow-Modell auf.
* Zeigen Sie Ihren Browser auf [Forms und Dokumente](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Klicken Sie auf Erstellen | Datei-Upload
* TimeOfRequestForm.zip hochladen
* Öffnen Sie die [TimeOffRequestform](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* Füllen Sie die drei erforderlichen Felder aus und senden Sie
* Melden Sie sich als &quot;Admin&quot;bei AEM an (falls noch nicht geschehen).
* Navigieren Sie zu [&quot;AEM Posteingang&quot;](http://localhost:4502/aem/inbox)
* Öffnen Sie das Formular &quot;Überprüfungszeit von Anforderung&quot;
* Beachten Sie, dass die Felder im ersten Bedienfeld deaktiviert sind. Dies liegt daran, dass das Formular von einem Validierer geöffnet wird. Beachten Sie außerdem, dass der Bereich für die Genehmigung oder Ablehnung der Anfrage jetzt sichtbar ist.

>[!NOTE]
>
>Sie können die Debug-Protokollierung aktivieren, indem Sie die Protokollfunktion für
>com.aemforms.setvalue.core.SetValueinXml
>durch Verweis Ihres Browsers auf http://localhost:4502/system/console/slinglog

>[!NOTE]
>
>Stellen Sie sicher, dass der Datendateipfad in den Sendeoptionen des adaptiven Formulars auf &quot;Data.xml&quot;festgelegt ist. Dies liegt daran, dass der Prozessschritt nach einer Datei namens Data.xml im Ordner &quot;Payload&quot;sucht.

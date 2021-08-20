---
title: Festlegen des JSON-Datenelements im AEM Forms-Workflow
description: Da ein adaptives Formular in AEM Workflow an verschiedene Benutzer weitergeleitet wird, müssen bestimmte Felder oder Bereiche je nach Benutzer, die das Formular überprüfen, ausgeblendet oder deaktiviert werden. Um diese Anwendungsfälle zu erfüllen, legen wir normalerweise den Wert eines ausgeblendeten Felds fest. Basierend auf den Werten dieses ausgeblendeten Felds können Geschäftsregeln erstellt werden, um entsprechende Bereiche oder Felder auszublenden/zu deaktivieren.
feature: Adaptive Formulare
version: 6.4
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '692'
ht-degree: 2%

---


# Festlegen des Werts des JSON-Datenelements im AEM Forms-Workflow {#setting-value-of-json-data-element-in-aem-forms-workflow}

Da ein adaptives Formular in AEM Workflow an verschiedene Benutzer weitergeleitet wird, müssen bestimmte Felder oder Bereiche je nach Benutzer, die das Formular überprüfen, ausgeblendet oder deaktiviert werden. Um diese Anwendungsfälle zu erfüllen, legen wir normalerweise den Wert eines ausgeblendeten Felds fest. Basierend auf den Werten dieses ausgeblendeten Felds können Geschäftsregeln erstellt werden, um entsprechende Bereiche oder Felder auszublenden/zu deaktivieren.

![Festlegen des Werts eines Elements in den JSON-Daten](assets/capture-3.gif)

In AEM Forms OSGi müssen wir ein benutzerdefiniertes OSGi-Bundle schreiben, um den Wert des JSON-Datenelements festzulegen. Das Bundle wird im Rahmen dieses Tutorials bereitgestellt.

Wir verwenden Prozessschritt in AEM Workflow. Wir verknüpfen dieses Prozessschritt mit dem OSGi-Bundle &quot;Set Value of Element in Json&quot;.

Wir müssen zwei Argumente an das Set Value Bundle übergeben. Das erste Argument ist der Pfad zum Element, dessen Wert festgelegt werden muss. Das zweite Argument ist der festzulegende Wert.

Im obigen Screenshot legen wir beispielsweise den Wert des Elements &quot;intialStep&quot;auf &quot;N&quot;fest

afData.afUnboundData.data.initialStep,N

In unserem Beispiel haben wir ein einfaches Antragsformular für die Zeit vor der Abreise. Der Initiator dieses Formulars gibt seinen Namen und die Uhrzeit der Veröffentlichung ein. Bei der Übermittlung wird dieses Formular zur Überprüfung an den &quot;Manager&quot;gesendet. Wenn der Manager das Formular öffnet, sind die Felder im ersten Bedienfeld deaktiviert. Dies liegt daran, dass wir den Wert des Elements des ersten Schritts in den JSON-Daten auf N festgelegt haben.

Basierend auf dem Wert der Felder für den ersten Schritt zeigen wir das Fenster Genehmiger , in dem der &quot;Manager&quot;die Anfrage genehmigen oder ablehnen kann.

Bitte schauen Sie sich die Regeln an, die gegen &quot;Ursprünglicher Schritt&quot;festgelegt wurden. Basierend auf dem Wert des Felds initialStep rufen wir die Benutzerdetails mithilfe des Formulardatenmodells ab, füllen die entsprechenden Felder aus und blenden/deaktivieren die entsprechenden Bereiche ein.

So stellen Sie die Assets auf Ihrem lokalen System bereit:

* [Herunterladen und Bereitstellen von DevelopingWitheServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Laden Sie das SetValue-Bundle herunter und stellen Sie es bereit](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dies ist das benutzerdefinierte OSGi-Bundle, mit dem Sie die Werte eines Elements in den gesendeten JSON-Daten festlegen können.

* [Herunterladen und Extrahieren des Inhalts der ZIP-Datei](assets/set-value-jsondata.zip)
   * Verweisen Sie Ihren Browser auf [Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
      * Importieren und installieren Sie die Datei SetValueOfElementInJSONDataWorkflow.zip. Dieses Paket enthält das Beispiel-Workflow-Modell und das Formulardatenmodell, das mit dem Formular verknüpft ist.

* Verweisen Sie Ihren Browser auf [Forms und Dokumente](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Klicken Sie auf Erstellen | Datei-Upload
* Datei &quot;Upload TimeOffRequestForm.zip&quot;
   **Dieses Formular wurde mit AEM Forms 6.4 erstellt. Bitte stellen Sie sicher, dass Sie AEM Forms 6.4 oder höher verwenden.**
* Öffnen Sie [form](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* Füllen Sie die Start- und Enddaten aus und senden Sie das Formular.
* Gehen Sie zu [&quot;Inbox&quot;](http://localhost:4502/aem/inbox)
* Öffnen Sie das mit der Aufgabe verknüpfte Formular.
* Beachten Sie, dass die Felder im ersten Bedienfeld deaktiviert sind.
* Beachten Sie, dass der Bereich zum Genehmigen oder Ablehnen der Anfrage jetzt angezeigt wird.

>[!NOTE]
>
>Da wir das adaptive Formular mithilfe des Benutzerprofils vorab ausfüllen, stellen Sie sicher, dass die Administrator-[Benutzerprofilinformationen ](http://localhost:4502/security/users.html) vorhanden sind. Stellen Sie mindestens sicher, dass Sie die Feldwerte FirstName, LastName und Email festgelegt haben.
>Sie können die Debug-Protokollierung aktivieren, indem Sie die Protokollfunktion für com.aemforms.setvalue.core.SetValueInJson [von hier](http://localhost:4502/system/console/slinglog) aktivieren.

>[!NOTE]
>
>Das OSGi-Bundle zum Festlegen des Werts von Datenelementen in JSON Data unterstützt derzeit die Möglichkeit, einen Elementwert gleichzeitig festzulegen. Wenn Sie mehrere Elementwerte festlegen möchten, müssen Sie den Prozessschritt mehrmals verwenden.
>
>Stellen Sie sicher, dass der Datendateipfad in den Sendeoptionen des adaptiven Formulars auf &quot;Data.xml&quot;festgelegt ist. Dies liegt daran, dass der Code im Prozessschritt nach einer Datei namens Data.xml unter dem Payload-Ordner sucht.

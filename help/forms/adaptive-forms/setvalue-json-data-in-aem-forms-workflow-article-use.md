---
title: Wert des JSON-Datenelements im AEM Forms-Workflow festlegen
seo-title: Wert des JSON-Datenelements im AEM Forms-Workflow festlegen
description: Da ein adaptives Formular in AEM Arbeitsablauf an verschiedene Benutzer weitergeleitet wird, müssen bestimmte Felder oder Bereiche je nach dem Benutzer, der das Formular überprüft, ein- oder ausgeblendet werden. Um diese Anwendungsfälle zu befriedigen, legen wir in der Regel einen Wert für ein unsichtbares Feld fest. Basierend auf den Wertregeln dieses verborgenen Felds können Geschäftsregeln erstellt werden, um geeignete Bereiche oder Felder auszublenden/zu deaktivieren.
seo-description: Da ein adaptives Formular in AEM Arbeitsablauf an verschiedene Benutzer weitergeleitet wird, müssen bestimmte Felder oder Bereiche je nach dem Benutzer, der das Formular überprüft, ein- oder ausgeblendet werden. Um diese Anwendungsfälle zu befriedigen, legen wir in der Regel einen Wert für ein unsichtbares Feld fest. Basierend auf den Wertregeln dieses verborgenen Felds können Geschäftsregeln erstellt werden, um geeignete Bereiche oder Felder auszublenden/zu deaktivieren.
uuid: a4ea6aef-a799-49e5-9682-3fa3b7a442fb
feature: adaptive Formulare,Workflow
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4
discoiquuid: 548fb2ec-cfcf-4fe2-a02a-14f267618d68
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '764'
ht-degree: 2%

---


# Wert des JSON-Datenelements im AEM Forms Workflow {#setting-value-of-json-data-element-in-aem-forms-workflow} festlegen

Da ein adaptives Formular in AEM Arbeitsablauf an verschiedene Benutzer weitergeleitet wird, müssen bestimmte Felder oder Bereiche je nach dem Benutzer, der das Formular überprüft, ein- oder ausgeblendet werden. Um diese Anwendungsfälle zu befriedigen, legen wir in der Regel einen Wert für ein unsichtbares Feld fest. Basierend auf den Wertregeln dieses verborgenen Felds können Geschäftsregeln erstellt werden, um geeignete Bereiche oder Felder auszublenden/zu deaktivieren.

![Wert eines Elements in JSON-Daten festlegen](assets/capture-3.gif)

In AEM Forms OSGI - müssen wir ein benutzerdefiniertes OSGi-Bundle schreiben, um den Wert des JSON-Datenelements festzulegen. Das Bundle wird im Rahmen dieses Tutorials bereitgestellt.

Wir verwenden Process Step im AEM Workflow. Wir verbinden das OSGi-Bundle &quot;Set Value of Element in JSON&quot; mit diesem Prozessschritt.

Wir müssen dem Set Value Bundle zwei Argumente übergeben. Das erste Argument ist der Pfad zu dem Element, dessen Wert festgelegt werden muss. Das zweite Argument ist der festzulegende Wert.

Im obigen Screenshot setzen wir beispielsweise den Wert des Elements &quot;intialStep&quot;auf &quot;N&quot;

afData.afUnboundData.data.initialStep,N

In unserem Beispiel haben wir ein einfaches Antragsformular &quot;Time Off&quot;. Der Initiator dieses Formulars gibt seinen Namen und die Uhrzeit der Antragstellung ein. Beim Senden wird dieses Formular zur Überprüfung an &quot;Manager&quot;gesendet. Wenn der Manager das Formular öffnet, sind die Felder im ersten Bereich deaktiviert. Dies liegt daran, dass wir den Wert des ersten step-Elements in den JSON-Daten auf N festgelegt haben.

Basierend auf dem Wert des ersten Schrittfeldes wird der Bereich für den Genehmiger angezeigt, in dem der &quot;Manager&quot;die Anforderung genehmigen oder ablehnen kann.

Sehen Sie sich bitte die Regeln an, die gegen &quot;Ursprünglicher Schritt&quot;festgelegt wurden. Basierend auf dem Wert des Felds initialStep rufen wir die Benutzerdetails mithilfe des Formulardatenmodells ab, füllen die entsprechenden Felder aus und blenden/deaktivieren die entsprechenden Bereiche ein.

So stellen Sie die Assets auf Ihrem lokalen System bereit:

* [Herunterladen und Bereitstellen von DevelopingWitheServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Laden Sie das SetValue-Bundle herunter und stellen Sie es bereit](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dies ist das benutzerdefinierte OSGI-Bundle, mit dem Sie die Werte eines Elements in den gesendeten JSON-Daten festlegen können.

* [Herunterladen und Extrahieren des Inhalts der ZIP-Datei](assets/set-value-jsondata.zip)
   * Verweisen Sie Ihren Browser auf [Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
      * Importieren und installieren Sie die Datei SetValueOfElementInJSONDataWorkflow.zip. Für dieses Paket sind das Beispiel-Workflow-Modell und das Formulardatenmodell mit dem Formular verknüpft.

* Verweisen Sie Ihren Browser auf [Forms und Dokumente](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Klicken Sie auf Erstellen | Datei-Upload
* Upload TimeOffRequestForm.zip-Datei
   **Dieses Formular wurde mit AEM Forms 6.4 erstellt. Stellen Sie sicher, dass Sie AEM Forms 6.4 oder höher verwenden**
* Öffnen Sie das [Formular](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* Füllen Sie die Beginns- und Enddaten aus und senden Sie das Formular ab.
* Gehen Sie zu [&quot;Inbox&quot;](http://localhost:4502/aem/inbox)
* Öffnen Sie das mit der Aufgabe verknüpfte Formular.
* Beachten Sie, dass die Felder im ersten Bereich deaktiviert sind.
* Beachten Sie, dass der Bereich zum Genehmigen oder Ablehnen der Anforderung jetzt sichtbar ist.

>[!NOTE]
>
>Da das adaptive Formular mit dem Profil &quot;user&quot;bereits ausgefüllt wurde, stellen Sie sicher, dass die Angaben zum Profil des Administrators [](http://localhost:4502/security/users.html) vorhanden sind. Vergewissern Sie sich mindestens, dass Sie die Feldwerte FirstName, LastName und Email festgelegt haben.
>Sie können die Debug-Protokollierung aktivieren, indem Sie die Protokollfunktion für com.aemforms.setvalue.core.SetValueInJson [von hier](http://localhost:4502/system/console/slinglog) aktivieren

>[!NOTE]
>
>Das OSGi-Bundle zum Festlegen des Werts von Datenelementen in JSON-Daten unterstützt derzeit die Möglichkeit, einen Elementwert gleichzeitig festzulegen. Wenn Sie mehrere Elementwerte festlegen möchten, müssen Sie den Prozessschritt mehrmals verwenden.
>
>Stellen Sie sicher, dass der Pfad der Datendatei in den Übermittlungsoptionen des adaptiven Formulars auf &quot;Data.xml&quot;eingestellt ist. Der Grund dafür ist, dass der Code im Prozessschritt nach einer Datei namens &quot;Data.xml&quot;im Payload-Ordner sucht.

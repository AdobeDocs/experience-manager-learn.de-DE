---
title: Festlegen von Werten für das JSON-Datenelement in AEM Forms Workflow
description: Da ein adaptives Formular im AEM-Workflow an verschiedene Benutzende weitergeleitet wird, müssen bestimmte Felder oder Panels abhängig davon, wer sich das Formular ansieht, ausgeblendet oder deaktiviert werden. Um diesen Anwendungsfällen gerecht zu werden, legen wir normalerweise ein Wert für ein ausgeblendetes Feld fest. Basierend auf dem Wert dieses ausgeblendeten Felds können Geschäftsregeln erstellt werden, um entsprechende Panels oder Felder auszublenden/zu deaktivieren.
feature: Adaptive Forms
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: fbe6d341-7941-46f5-bcd8-58b99396d351
last-substantial-update: 2021-06-09T00:00:00Z
duration: 161
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '656'
ht-degree: 100%

---

# Festlegen von Werten für das JSON-Datenelement in AEM Forms Workflow {#setting-value-of-json-data-element-in-aem-forms-workflow}

Da ein adaptives Formular im AEM-Workflow an verschiedene Benutzende weitergeleitet wird, müssen bestimmte Felder oder Panels abhängig davon, wer sich das Formular ansieht, ausgeblendet oder deaktiviert werden. Um diesen Anwendungsfällen gerecht zu werden, legen wir normalerweise ein Wert für ein ausgeblendetes Feld fest. Basierend auf dem Wert dieses ausgeblendeten Felds können Geschäftsregeln erstellt werden, um entsprechende Panels oder Felder auszublenden/zu deaktivieren.

![Festlegen eines Elementwerts in JSON-Daten](assets/capture-3.gif)

In AEM Forms OSGi müssen wir ein benutzerdefiniertes OSGi-Bundle erstellen, um den Wert des JSON-Datenelements festzulegen. Dieses Bundle wird im Rahmen dieses Tutorials bereitgestellt.

Wir verwenden den Prozessschritt in AEM-Workflow. Wir verknüpfen das OSGi-Bundle „Set Value of Element in Json“ mit diesem Prozessschritt.

Wir müssen zwei Argumente an das Bundle zum Festlegen der Werte übergeben. Das erste Argument ist der Pfad zum Element, dessen Wert festgelegt werden muss. Das zweite Argument ist der Wert, der festgelegt werden muss.

Im obigen Screenshot ist beispielsweise für das initialStep-Element der Wert „N“ festgelegt.

afData.afUnboundData.data.initialStep,N

In unserem Beispiel haben wir einen einfachen Urlaubsantrag vorliegen. Die Person, die dieses Formular aufgerufen hat, gibt ihren Namen und die Urlaubstage ein. Bei der Übermittlung wird dieses Formular zur Überprüfung an die Vorgesetzte oder den Vorgesetzten gesendet. Wenn die oder der Vorgesetzte das Formular öffnet, sind die Felder im ersten Bedienfeld deaktiviert. Dies liegt daran, dass wir für das initialStep-Element in den JSON-Daten den Wert „N“ festgelegt haben.

Basierend auf dem Wert des initialStep-Feldes lassen wir das Bedienfeld für genehmigende Personen anzeigen, über das die oder der Vorgesetzte den Antrag genehmigen oder ablehnen kann.

Sehen Sie sich bitte die Regeln an, die für „InitialSetup“ festgelegt wurden. Basierend auf dem Wert des initialStep-Feldes lassen wir die Benutzerdetails mithilfe des Formulardatenmodells abrufen, die entsprechenden Felder auffüllen und die entsprechenden Panels ausblenden/deaktivieren.

So stellen Sie die Assets auf Ihrem lokalen System bereit:

* [Laden Sie „DevelopingWitheServiceUserBundle“ herunter und stellen Sie dieses Bundle bereit.](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Laden Sie das setvalue-Bundle herunter und stellen Sie es bereit](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Hierbei handelt es sich um das benutzerdefinierte OSGi-Bundle, mit dem Sie die Werte eines Elements in den übermittelten JSON-Daten festlegen können.

* [Laden Sie die ZIP-Datei herunter und extrahieren Sie deren Inhalt.](assets/set-value-jsondata.zip)
   * Verweisen Sie Ihren Browser auf [Package Manager](http://localhost:4502/crx/packmgr/index.jsp).
      * Importieren und installieren Sie die Datei „SetValueOfElementInJSONDataWorkflow.zip“. Dieses Paket enthält das Workflow-Beispielmodell und das mit dem Formular verknüpfte Formulardatenmodell.

* Verweisen Sie Ihren Browser auf [Formulare und Dokumente](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Klicken Sie auf „Erstellen“ > „Datei hochladen“.
* Laden Sie die Datei „TimeOffRequestForm.zip“ hoch.
  **Dieses Formular wurde mit AEM Forms 6.4 erstellt. Bitte stellen Sie sicher, dass Sie AEM Forms 6.4 oder höher verwenden.**
* Öffnen Sie das [Formular](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled).
* Tragen Sie das Start- und Enddatum ein und übermitteln Sie das Formular.
* Navigieren Sie zum [Posteingang](http://localhost:4502/aem/inbox).
* Öffnen Sie das mit der Aufgabe verknüpfte Formular.
* Die Felder im ersten Panel sind deaktiviert.
* Das Panel zum Genehmigen oder Ablehnen des Antrags wird jetzt angezeigt.

>[!NOTE]
>
>Da wir das adaptive Formular anhand des Benutzerprofils vorab ausfüllen, stellen Sie sicher, dass die Admin-[Benutzerprofilinformationen](http://localhost:4502/security/users.html) korrekt sind. Sie müssen zumindest sicherstellen, dass die Feldwerte für „FirstName“, „LastName“ und „Email“ festgelegt sind.
>Sie können die Debugging-Protokollierung aktivieren, indem Sie den Logger für „com.aemforms.setvalue.core.SetValueInJson“ [hier](http://localhost:4502/system/console/slinglog) aktivieren.

>[!NOTE]
>
>Das OSGi-Bundle zum Festlegen des Werts von Datenelementen in JSON-Daten ermöglicht es, jeweils einen Elementwert festzulegen. Wenn Sie mehrere Elementwerte festlegen möchten, müssen Sie den Prozessschritt mehrmals anwenden.
>
>Stellen Sie sicher, dass der Datendateipfad in den Sendeoptionen des adaptiven Formulars auf „Data.xml“ festgelegt ist. Der Code im Prozessschritt sucht nämlich nach der Datei „Data.xml“ im Payload-Ordner.

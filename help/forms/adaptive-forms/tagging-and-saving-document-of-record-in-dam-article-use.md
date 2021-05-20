---
title: Tagging und Speichern von AEM Forms DoR in DAM
seo-title: Tagging und Speichern von AEM Forms DoR in DAM
description: In diesem Artikel wird das Anwendungsbeispiel zum Speichern und Taggen des von AEM Forms in AEM DAM generierten DoR erläutert. Das Tagging des Dokuments erfolgt basierend auf den gesendeten Formulardaten.
seo-description: In diesem Artikel wird das Anwendungsbeispiel zum Speichern und Taggen des von AEM Forms in AEM DAM generierten DoR erläutert. Das Tagging des Dokuments erfolgt basierend auf den gesendeten Formulardaten.
uuid: b9ba13ed-52d5-4389-a7d5-bf85e58fea49
feature: Adaptive Forms,Workflow
topics: developing
audience: implementer
doc-type: article
activity: develop
version: 6.4,6.5
discoiquuid: 53961454-633b-4cd8-aef7-e64ab4e528e4
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '659'
ht-degree: 1%

---


# Tagging und Speichern von AEM Forms DoR in DAM {#tagging-and-storing-aem-forms-dor-in-dam}

In diesem Artikel wird das Anwendungsbeispiel zum Speichern und Taggen des von AEM Forms in AEM DAM generierten DoR erläutert. Das Tagging des Dokuments erfolgt basierend auf den gesendeten Formulardaten.

Eine häufige Kundenanfrage besteht darin, das von AEM Forms generierte Datensatzdokument (DoR) in AEM DAM zu speichern und zu taggen. Das Tagging des Dokuments muss auf den von Adaptive Forms übermittelten Daten basieren. Wenn der Beschäftigungsstatus in den gesendeten Daten beispielsweise &quot;Rentner&quot;lautet, möchten wir das Dokument mit dem Tag &quot;Rentner&quot;versehen und das Dokument in DAM speichern.

Der Anwendungsfall sieht folgendermaßen aus:

* Ein Benutzer füllt das adaptive Formular aus. Im adaptiven Formular werden der Familienstand (ex Single) und der Beschäftigungsstatus (Ex Remüde) des Benutzers erfasst.
* Beim Senden des Formulars wird ein AEM Workflow ausgelöst. Dieser Workflow markiert das Dokument mit dem Familienstand (Single) und dem Beschäftigungsstatus (Rentner) und speichert das Dokument in DAM.
* Sobald das Dokument in DAM gespeichert ist, sollte der Administrator das Dokument anhand dieser Tags durchsuchen können. Beispielsweise würde die Suche nach Single oder Rentner die entsprechenden DoR abrufen.

Um diesen Anwendungsfall zu erfüllen, wurde ein benutzerdefinierter Prozessschritt geschrieben. In diesem Schritt rufen wir die Werte der entsprechenden Datenelemente aus den gesendeten Daten ab. Anschließend erstellen wir die Tag-Kachel mit diesem Wert. Wenn der Wert des Ehestatus-Elements beispielsweise &quot;Single&quot;lautet, wird der Tag-Titel zu &quot;Peak:EmploymentStatus/Single&quot;. **Mithilfe der TagManager-API finden wir das Tag und wenden es auf das DoR an.

Das folgende Codefragment zeigt, wie Sie das Tag finden und auf das Dokument anwenden können.

```java
Tag tagFound = tagManager.resolveByTitle(tagTitle+xmlElement.getTextContent());
//tagTitle is "Peak:EmploymentStatus/" and the xmlElement.getTextContent() will return the value Single. So the tag title becomes Peak:EmploymentStatus/Single. Once the tag is found we put the tag in array and apply the tags to the resource as shown below
tagArray[i] = tagFound;
tagManager.setTags(metadata, tagArray, true);
```

Um dieses Beispiel auf Ihrem System verwenden zu können, führen Sie die folgenden Schritte aus:
* [Bereitstellen des Entwicklungs-mit-Service-Benutzer-Bundles](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Laden Sie das SetValue-Bundle herunter und stellen Sie es bereit](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dies ist das benutzerdefinierte OSGI-Bundle, das die Tags aus den gesendeten Formulardaten festlegt.

* [Herunterladen des adaptiven Beispielformulars](assets/tag-and-store-in-dam-assets.zip)

* [Navigieren Sie zu Forms und Dokumenten .](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Klicken Sie auf Erstellen | Datei-Upload und Hochladen der sampleadaptiveform.zip-Datei

* [Importieren Sie die Artikel-](assets/tag-and-store-in-dam-assets.zip) Assets mit AEM Package Manager.
* Öffnen Sie das Beispielformular [im Vorschaumodus](http://localhost:4502/content/dam/formsanddocuments/summit/peakform/jcr:content?wcmmode=disabled). Füllen Sie den Abschnitt Personen aus und senden Sie das Formular.
* [Navigieren Sie zum Ordner &quot;Peak&quot;in DAM](http://localhost:4502/assets.html/content/dam/Peak). Sie sollten DoR im Ordner Peak sehen. Überprüfen Sie die Eigenschaften des Dokuments. Sie sollte entsprechend mit Tags versehen werden.
Herzlichen Glückwunsch!! Sie haben das Muster erfolgreich auf Ihrem System installiert.

* Sehen wir uns den [Workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html) an, der beim Senden des Formulars ausgelöst wird.
* Der erste Schritt im Workflow erstellt einen eindeutigen Dateinamen, indem der Name des Antragstellers und das Land des Wohnsitzes verkettet werden.
* Der zweite Schritt des Workflows besteht aus der Tag-Hierarchie und den Formularfeldelementen, die mit Tags versehen werden müssen. Der Prozessschritt extrahiert den Wert aus den gesendeten Daten und erstellt den Tag-Titel, der das Dokument taggen muss.
* Wenn Sie DoR in einem anderen Ordner im DAM speichern möchten, geben Sie den Ordnerspeicherort mithilfe der Konfigurationseigenschaften an, wie im Screenshot unten angegeben.

Die anderen beiden Parameter sind spezifisch für DoR und Datendateipfad, wie in den Übermittlungsoptionen für adaptive Formulare angegeben. Stellen Sie sicher, dass die hier angegebenen Werte mit den Werten übereinstimmen, die Sie in den Übermittlungsoptionen für adaptive Formulare angegeben haben.

![Tag Dor](assets/tag_dor_service_configuration.gif)


---
title: Tagging und Speichern des AEM Forms DoR in DAM
seo-title: Tagging und Speichern des AEM Forms DoR in DAM
description: In diesem Artikel wird der Verwendungsfall zum Speichern und Taggen des DoR, das von AEM Forms in AEM DAM generiert wurde, erläutert. Das Tagging des Dokuments erfolgt auf der Grundlage der gesendeten Formulardaten.
seo-description: In diesem Artikel wird der Verwendungsfall zum Speichern und Taggen des DoR, das von AEM Forms in AEM DAM generiert wurde, erläutert. Das Tagging des Dokuments erfolgt auf der Grundlage der gesendeten Formulardaten.
uuid: b9ba13ed-52d5-4389-a7d5-bf85e58fea49
feature: Adaptives Forms, Workflow
topics: developing
audience: implementer
doc-type: article
activity: develop
version: 6.4,6.5
discoiquuid: 53961454-633b-4cd8-aef7-e64ab4e528e4
topic: Entwicklung
role: Entwickler
level: Erfahren
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '661'
ht-degree: 1%

---


# Tagging und Speichern von AEM Forms DoR in DAM {#tagging-and-storing-aem-forms-dor-in-dam}

In diesem Artikel wird der Verwendungsfall zum Speichern und Taggen des DoR, das von AEM Forms in AEM DAM generiert wurde, erläutert. Das Tagging des Dokuments erfolgt auf der Grundlage der gesendeten Formulardaten.

Eine häufige Kundenanforderung besteht darin, das Dokument von Record(DoR) zu speichern und zu kennzeichnen, das von AEM Forms in AEM DAM generiert wurde. Das Tagging des Dokuments muss auf den vom adaptiven Forms übermittelten Daten basieren. Wenn beispielsweise der Beschäftigungsstatus in den gesendeten Daten &quot;Rentner&quot;ist, möchten wir das Dokument mit dem Tag &quot;Rentner&quot;markieren und das Dokument in DAM speichern.

Der Verwendungsfall lautet wie folgt:

* Ein Benutzer füllt das adaptive Formular aus. Im adaptiven Formular werden der Familienstand (ex Single) und der Beschäftigungsstatus (Ex Remüde) des Benutzers erfasst.
* Beim Senden des Formulars wird ein AEM Workflow ausgelöst. Dieser Arbeitsablauf markiert das Dokument mit dem Familienstand (Single) und dem Beschäftigungsstatus (Remüde) und speichert das Dokument in DAM.
* Sobald das Dokument in DAM gespeichert ist, sollte der Administrator das Dokument anhand dieser Tags durchsuchen können. Beispielsweise würde die Suche nach Einzel oder Rentner die entsprechenden DoRs abrufen.

Um diesen Verwendungsfall zu befriedigen, wurde ein benutzerdefinierter Prozessschritt geschrieben. In diesem Schritt holen wir die Werte der entsprechenden Datenelemente aus den gesendeten Daten ab. Anschließend erstellen wir die Tag-Kachel mit diesem Wert. Wenn der Wert des Elements &quot;Familienstand&quot;beispielsweise &quot;Single&quot;lautet, wird der Tag-Titel **Peak:EmploymentStatus/Single. **Mithilfe der TagManager-API finden Sie das Tag und wenden das Tag auf das DoR an.

Das folgende Codefragment zeigt Ihnen, wie Sie das Tag finden und das Tag auf das Dokument anwenden.

```java
Tag tagFound = tagManager.resolveByTitle(tagTitle+xmlElement.getTextContent());
//tagTitle is "Peak:EmploymentStatus/" and the xmlElement.getTextContent() will return the value Single. So the tag title becomes Peak:EmploymentStatus/Single. Once the tag is found we put the tag in array and apply the tags to the resource as shown below
tagArray[i] = tagFound;
tagManager.setTags(metadata, tagArray, true);
```

Gehen Sie wie folgt vor, um dieses Beispiel auf Ihrem System zu verwenden:
* [Developing with serviceUser-Bundle bereitstellen](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Laden Sie das SetValue-Bundle herunter und stellen Sie es bereit](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dies ist das benutzerdefinierte OSGI-Bundle, das die Tags aus den gesendeten Formulardaten festlegt.

* [Beispiel für ein adaptives Formular herunterladen](assets/tag-and-store-in-dam-assets.zip)

* [Gehen Sie zu Forms und Dokumente](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Klicken Sie auf Erstellen | Datei-Upload und Hochladen der Datei &quot;sampleadaptiveform.zip&quot;

* [Artikelelemente ](assets/tag-and-store-in-dam-assets.zip) mit AEM Paketmanager importieren
* Öffnen Sie das Musterformular [im Vorschau-Modus](http://localhost:4502/content/dam/formsanddocuments/summit/peakform/jcr:content?wcmmode=disabled). Füllen Sie den Abschnitt Personen aus und senden Sie das Formular.
* [Navigieren Sie zum Ordner Peak in DAM](http://localhost:4502/assets.html/content/dam/Peak). Sie sollten DoR im Ordner Peak sehen. Überprüfen Sie die Eigenschaften des Dokuments. Es sollte angemessen getaggt werden.
Herzlichen Glückwunsch!! Sie haben das Muster erfolgreich auf Ihrem System installiert.

* Sehen wir uns den [Workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html) an, der beim Senden des Formulars ausgelöst wird.
* Der erste Schritt im Arbeitsablauf erstellt einen eindeutigen Dateinamen, indem der Name des Antragstellers mit dem Land des Wohnsitzes verknüpft wird.
* Der zweite Schritt des Workflows besteht aus der Tag-Hierarchie und den Formularfeldelementen, die mit Tags versehen werden müssen. Der Prozessschritt extrahiert den Wert aus den gesendeten Daten und erstellt den Tag-Titel, der das Dokument taggen muss.
* Wenn Sie DoR in einem anderen Ordner im DAM speichern möchten, geben Sie den Speicherort des Ordners mithilfe der Konfigurationseigenschaften an, wie im Screenshot unten angegeben.

Die anderen beiden Parameter sind spezifisch für DoR und Datendateipfad, wie in den Übermittlungsoptionen für adaptive Formulare angegeben. Vergewissern Sie sich, dass die hier angegebenen Werte mit den Werten übereinstimmen, die Sie in den Optionen zur Übermittlung des adaptiven Formulars angegeben haben.

![Tag Dor](assets/tag_dor_service_configuration.gif)


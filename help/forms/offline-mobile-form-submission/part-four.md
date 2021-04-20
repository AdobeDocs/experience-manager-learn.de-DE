---
title: Arbeitsablauf für Trigger AEM HTML5-Formularübermittlung
seo-title: Trigger AEM Workflow bei der Übermittlung von HTML5-Formularen
description: Fahren Sie mit dem Ausfüllen des mobilen Formulars im Offlinemodus fort und senden Sie das Mobile-Formular an den Trigger AEM Arbeitsablauf
seo-description: Fahren Sie mit dem Ausfüllen des mobilen Formulars im Offlinemodus fort und senden Sie das Mobile-Formular an den Trigger AEM Arbeitsablauf
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 1%

---


# Verwendungsfall für Ihr System einrichten

>[!NOTE]
>
>Damit die Beispiel-Assets auf Ihrem System funktionieren, wird davon ausgegangen, dass die AEM Author- und die Veröffentlichungsinstanz auf Port 4502 bzw. 4503 ausgeführt werden. Es wird auch angenommen, dass der AEM-Autor über `admin`/`admin` zugänglich ist. Wenn die Anschlussnummern oder das Administratorkennwort geändert wurden, funktionieren diese Beispiel-Assets nicht. Sie müssen Ihre eigenen Assets mit dem bereitgestellten Beispielcode erstellen.

Gehen Sie wie folgt vor, um diesen Verwendungsfall auf Ihrem lokalen System zu verwenden:

* Installieren der AEM Author-Instanz auf Port 4502 und der AEM Publish-Instanz auf Port 4503
* [Befolgen Sie die Anweisungen, die unter Entwickeln mit Dienstbenutzern in AEM Forms](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html) beschrieben werden. Erstellen Sie den Dienstbenutzer und stellen Sie das Bundle in Ihrer AEM Author- und Veröffentlichungsinstanz bereit.
* [Öffnen Sie die OSGI-Konfiguration  ](http://localhost:4503/system/console/configMgr).
* Suchen Sie nach **Apache Sling Werber Filter**. Vergewissern Sie sich, dass das Kontrollkästchen &quot;Leeres Feld zulassen&quot;aktiviert ist.
* [Stellen Sie das benutzerdefinierte AEMFormDocumentService-Bundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar) bereit. Dieses Bundle muss auf Ihrer AEM Publish-Instanz bereitgestellt werden. Dieses Bundle enthält den Code zum Generieren interaktiver PDF-Dateien aus einem mobilen Formular.
* [Laden Sie die Elemente für diesen Artikel herunter und entpacken Sie sie.](assets/offline-pdf-submission-assets.zip) Sie erhalten Folgendes:
   * **offline-submission-Profil.zip**  - Dieses AEM Paket enthält das benutzerdefinierte Profil, mit dem Sie die interaktive PDF-Datei in Ihr lokales Dateisystem herunterladen können. Stellen Sie dieses Paket auf Ihrer AEM Publish-Instanz bereit.
   * **xdp-form-and-workflow.zip**  - Dieses AEM Paket enthält XDP, Beispiel-Workflow, Starter, der auf Knoten content/pdfsubmissions konfiguriert wurde. Stellen Sie dieses Paket auf Ihrer AEM-Instanz im Autorenmodus und in der Veröffentlichungsinstanz bereit.
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar**  - Dies ist das AEM Bundle, das die meiste Arbeit leistet. Dieses Bundle enthält das auf `/bin/startworkflow` gemountete Servlet. Dieses Servlet speichert die gesendeten Formulardaten unter dem Knoten `/content/pdfsubmissions` im AEM Repository. Stellen Sie dieses Bundle auf Ihrer AEM-Instanz im Autorenmodus und in der Veröffentlichungsinstanz bereit.
* [Vorschau des mobilen Formulars](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* Füllen Sie mehrere Felder aus und klicken Sie dann auf die Schaltfläche in der Symbolleiste, um die interaktive PDF herunterzuladen.
* Füllen Sie die heruntergeladene PDF mit Acrobat aus und klicken Sie auf Senden.
* Sie sollten eine Erfolgsmeldung erhalten
* Anmelden bei der AEM-Autoreninstanz als Administrator
* [Markieren Sie den AEM Posteingang.](http://localhost:4502/aem/inbox)
* Sie sollten über ein Arbeitselement verfügen, um die gesendete PDF-Datei zu überprüfen.

>[!NOTE]
>
>Anstatt das PDF-Dokument an das Servlet zu übermitteln, das in der Veröffentlichungsinstanz ausgeführt wird, haben einige Kunden das Servlet in Servlet-Container wie Tomcat bereitgestellt. Es hängt alles von der Topologie ab, mit der der Kunde vertraut ist. Für die Zwecke dieses Tutorials werden wir das in der Veröffentlichungsinstanz bereitgestellte Servlet für die Bearbeitung der PDF-Übermittlungen verwenden.


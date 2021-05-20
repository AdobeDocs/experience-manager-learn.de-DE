---
title: Trigger AEM Workflow bei der Übermittlung von HTML5-Formularen
seo-title: Trigger AEM Workflow bei der Übermittlung von HTML5-Formularen
description: Fahren Sie mit dem Ausfüllen des mobilen Formulars im Offline-Modus fort und senden Sie das mobile Formular an den Trigger AEM Workflow
seo-description: Fahren Sie mit dem Ausfüllen des mobilen Formulars im Offline-Modus fort und senden Sie das mobile Formular an den Trigger AEM Workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 1%

---


# Dieses Nutzungsszenario für das System verwenden

>[!NOTE]
>
>Damit die Beispiel-Assets auf Ihrem System funktionieren, wird davon ausgegangen, dass die AEM-Autoren- und Veröffentlichungsinstanz auf Port 4502 bzw. Port 4503 ausgeführt wird. Es wird außerdem davon ausgegangen, dass der Zugriff auf die AEM-Autoreninstanz über `admin`/`admin` möglich ist. Wenn die Portnummern oder das Admin-Kennwort geändert wurden, funktionieren diese Beispiel-Assets nicht. Sie müssen Ihre eigenen Assets mit dem bereitgestellten Beispielcode erstellen.

Gehen Sie wie folgt vor, um dieses Anwendungsbeispiel auf Ihrem lokalen System zu verwenden:

* Installieren Sie die AEM-Autoreninstanz auf Port 4502 und die AEM-Veröffentlichungsinstanz auf Port 4503
* [Befolgen Sie die Anweisungen, die bei der Entwicklung mit Dienstbenutzern in AEM Forms](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html) angegeben wurden. Stellen Sie sicher, dass Sie den Dienstbenutzer erstellen und das Bundle in Ihrer AEM-Autoren- und Veröffentlichungsinstanz bereitstellen.
* [Öffnen Sie die OSGi-Konfiguration  ](http://localhost:4503/system/console/configMgr).
* Suchen Sie nach **Apache Sling Referrer Filter**. Stellen Sie sicher, dass das Kontrollkästchen Leere erlauben aktiviert ist.
* [Stellen Sie das benutzerdefinierte AEMFormDocumentService-Bundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar) bereit. Dieses Bundle muss auf Ihrer AEM-Veröffentlichungsinstanz bereitgestellt werden. Dieses Bundle enthält den Code zum Generieren interaktiver PDF-Dateien aus einem mobilen Formular.
* [Laden Sie die mit diesem Artikel verknüpften Assets herunter und entpacken Sie sie.](assets/offline-pdf-submission-assets.zip) Sie erhalten Folgendes
   * **offline-submission-profile.zip**  - Dieses AEM-Paket enthält das benutzerdefinierte Profil, mit dem Sie das interaktive PDF-Dokument in Ihr lokales Dateisystem herunterladen können. Stellen Sie dieses Paket auf Ihrer AEM-Veröffentlichungsinstanz bereit.
   * **xdp-form-and-workflow.zip**  - Dieses AEM-Paket enthält XDP, Beispiel-Workflow, Starter, konfiguriert für Knoteninhalt/pdfsubmissions. Stellen Sie dieses Paket auf Ihrer AEM-Autoren- und Veröffentlichungsinstanz bereit.
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar**  - Dies ist das AEM Bundle, das den Großteil der Arbeit ausführt. Dieses Bundle enthält das auf `/bin/startworkflow` bereitgestellte Servlet. Dieses Servlet speichert die gesendeten Formulardaten unter dem Knoten `/content/pdfsubmissions` im AEM Repository. Stellen Sie dieses Bundle sowohl auf Ihrer AEM-Autoren- als auch auf Ihrer Veröffentlichungsinstanz bereit.
* [Vorschau des mobilen Formulars](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* Füllen Sie mehrere Felder aus und klicken Sie dann auf die Schaltfläche in der Symbolleiste, um die interaktive PDF-Datei herunterzuladen.
* Füllen Sie die heruntergeladene PDF-Datei mit Acrobat aus und drücken Sie die Senden-Schaltfläche.
* Sie sollten eine Erfolgsmeldung erhalten
* Melden Sie sich bei der AEM-Autoreninstanz als Administrator an
* [Aktivieren Sie den AEM Posteingang .](http://localhost:4502/aem/inbox)
* Sie sollten über ein Arbeitselement verfügen, um die gesendete PDF-Datei zu überprüfen.

>[!NOTE]
>
>Anstatt die PDF-Datei an das Servlet zu senden, das auf der Veröffentlichungsinstanz ausgeführt wird, haben einige Kunden das Servlet im Servlet-Container wie Tomcat bereitgestellt. Das alles hängt von der Topologie ab, mit der der Kunde vertraut ist. Für dieses Tutorial verwenden wir das Servlet, das in der Veröffentlichungsinstanz bereitgestellt wird, zum Verarbeiten der PDF-Übermittlungen.


---
title: Auslösen eines AEM-Workflows für die Übermittlung von HTML5-Formularen – Umsetzen des Anwendungsfalls
description: Fahren Sie mit dem Ausfüllen des Mobile-Formulars im Offline-Modus fort und übermitteln Sie das Mobile-Formular, um den AEM-Workflow auszulösen.
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 79935ef0-bc73-4625-97dd-767d47a8b8bb
duration: 111
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 100%

---

# Umsetzen dieses Anwendungsfalls auf Ihrem System

>[!NOTE]
>
>Damit die Beispiel-Assets auf Ihrem System verwendet werden können, wird davon ausgegangen, dass die AEM-Autoren- und -Veröffentlichungsinstanz auf Port 4502 bzw. Port 4503 ausgeführt wird. Es wird außerdem angenommen, dass über `admin`/`admin` auf die AEM-Autoreninstanz zugegriffen werden kann. Wenn die Port-Nummern oder das Admin-Kennwort geändert wurden, können diese Beispiel-Assets nicht verwendet werden. Sie müssen dann Ihre eigenen Assets anhand des bereitgestellten Beispiel-Codes erstellen.

Gehen Sie wie folgt vor, um diesen Anwendungsfall auf Ihrem lokalen System umzusetzen:

* Installieren Sie die AEM-Autoreninstanz auf Port 4502 und die AEM-Veröffentlichungsinstanz auf Port 4503.
* Befolgen Sie die Anweisungen unter [Entwickeln mit Dienstbenutzenden in AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=de). Stellen Sie sicher, dass Sie eine Dienstbenutzerin oder einen Dienstbenutzer erstellen und das Bundle in Ihrer AEM-Autoren- und -Veröffentlichungsinstanz bereitstellen.
* [Öffnen Sie die OSGi-Konfiguration.](http://localhost:4503/system/console/configMgr)
* Suchen Sie nach **Apache Sling Referrer Filter**. Stellen Sie sicher, dass das Kontrollkästchen „Leere zulassen“ aktiviert ist.
* Stellen Sie das [benutzerdefinierte Bundle „AEMFormDocumentService“](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar) in Ihrer AEM-Veröffentlichungsinstanz bereit. Dieses Bundle enthält den Code zum Generieren interaktiver PDF-Dateien aus einem Mobile-Formular.
* [Laden Sie die mit diesem Artikel verbundenen Assets herunter und entpacken Sie sie.](assets/offline-pdf-submission-assets.zip) Sie erhalten Folgendes:
   * **offline-submission-profile.zip**: Dieses AEM-Paket enthält das benutzerdefinierte Profil, mit dem Sie das interaktive PDF-Dokument in Ihr lokales Dateisystem herunterladen können. Stellen Sie dieses Paket in Ihrer AEM-Veröffentlichungsinstanz bereit.
   * **xdp-form-and-workflow.zip**: Dieses AEM-Paket enthält die XDP, einen Beispiel-Workflow und den für den Knoten „content/pdfsubmissions“ konfigurierten Starter. Stellen Sie dieses Paket in Ihrer AEM-Autoren- und -Veröffentlichungsinstanz bereit.
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar**: Dieses AEM-Bundle erledigt den Großteil der Arbeit und enthält das unter `/bin/startworkflow` bereitgestellte Servlet. Dieses Servlet speichert die übermittelten Formulardaten unter dem Knoten `/content/pdfsubmissions` im AEM-Repository. Stellen Sie dieses Bundle in Ihrer AEM-Autoren- und -Veröffentlichungsinstanz bereit.
* [Zeigen Sie das Mobile-Formular in einer Vorschau an.](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* Füllen Sie mehrere Felder aus und klicken Sie dann auf die Schaltfläche in der Symbolleiste, um die interaktive PDF-Datei herunterzuladen.
* Füllen Sie die heruntergeladene PDF-Datei mit Acrobat aus und klicken Sie auf die Schaltfläche „Senden“.
* Sie sollten eine Erfolgsmeldung erhalten.
* Melden Sie sich bei der AEM-Autoreninstanz als Admin an.
* [Überprüfen Sie den AEM-Posteingang.](http://localhost:4502/aem/inbox)
* Sie sollten über ein Arbeitselement verfügen, um die übermittelte PDF-Datei zu überprüfen.

>[!NOTE]
>
>Anstatt die PDF-Datei an das in der Veröffentlichungsinstanz ausgeführte Servlet zu übermitteln, haben einige Kundinnen und Kunden das Servlet im Servlet-Container, z. B. Tomcat, bereitgestellt. Das alles hängt von der Topologie ab, mit der die Kundin oder der Kunde vertraut ist. Für dieses Tutorial verwenden wir das in der Veröffentlichungsinstanz bereitgestellte Servlet, um die PDF-Übermittlungen zu verarbeiten.

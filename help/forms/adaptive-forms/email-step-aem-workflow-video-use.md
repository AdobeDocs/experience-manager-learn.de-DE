---
title: Verwenden des Forms Workflow-Schritts „E-Mail senden“
description: Der Schritt „E-Mail senden“ wurde in AEM Forms 6.4 eingeführt. Mithilfe dieses Schritts können wir Geschäftsprozesse oder Workflows erstellen, mit denen Sie E-Mails mit oder ohne Anhänge senden können. Im folgenden Video werden die Schritte zum Konfigurieren der Komponente „E-Mail senden“ erläutert.
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 21e58bbc-c1d6-4d41-a4d4-f522a3a5d4a7
last-substantial-update: 2020-06-09T00:00:00Z
duration: 326
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '418'
ht-degree: 100%

---

# Verwenden des Forms Workflows-Schritts „E-Mail senden“ {#using-send-email-step-of-forms-workflow}

Der Schritt „E-Mail senden“ wurde in AEM Forms 6.4 eingeführt. Mithilfe dieses Schritts können wir Geschäftsprozesse oder Workflows erstellen, mit denen Sie E-Mails mit oder ohne Anhänge senden können. Im folgenden Video werden die Schritte zum Konfigurieren der Komponente „E-Mail senden“ erläutert.

>[!VIDEO](https://video.tv.adobe.com/v/21499?quality=12&learn=on)

Im Rahmen dieses Artikels führen wir Sie durch folgenden Anwendungsfall:

1. Eine Benutzerin oder ein Benutzer füllt einen Urlaubsantrag aus.
1. Beim Übermitteln des Formulars wird der AEM-Workflow ausgelöst.
1. Der AEM-Workflow verwendet die Komponente „E-Mail senden“, um eine E-Mail mit dem Datensatzdokument (Document of Record, DoR) im Anhang zu senden.

Bevor Sie den Schritt „E-Mail senden“ ausführen, konfigurieren Sie den Day CQ Mail Service über [configMgr](http://localhost:4502/system/console/configMgr). Geben Sie die für Ihre Umgebung zutreffenden Werte an.

![Konfigurieren des Day CQ Mail Service](assets/mailservice.png)

Im Rahmen der mit diesem Artikel verknüpften Assets erhalten Sie Folgendes:

1. adaptives Formular, das den Workflow bei der Übermittlung auslöst
1. Beispiel-Workflow, der eine E-Mail mit dem DoR im Anhang sendet
1. OSGi-Bundle, das die Metadateneigenschaften erstellt

Führen Sie die folgenden Schritte aus, um das Beispiel auf Ihrem System auszuführen:

1. [Stellen Sie das Bundle „DevelopingWithServiceUser“ bereit.](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Laden Sie das SetValue-Bundle herunter und installieren Sie es](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dieses Bundle enthält den Code zum Erstellen der Metadateneigenschaften im Rahmen des Prozessschritts des Workflows.
1. [Konfigurieren des Day CQ Mail Service](https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/notification.html)
1. [Importieren und installieren Sie die mit diesem Artikel verbundenen Assets mit Package Manager in CRX.](assets/emaildoraemformskt.zip)
1. Starten Sie das [adaptive Formular](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled). Füllen Sie die erforderlichen Felder aus und übermitteln Sie das Formular.
1. Sie sollten eine E-Mail mit dem Datensatzdokument im Anhang erhalten.

Erkunden Sie das [Workflow-Modell](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html).

Sehen Sie sich den Prozessschritt des Workflows an. Der mit dem Prozessschritt verknüpfte benutzerdefinierte Code erstellt Namen der Metadateneigenschaften und legt deren Werte anhand der übermittelten Daten fest. Diese Werte werden dann von der Komponente „E-Mail senden“ verwendet.

>[!NOTE]
>
>Ab AEM Forms 6.5 benötigen Sie diesen benutzerdefinierten Code nicht, um Metadateneigenschaften zu erstellen. Verwenden Sie stattdessen die Variablenfunktion im AEM-Workflow.

Stellen Sie sicher, dass die Registerkarte „Anhänge“ der Komponente „E-Mail senden“ gemäß dem folgenden Screenshot konfiguriert ist.
![Registerkarte „Anhänge“ der Komponente „E-Mail senden“](assets/sendemailcomponentconfigure.jpg)Der Wert „DOR.pdf“ muss mit dem Wert übereinstimmen, der in dem in den Sendeoptionen Ihres adaptiven Formulars festgelegten Datensatzdokument-Pfad angegeben ist.

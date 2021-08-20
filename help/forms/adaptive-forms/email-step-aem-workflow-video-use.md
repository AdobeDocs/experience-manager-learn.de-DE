---
title: E-Mail-Sendeschritt des Forms Workflows verwenden
description: Der Schritt E-Mail senden wurde in AEM Forms 6.4 eingeführt. Mit diesem Schritt können wir Geschäftsprozesse oder Workflows erstellen, mit denen Sie E-Mails mit oder ohne Anhänge senden können. Im folgenden Video werden die Schritte zum Konfigurieren der Komponente "E-Mail senden"erläutert
feature: Workflow
version: 6.4,6.5
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 3%

---


# E-Mail-Sendeschritt des Forms Workflows verwenden {#using-send-email-step-of-forms-workflow}

Der Schritt E-Mail senden wurde in AEM Forms 6.4 eingeführt. Mit diesem Schritt können wir Geschäftsprozesse oder Workflows erstellen, mit denen Sie E-Mails mit oder ohne Anhänge senden können. Im folgenden Video werden die Schritte zum Konfigurieren der Komponente &quot;E-Mail senden&quot;erläutert.

>[!VIDEO](https://video.tv.adobe.com/v/21499/?quality=9&learn=on)

Im Rahmen dieses Artikels führen wir Sie durch das folgende Anwendungsbeispiel:

1. Ein Benutzer füllt das Anforderungsformular für die Zeitüberschreitung aus
1. Beim Senden des Formulars wird AEM Workflow ausgelöst
1. Der AEM Workflow verwendet die Komponente E-Mail senden , um eine E-Mail mit dem DoR als Anlage zu senden

Bevor Sie den Schritt E-Mail senden verwenden, stellen Sie sicher, dass Sie den Day CQ Mail Service über [configMgr](http://localhost:4502/system/console/configMgr) konfigurieren. Umgebungsspezifische Werte bereitstellen

![Konfigurieren des Day CQ Mail Service](assets/mailservice.png)

Als Teil der mit diesem Artikel verknüpften Assets erhalten Sie Folgendes

1. Adaptives Formular, das den Workflow bei der Übermittlung Trigger
1. Beispielworkflow, der eine E-Mail mit DOR als Anhang sendet
1. OSGi-Bundle, das die Metadateneigenschaften erstellt

Führen Sie die folgenden Schritte aus, um das Beispiel auf Ihrem System auszuführen:

1. [Bereitstellen des Entwicklungs-mit-Service-Benutzer-Bundles](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Herunterladen und Installieren von setvalue ](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)bundleDieses Bundle enthält den Code zum Erstellen der Metadateneigenschaften als Teil des Prozessschritts des Workflows.
1. [Konfigurieren des Day CQ Mail Service](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html)
1. [Importieren und installieren Sie die mit diesem Artikel verknüpften Assets mithilfe des Paketmanagers in CRX](assets/emaildoraemformskt.zip)
1. Starten Sie das adaptive Formular [a1/>. ](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled) Füllen Sie die erforderlichen Felder aus und senden Sie sie.
1. Sie sollten eine E-Mail mit DocumentOfRecord als Anlage erhalten

Durchsuchen Sie das [Workflow-Modell](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html).

Sehen Sie sich den Prozessschritt des Workflows an. Der mit dem Prozessschritt verknüpfte benutzerdefinierte Code erstellt Metadaten-Eigenschaftsnamen und legt deren Werte aus den gesendeten Daten fest. Diese Werte werden dann von der Komponente E-Mail senden verwendet.

>[!NOTE]
>
>In AEM Forms 6.5 und höher benötigen Sie diesen benutzerdefinierten Code nicht, um Metadateneigenschaften zu erstellen. Bitte verwenden Sie die Variablenfunktion in AEM Workflow

Stellen Sie sicher, dass die Registerkarte &quot;Anlagen&quot;der Komponente E-Mail senden gemäß dem Screenshot unten konfiguriert ist.
![Tab E-Mail-Anhang senden](assets/sendemailcomponentconfigure.jpg)Der Wert &quot;DOR.pdf&quot;muss mit dem Wert übereinstimmen, der im Pfad des Datensatzdokuments angegeben ist, das in den Übermittlungsoptionen Ihres adaptiven Formulars angegeben ist.


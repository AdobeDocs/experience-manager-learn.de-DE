---
title: Senden von E-Mails beim Übermitteln adaptiver Formulare
seo-title: Senden von E-Mails beim Übermitteln adaptiver Formulare
description: Bestätigungs-E-Mail bei Übermittlung des adaptiven Formulars mit der Komponente E-Mail senden senden senden senden
seo-description: Bestätigungs-E-Mail bei Übermittlung des adaptiven Formulars mit der Komponente E-Mail senden senden senden senden
uuid: 6c9549ba-cb56-4d69-902c-45272a8fd17e
feature: Adaptive Formulare
topics: authoring, integrations
audience: developer
doc-type: article
activity: use
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 3%

---


# Senden von E-Mails zur Übermittlung adaptiver Formulare {#sending-email-on-adaptive-form-submission}

Eine der häufigsten Aktionen besteht darin, eine Bestätigungs-E-Mail an den Absender zu senden, wenn das adaptive Formular erfolgreich übermittelt wurde. Dazu wählen wir die Übermittlungsaktion &quot;E-Mail senden&quot;.

Sie können eine E-Mail-Vorlage verwenden oder einfach den Text der E-Mail eingeben, wie in diesem Screenshot unten dargestellt.

Beachten Sie die Syntax zum Einfügen von Formularfeldwerten in die E-Mail. Wir haben auch die Möglichkeit, Formularanhänge in die E-Mail einzuschließen, indem Sie das Kontrollkästchen &quot;Anlagen einschließen&quot;in den Konfigurationseigenschaften aktivieren.

Wenn das adaptive Formular gesendet wird, erhält der Empfänger eine E-Mail.

![SendEmail](assets/sendemailaction.gif)

## Erforderliche Konfigurationen {#configurations-needed}

Sie müssen den Day CQ Mail-Dienst konfigurieren. Dies kann konfiguriert werden, indem Sie Ihren Browser auf [Felix Configuration Manager](http://localhost:4502/system/console/configMgr) verweisen.

Der Screenshot zeigt die Konfigurationseigenschaften für den Adobe-E-Mail-Server.

![mailservice](assets/mailservice.png)

Gehen Sie wie folgt vor, um dies auf Ihrem Server auszuführen:

* [Importieren Sie die mit diesem Artikel verknüpften ](assets/timeoffrequest.zip) Assets in AEM mit dem Package Manager.

* Öffnen Sie [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* Füllen Sie die Details aus. Stellen Sie sicher, dass Sie im E-Mail-Feld eine gültige E-Mail-Adresse angeben.

* Senden Sie das Formular.

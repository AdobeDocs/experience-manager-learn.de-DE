---
title: Senden von E-Mails beim Übermitteln adaptiver Formulare
seo-title: Sending Email on Adaptive Form Submission
description: Bestätigungs-E-Mail bei Übermittlung des adaptiven Formulars mit der Komponente E-Mail senden senden senden senden
seo-description: Send confirmation email on adaptive form submission using the send email component
uuid: 6c9549ba-cb56-4d69-902c-45272a8fd17e
feature: Adaptive Forms
topics: authoring, integrations
audience: developer
doc-type: article
activity: use
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: Development
role: Developer
level: Beginner
exl-id: 19c5aeec-2893-4ada-b6df-b80c4be2468a
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 3%

---

# Senden von E-Mails beim Übermitteln adaptiver Formulare {#sending-email-on-adaptive-form-submission}

Eine der häufigsten Aktionen besteht darin, eine Bestätigungs-E-Mail an den Absender zu senden, wenn das adaptive Formular erfolgreich übermittelt wurde. Dazu wählen wir die Übermittlungsaktion &quot;E-Mail senden&quot;.

Sie können eine E-Mail-Vorlage verwenden oder einfach den Text der E-Mail eingeben, wie in diesem Screenshot unten dargestellt.

Beachten Sie die Syntax zum Einfügen von Formularfeldwerten in die E-Mail. Wir haben auch die Möglichkeit, Formularanhänge in die E-Mail einzuschließen, indem Sie das Kontrollkästchen &quot;Anlagen einschließen&quot;in den Konfigurationseigenschaften aktivieren.

Wenn das adaptive Formular gesendet wird, erhält der Empfänger eine E-Mail.

![SendEmail](assets/sendemailaction.gif)

## Erforderliche Konfigurationen {#configurations-needed}

Sie müssen den Day CQ Mail-Dienst konfigurieren. Dies kann konfiguriert werden, indem Sie Ihren Browser auf [Felix Configuration Manager](http://localhost:4502/system/console/configMgr)

Der Screenshot zeigt die Konfigurationseigenschaften für den Adobe-E-Mail-Server.

![mailservice](assets/mailservice.png)

Gehen Sie wie folgt vor, um dies auf Ihrem Server auszuführen:

* [Importieren der Assets](assets/timeoffrequest.zip) mit diesem Artikel in AEM mithilfe des Paketmanagers verknüpft ist.

* Öffnen Sie die [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* Füllen Sie die Details aus. Stellen Sie sicher, dass Sie im E-Mail-Feld eine gültige E-Mail-Adresse angeben.

* Formular senden.

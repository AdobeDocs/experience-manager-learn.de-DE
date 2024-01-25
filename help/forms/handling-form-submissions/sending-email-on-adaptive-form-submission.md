---
title: Senden von E-Mails beim Übermitteln adaptiver Formulare
description: Bestätigungs-E-Mail bei Übermittlung des adaptiven Formulars mit der Komponente „E-Mail senden“ senden
feature: Adaptive Forms
doc-type: article
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: Development
role: Developer
level: Beginner
exl-id: 19c5aeec-2893-4ada-b6df-b80c4be2468a
last-substantial-update: 2020-07-07T00:00:00Z
duration: 45
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 100%

---

# Senden von E-Mails beim Übermitteln adaptiver Formulare {#sending-email-on-adaptive-form-submission}

Eine übliche Aktion besteht darin, eine Bestätigungs-E-Mail an den Absender zu senden, wenn das adaptive Formular erfolgreich übermittelt wurde. Dazu wählen wir die Übermittlungsaktion „E-Mail senden“.

Sie können eine E-Mail-Vorlage verwenden oder einfach den Text der E-Mail eingeben, wie im Screenshot unten dargestellt.

Beachten Sie die Syntax zum Einfügen von Formularfeldwerten in die E-Mail. Wir haben auch die Möglichkeit, Formularanhänge in die E-Mail einzuschließen, indem das Kontrollkästchen „Anhänge einschließen“ in den Konfigurationseigenschaften aktiviert wird.

Wenn das adaptive Formular gesendet wird, erhält der Empfänger eine E-Mail.

![SendEmail](assets/sendemailaction.gif)

## Erforderliche Konfigurationen {#configurations-needed}

Sie müssen den Day CQ Mail-Dienst konfigurieren. Dies kann über Ihren Browser mit dem [Felix Configuration Manager](http://localhost:4502/system/console/configMgr) konfiguriert werden

Der Screenshot zeigt die Konfigurationseigenschaften für den Adobe-E-Mail-Server.

![mailservice](assets/mailservice.png)

Gehen Sie wie folgt vor, um dies auf Ihrem Server auszuführen:

* [Importieren der Assets](assets/timeoffrequest.zip), die mit diesem Artikel verknüpft sind, in AEM mithilfe des Package Managers.

* Öffnen Sie die [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* Füllen Sie die Details aus. Stellen Sie sicher, dass Sie im E-Mail-Feld eine gültige E-Mail-Adresse angeben.

* Formular senden.

---
title: Senden von E-Mails beim Senden von adaptiven Formularen
seo-title: Senden von E-Mails beim Senden von adaptiven Formularen
description: Senden von Bestätigungs-E-Mail beim Senden des adaptiven Formulars mit der Komponente "E-Mail senden"
seo-description: Senden von Bestätigungs-E-Mail beim Senden des adaptiven Formulars mit der Komponente "E-Mail senden"
uuid: 6c9549ba-cb56-4d69-902c-45272a8fd17e
feature: Adaptive Formulare
topics: authoring, integrations
audience: developer
doc-type: article
activity: use
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: Entwicklung
role: Entwickler
level: Anfänger
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 3%

---


# Senden von E-Mails zur Übermittlung adaptiver Formulare {#sending-email-on-adaptive-form-submission}

Eine der gängigen Aktionen besteht darin, eine Bestätigungs-E-Mail nach erfolgreicher Übermittlung des adaptiven Formulars an den Übermittler zu senden. Um dies zu erreichen, wählen wir &quot;E-Mail senden&quot; als Übermittlungsaktion.

Sie können eine E-Mail-Vorlage verwenden oder einfach den Text der E-Mail eingeben, wie in diesem Screenshot unten gezeigt.

Beachten Sie die Syntax zum Einfügen von Formularfeldwerten in die E-Mail.Wir haben auch die Möglichkeit, Formularanlagen in die E-Mail einzuschließen, indem Sie das Kontrollkästchen &quot;Anlagen einschließen&quot;in den Konfigurationseigenschaften aktivieren.

Wenn das adaptive Formular gesendet wird, erhält der Empfänger eine E-Mail.

![SendEmail](assets/sendemailaction.gif)

## Benötigte Konfigurationen {#configurations-needed}

Sie müssen den Day CQ Mail-Dienst konfigurieren. Dies kann konfiguriert werden, indem Sie Ihren Browser auf [Felix Configuration Manager](http://localhost:4502/system/console/configMgr) verweisen

Der Screenshot zeigt die Konfigurationseigenschaften für den Adobe Mail-Server.

![mailservice](assets/mailservice.png)

Gehen Sie wie folgt vor, um dies auf Ihrem Server auszuprobieren:

* [Importieren Sie die mit diesem Artikel verknüpften ](assets/timeoffrequest.zip) Assets in AEM mit dem Package Manager.

* Öffnen Sie [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* Füllen Sie die Details aus.Achten Sie darauf, im E-Mail-Feld eine gültige E-Mail-Adresse anzugeben.

* Senden Sie das Formular.

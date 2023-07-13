---
title: Integrieren von AEM Forms mit SendGrid
description: Nutzen Sie die Cloud-basierte SengGrid-E-Mail-Versandplattform mit AEM Forms.
feature: Adaptive Forms
version: 6.4,6.5
kt: 13605
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-07-14T00:00:00Z
source-git-commit: cc24ebca488ea286e8a4605edfb39420c1c10022
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 1%

---

# Integrieren von AEM Forms mit SendGrid

Willkommen in diesem technischen Handbuch, in dem wir den Prozess des Versands von E-Mails mit dynamischen SendGrid-Vorlagen aus AEM Forms untersuchen. In diesem Handbuch erfahren Sie, wie Sie dynamische Vorlagen nutzen können, um E-Mail-Inhalte effektiv zu personalisieren.

Mit dynamischen Vorlagen können Sie E-Mail-Vorlagen erstellen, die Empfängern je nach den im adaptiven Formular erfassten Daten unterschiedliche Inhalte anzeigen. Mithilfe von Personalisierungsvariablen können Sie zielgerichtete und benutzerdefinierte E-Mail-Erlebnisse bereitstellen, die bei Ihrer Zielgruppe ankommen.

Darüber hinaus werden wir uns mit der Verwendung der Swagger-Datei befassen, mit der Sie Ihre E-Mails weiter personalisieren können, indem Sie den Namen und die E-Mail-Adresse des Kunden sowie die entsprechende dynamische E-Mail-Vorlage auswählen.

Befolgen Sie die schrittweisen Anweisungen in diesem Dokument, um die Leistungsfähigkeit dynamischer SendGrid-Vorlagen und AEM Forms zu nutzen und Ihre E-Mail-Kommunikation auf neue Interaktions- und Relevanzstufen zu erweitern. Fangen wir an!

## Voraussetzungen

Bevor Sie mit dem Versand von E-Mails mit dynamischen SendGrid-Vorlagen aus AEM Forms fortfahren, stellen Sie sicher, dass folgende Voraussetzungen erfüllt sind:

1. **SendGrid-Konto**: Registrieren Sie sich unter für ein SendGrid-Konto. [https://sendgrid.com](https://sendgrid.com) , um auf ihre E-Mail-Versanddienste zuzugreifen. Sie benötigen die Kontoanmeldeinformationen, um SendGrid mit AEM Forms zu integrieren.
1. **Kenntnis der Erstellung von Data Sources**: Sie verfügen über Kenntnisse zum Erstellen von Datenquellen in AEM Forms. Lesen Sie bei Bedarf das Handbuch unter [Erstellen von Datenquellen](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) für detaillierte Anweisungen.
1. **Vertrautheit mit dem Formulardatenmodell**: Machen Sie sich mit dem Konzept des Formulardatenmodells in AEM Forms vertraut. Lesen Sie bei Bedarf die Dokumentation zu [Erstellen von Formulardatenmodellen](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=de) um sicherzustellen, dass Sie über das erforderliche Verständnis verfügen.

Wenn Sie diese Voraussetzungen erfüllen, verfügen Sie über die nötigen Kenntnisse und Ressourcen, um E-Mails mit dynamischen SendGrid-Vorlagen aus AEM Forms effektiv zu versenden.

## Beispiel-Assets

Die in diesem Artikel bereitgestellten Beispiel-Assets umfassen:

* **[Swagger-Datei](assets/SendGridWithDynamicTemplate.yaml)**: Diese Datei ermöglicht den Versand von E-Mails mithilfe einer dynamischen E-Mail-Vorlage. Es bietet die erforderlichen Spezifikationen und Konfigurationen zur Integration mit SendGrid und AEM Forms für einen nahtlosen E-Mail-Versand.

Nutzen Sie die bereitgestellte Swagger-Datei als Referenz oder Ausgangspunkt für die Implementierung von E-Mail-Funktionen mit dynamischen Vorlagen.

## Testanweisungen

Gehen Sie wie folgt vor, um die in diesem Handbuch beschriebenen Funktionen zu testen:

1. Laden Sie die [Swagger-Datei](assets/SendGridWithDynamicTemplate.yaml) im Ordner &quot;Assets&quot;bereitgestellt.
2. Erstellen Sie eine Restful-Datenquelle mithilfe der heruntergeladenen Swagger-Datei und Ihrer SendGrid-Anmeldeinformationen.
3. Erstellen Sie ein Formulardatenmodell basierend auf der Restful-Datenquelle.
4. Rufen Sie die `mail/send` POST des Formulardatenmodells entsprechend Ihren Anforderungen. Beispielsweise können Sie die E-Mail beim Klicken auf eine Schaltfläche Trigger oder als Teil Ihres AEM Forms-Workflows einfügen.

Die Beispiel-Payload für den Dienst lautet wie folgt. Ersetzen Sie die Platzhalterwerte durch Ihre eigenen Daten:

```json
{
    "sendgridpayload": {
        "from": {
            "email": "gs@xyz.com"
        },
        "personalizations": [{
            "to": [{
                "email": "johndoe@xyz.com"
            }],
            "dynamic_template_data": {
                "customerName": "John Doe"
            }
        }],
        "template_id": "d-72aau292a3bd60b5300c"
    }
}
```

Stellen Sie sicher, dass `template_id` entspricht der Kennung Ihrer dynamischen E-Mail-Vorlage SendGrid und die E-Mail-Adressen sind gültig und von SendGrid überprüft. Die Werte in `personalizations` können Sie die E-Mail mithilfe der vom Benutzer eingegebenen Daten aus dem adaptiven Formular personalisieren.

Indem Sie diese Schritte ausführen und die bereitgestellte Payload anpassen, können Sie die Integration dynamischer SendGrid-Vorlagen mit AEM Forms effektiv testen.


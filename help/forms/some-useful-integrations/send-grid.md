---
title: Integrieren von AEM Forms in SendGrid
description: Nutzen Sie die Cloud-basierte E-Mail-Versandplattform SendGrid mit AEM Forms.
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-13605
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-07-14T00:00:00Z
exl-id: 62b73f4b-69d8-4ede-9d57-3d6472d25d5a
duration: 142
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 100%

---

# Integrieren von AEM Forms in SendGrid

Willkommen bei diesem technischen Handbuch, in dem wir den Prozess des E-Mail-Versands mit dynamischen SendGrid-Vorlagen aus AEM Forms untersuchen. In diesem Handbuch erfahren Sie, wie Sie dynamische Vorlagen nutzen können, um E-Mail-Inhalte effektiv zu personalisieren.

Mit dynamischen Vorlagen können Sie E-Mail-Vorlagen erstellen, die Empfängerinnen und Empfängern je nach den im adaptiven Formular erfassten Daten unterschiedliche Inhalte präsentieren. Durch Personalisierungsvariablen können Sie zielgerichtete und benutzerdefinierte E-Mail-Erlebnisse bereitstellen, die bei Ihrer Zielgruppe ankommen.

Darüber hinaus werden wir uns mit der Verwendung der Swagger-Datei befassen, mit der Sie Ihre E-Mails weiter personalisieren können, indem Sie den Namen und die E-Mail-Adresse der Kundin oder des Kunden einschließen sowie die entsprechende dynamische E-Mail-Vorlage auswählen.

Befolgen Sie die schrittweisen Anweisungen in diesem Dokument, um die Leistungsfähigkeit dynamischer SendGrid-Vorlagen und AEM Forms zu nutzen und Ihre E-Mail-Kommunikation in eine neue Dimension der Interaktion und Relevanz zu führen. Fangen wir an!

## Voraussetzungen

Bevor Sie mit dem Versand von E-Mails mit dynamischen SendGrid-Vorlagen aus AEM Forms fortfahren, stellen Sie sicher, dass folgende Voraussetzungen erfüllt sind:

1. **SendGrid-Konto**: Registrieren Sie sich auf [https://sendgrid.com](https://sendgrid.com) für ein SendGrid-Konto, um auf die entsprechenden E-Mail-Versanddienste zuzugreifen. Sie benötigen die Konto-Anmeldeinformationen, um SendGrid in AEM Forms zu integrieren.
1. **Vertrautheit mit der Erstellung von Datenquellen**: Sie verfügen über Kenntnisse zum Erstellen von Datenquellen in AEM Forms. Ausführliche Anweisungen finden Sie ggf. in der Dokumention zum [Erstellen von Datenquellen](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=de).
1. **Vertrautheit mit dem Formulardatenmodell**: Sie verstehen das Konzept des Formulardatenmodells in AEM Forms. Lesen Sie bei Bedarf die Dokumentation zum [Erstellen von Formulardatenmodellen](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=de), um sicherzustellen, dass Sie über die erforderlichen Kenntnisse verfügen.

Wenn Sie diese Voraussetzungen erfüllen, verfügen Sie über die nötigen Kenntnisse und Ressourcen, um E-Mails mit dynamischen SendGrid-Vorlagen aus AEM Forms effektiv zu versenden.

## Beispiel-Assets

Im Rahmen dieses Artikels werden folgende Beispiel-Assets bereitgestellt:

* **[Swagger-Datei](assets/SendGridWithDynamicTemplate.yaml)**: Diese Datei ermöglicht den Versand von E-Mails mithilfe einer dynamischen E-Mail-Vorlage. Sie liefert die erforderlichen Spezifikationen und Konfigurationen zur Integration in SendGrid und AEM Forms für einen nahtlosen E-Mail-Versand.

Nutzen Sie die bereitgestellte Swagger-Datei als Referenz oder Ausgangspunkt, um E-Mail-Funktionen mit dynamischen Vorlagen zu implementieren.

## Testanweisungen

Gehen Sie wie folgt vor, um die in diesem Handbuch beschriebenen Funktionen zu testen:

1. Laden Sie die [Swagger-Datei](assets/SendGridWithDynamicTemplate.yaml) aus dem Assets-Ordner herunter.
2. Erstellen Sie eine Restful-Datenquelle mithilfe der heruntergeladenen Swagger-Datei und Ihrer SendGrid-Anmeldeinformationen.
3. Erstellen Sie ein Formulardatenmodell basierend auf der Restful-Datenquelle.
4. Rufen Sie den `mail/send`-POST-Vorgang des Formulardatenmodells entsprechend Ihren Anforderungen auf. Beispielsweise können Sie die E-Mail beim Klicken auf eine Schaltfläche auslösen oder als Teil Ihres AEM Forms-Workflows einschließen.

Die Beispiel-Payload für den Dienst ist wie folgt. Ersetzen Sie die Platzhalterwerte durch Ihre eigenen Daten:

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

Stellen Sie sicher, dass `template_id` der ID Ihrer dynamischen SendGrid-E-Mail-Vorlage entspricht und die E-Mail-Adressen gültig sind und von SendGrid überprüft wurden. Anhand der Werte im Abschnitt `personalizations` können Sie die E-Mail mithilfe der von der Benutzerin oder dem Benutzer eingegebenen Daten aus dem adaptiven Formular personalisieren.

Indem Sie diese Schritte ausführen und die bereitgestellte Payload anpassen, können Sie die Integration dynamischer SendGrid-Vorlagen in AEM Forms effektiv testen.

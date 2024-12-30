---
title: Integrieren von AEM Forms as a Cloud Service und Marketo (Teil 3)
description: Erfahren Sie, wie Sie AEM Forms und Marketo mithilfe des AEM Forms-Formulardatenmodells integrieren.
feature: Form Data Model,Integration
version: Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
exl-id: 43737765-b1ea-4594-853a-d78f41136b5e
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 100%

---

# Erstellen eines Formulardatenmodells

Nach dem Konfigurieren der Datenquelle besteht der nächste Schritt darin, ein Formulardatenmodell zu erstellen, das auf der in einem früheren Schritt konfigurierten Datenquelle basiert. Gehen Sie wie folgt vor, um ein Formulardatenmodell zu erstellen:

Verweisen Sie den Browser auf die [Datenintegrationsseite.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) Hier werden alle in Ihrer AEM-Instanz erstellten Datenintegrationen aufgelistet.

1. Klicken Sie auf „Erstellen“ > „Formulardatenmodell erstellen“.
1. Geben Sie einen aussagekräftigen Titel wie „FormulareUndMarketo“ ein und klicken Sie auf „Weiter“.
1. Wählen Sie die im vorherigen Schritt konfigurierte Datenquelle aus und klicken Sie auf „Erstellen und Bearbeiten“, um das Formulardatenmodell im Bearbeitungsmodus zu öffnen.
1. Erweitern Sie den Knoten „FormulareUndMarketo“. Erweitern Sie den Knoten „Dienste“.
1. Wählen Sie den ersten GET-Vorgang aus
1. Klicken Sie auf „Ausgewählte hinzufügen“.
1. Klicken Sie im Dialogfeld „Zugeordnete Modellobjekte hinzufügen“ auf „Alle auswählen“ und dann auf „Hinzufügen“.
1. Speichern Sie das Formulardatenmodell, indem Sie auf die Schaltfläche „Speichern“ klicken.
1. Wechseln Sie zur Registerkarte „Dienste“.
1. Wählen Sie den einzigen aufgelisteten Dienst aus und klicken Sie auf „Dienst testen“.
1. Geben Sie eine gültige Lead-ID ein und klicken Sie auf „Testen“. Wenn alles reibungslos funktioniert, sollten die Lead-Details zurückgegeben werden, wie im Screenshot unten dargestellt
   ![Testergebnisse](assets/testresults.png)

Sie können nun ein auf diesem Formulardatenmodell basierendes adaptives Formular erstellen, um Marketo-Objekte einzufügen und abzurufen.

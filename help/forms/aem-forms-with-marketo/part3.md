---
title: AEM Forms in Marketo (Teil 3)
description: Tutorial zur Integration von AEM Forms in Marketo mithilfe des AEM Forms-Formulardatenmodells.
feature: Adaptive Forms, Form Data Model
version: Experience Manager 6.4, Experience Manager 6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 7096340b-8ccf-4f5e-b264-9157232e96ba
duration: 78
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '217'
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

## Nächste Schritte

[Anwenden für Tests](./part4.md)

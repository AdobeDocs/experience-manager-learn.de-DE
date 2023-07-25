---
title: Berichte zu gesendeten Formulardatenfeldern mit Adobe Analytics
description: Integrieren von AEM Forms CS mit Adobe Analytics für Berichte zu Formulardatenfeldern
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
kt: 12557
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 9982e041-fff7-4be6-91c9-e322d2fd3e01
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 5%

---

# Regel definieren

In der Eigenschaft &quot;Tags&quot;wurden zwei neue erstellt [Regeln](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html) (**Fehler bei Feldvalidierung und FormularSubmit**).

![adaptives Formular](assets/rules.png)


## Feldvalidierungsfehler

Die **Feldvalidierungsfehler** -Regel wird jedes Mal ausgelöst, wenn im Feld des adaptiven Formulars ein Validierungsfehler auftritt. Wenn beispielsweise in unserem Formular die Telefonnummer oder die E-Mail nicht das erwartete Format aufweist, wird eine Validierungsfehlermeldung angezeigt.

Die Regel &quot;Feldvalidierungsfehler&quot;wird konfiguriert, indem das Ereignis auf _**Adobe Experience Manager Forms-Error**_ wie im Screenshot gezeigt



![applicant-state-Residence](assets/field_validation_error_rule.png)

Adobe Analytics - Variablen festlegen ist wie folgt konfiguriert

![Aktion festlegen](assets/field_validation_action_rule.png)

## Formular-Senderegel

Die Regel &quot;Formular senden&quot;wird jedes Mal ausgelöst, wenn ein adaptives Formular erfolgreich gesendet wurde.

Die Regel zum Senden von Formularen wird mithilfe der _**Adobe Experience Manager Forms - Submit**_ event

![form-submit-rule](assets/form-submit-rule.png)

In der Regel &quot;Formular senden&quot;ist der Wert des Datenelements _**applicantStateOfResidence**_ wird prop5 zugeordnet und der Wert des Datenelements FormTitle wird prop8 zugeordnet.

Die Variablen Adobe Analytics - Set sind wie folgt konfiguriert.
![form-submit-rule-set-variables](assets/form-submit-set-variable.png)

Wenn Sie bereit sind, Ihren Tags-Code zu testen,[Veröffentlichen der an den Tags vorgenommenen Änderungen](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html) mithilfe des Veröffentlichungsflusses.

## Nächste Schritte

[Testen der Lösung](./test.md)
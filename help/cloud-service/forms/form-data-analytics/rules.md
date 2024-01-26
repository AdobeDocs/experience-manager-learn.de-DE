---
title: Reporting zu übermittelten Formulardatenfeldern mit Adobe Analytics
description: Integrieren von AEM Forms CS mit Adobe Analytics für das Reporting zu Formulardatenfeldern
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 9982e041-fff7-4be6-91c9-e322d2fd3e01
duration: 55
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 100%

---

# Definieren der Regel

In der Eigenschaft „Tags“ haben wir zwei neue [Regeln](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html?lang=de) (**Fehler bei Feldvalidierung und FormSubmit**) erstellt.

![adaptive-form](assets/rules.png)


## Feldvalidierungsfehler

Die **Feldvalidierungsfehler**-Regel wird jedes Mal ausgelöst, wenn in einem Feld des adaptiven Formulars ein Validierungsfehler auftritt. Wenn beispielsweise in unserem Formular die Telefonnummer oder die E-Mail-Adresse nicht das erwartete Format aufweist, wird eine Validierungsfehlermeldung angezeigt.

Die Feldvalidierungsfehler-Regel wird konfiguriert, indem das Ereignis auf _**Adobe Experience Manager Forms-Fehler**_ festgelegt wird, wie im Screenshot gezeigt



![applicant-state-residence](assets/field_validation_error_rule.png)

„Adobe Analytics – Variablen festlegen“ ist wie folgt konfiguriert

![Aktion festlegen](assets/field_validation_action_rule.png)

## Formularübermittlungs-Regel

Die Formularsenderegel wird jedes Mal ausgelöst, wenn ein adaptives Formular gesendet wurde.

Die Formularsenderegel wird mithilfe des _**Adobe Experience Manager Forms – Absenden**_-Ereignisses konfiguriert

![form-submit-rule](assets/form-submit-rule.png)

In der Formularsenderegel ist der Wert des Datenelements _**applicantStateOfResidence**_ prop5 zugeordnet und der Wert des Datenelements „FormTitle“ ist prop8 zugeordnet.

„Adobe Analytics – Variablen festlegen“ ist wie folgt konfiguriert.
![form-submit-rule-set-variables](assets/form-submit-set-variable.png)

Wenn Sie bereit sind, Ihren Tags-Code zu testen,[veröffentlichen Sie die an den Tags vorgenommenen Änderungen](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html?lang=de) mithilfe des Veröffentlichungsflusses.

## Nächste Schritte

[Testen der Lösung](./test.md)
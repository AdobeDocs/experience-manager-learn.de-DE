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
exl-id: b9dc505d-72c8-4b6a-974b-fc619ff7c256
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 6%

---

# Erstellen von Datenelementen

In der Eigenschaft &quot;Tags&quot;wurden zwei neue Datenelemente hinzugefügt (ApplicationStateOfResidence und validationError).

![adaptives Formular](assets/data_elements.png)

## applicantStateOfResidence

Die **applicantStateOfResidence** Das Datenelement wurde durch Auswahl von **Core** in der Dropdown-Liste Erweiterung und **Benutzerspezifischer Code** für den Datenelementtyp, wie im Screenshot unten dargestellt
![applicant-state-Residence](assets/applicantstateofresidence.png)

Der folgende benutzerspezifische Code wurde verwendet, um den Wert aus dem **_state_** Feld für adaptives Formular.

```javascript
// use the GuideBridge API to access adaptive form elements
//The state field's SOM expression is used to access the state field
var ApplicantsStateOfResidence = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].state[0]").value;
_satellite.logger.log("Returning  Applicants State Of Residence is "+ApplicantsStateOfResidence);
return ApplicantsStateOfResidence;
```

## validationError

Die **ValidationError** Das Datenelement wurde durch Auswahl von **Core** in der Dropdown-Liste Erweiterung und **Benutzerspezifischer Code** für den Datenelementtyp, wie im Screenshot unten dargestellt

![validation-error](assets/validation-error.png)

Der folgende benutzerspezifische Code wurde geschrieben, um die `validationError` Datenelementwert.

```javascript
var validationError = "";
// Using GuideBridge API to access adaptive forms fields using the fields SOM expression
var tel = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].telephone[0]");
var email = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].email[0]");

_satellite.logger.log("Got tel in Tags custom script "+tel.isValid)
_satellite.logger.log("Got email in Tags custom script "+email.isValid)

if (tel.isValid == false) {  
  validationError = "error: telephone number";
  _satellite.logger.log("Validation error is "+ validationError);
}

if (email.isValid == false) {  
  validationError = "error: invalid email";
  _satellite.logger.log("Validation error is "+ validationError);
}

return validationError;
```

## Nächste Schritte

[Regeln erstellen](./rules.md)
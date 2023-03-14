---
title: Berichte zu gesendeten Formulardatenfeldern mit Adobe Analytics
description: Integrieren von AEM Forms CS mit Adobe Analytics für Berichte zu Formulardatenfeldern
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
source-git-commit: 439167be96959baea54f50a221c6d26f8fab78b2
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 0%

---

# Erstellen entsprechender Datenelemente

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
_satellite.logger.log(" Returning  Applicants State Of Residence is "+ApplicantsStateOfResidence);
return ApplicantsStateOfResidence;
```

## validationError

Die **ValidationError** Das Datenelement wurde durch Auswahl von **Core** in der Dropdown-Liste Erweiterung und **Benutzerspezifischer Code** für den Datenelementtyp, wie im Screenshot unten dargestellt

![validation-error](assets/validation-error.png)

Der folgende benutzerdefinierte Code wurde geschrieben, um den Datenelementwert validationError festzulegen.

```javascript
var validationError = "";
// Using GuideBridge API to access adaptive forms fields using the fields SOM expression
var tel = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].telephone[0]");
var email = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].email[0]");
_satellite.logger.log("Got tel in Tags custom script "+tel.isValid)
_satellite.logger.log("Got email in Tags custom script "+email.isValid)
if(tel.isValid == false)
{  
  validationError = "error: telephone number";
  _satellite.logger.log("Validation error is "+ validationError);
}

if(email.isValid == false)
{  
  validationError = "error: invalid email";
  _satellite.logger.log("Validation error is "+ validationError);
}

return validationError;
```

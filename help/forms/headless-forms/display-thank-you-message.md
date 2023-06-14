---
title: Dankesmeldung bei Formularübermittlung anzeigen
description: Verwenden Sie den Handler onSubmitSuccess , um die konfigurierte Dankesnachricht in der React-App anzuzeigen.
feature: Adaptive Forms
version: 6.5
kt: 13490
topic: Development
role: User
level: Intermediate
source-git-commit: 10ff0d87991d7766d5ca9563062a2f7be6035e43
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 0%

---

# Anzeige der konfigurierten Dankesnachricht

Eine Dankesnachricht bei der Formularübermittlung ist eine umsichtige Möglichkeit, den Benutzer für das Ausfüllen und Übermitteln eines Formulars zu danken und ihm zu danken. Es dient als Bestätigung, dass ihre Einreichung empfangen und begrüßt wurde. Die Dankesnachricht wird über die Registerkarte &quot;Übermittlung&quot;im Guide-Container des adaptiven Formulars konfiguriert

![thank-you-message](assets/thank-you-message.png)

Auf die konfigurierte Dankesmeldung kann im onSuccess-Ereignishandler der Super-Komponente AdaptiveForm zugegriffen werden.
Der Code für die Zuordnung des onSuccess-Ereignisses und des Codes für den onSuccess-Ereignishandler ist unten aufgeführt

```javascript
<AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
```

```javascript
const onSuccess=(action) =>{
        let body = action.payload?.body;
        debugger;
        setThankYouMessage(body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));
        console.log("Thank you message "+body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));

      }
```

Der vollständige Code der Komponente Kontaktfunktion ist unten angegeben.

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import { useState,useEffect } from "react";
import { AdaptiveForm } from "@aemforms/af-react-renderer";
export default function Contact(){
  
    const [selectedForm, setForm] = useState("");
    const [thankYouMessage, setThankYouMessage] = useState("");
    const [formSubmitted, setFormSubmitted] = useState(false);
  
    const extendMappings = {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
      };
     const onSuccess=(action) =>{
        let body = action.payload?.body;
        debugger;
        setFormSubmitted(true);
        setThankYouMessage(body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));
        // Remove any html tags in the thank you message
        console.log("Thank you message "+body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));

      }
      
      const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/L2NvbnRlbnQvZm9ybXMvYWYvY29udGFjdHVz');
        // Get the form id manually using the listform api
        let formJSON = await resp.json();
        setForm(formJSON.afModelDefinition)
      }
      useEffect( ()=>{
        getForm()
        

    },[]);
    
    return(
        
        <div>
           {!formSubmitted ?
            (
                <div>
                    <h1>Get in touch with us!!!!</h1>
                    <AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
                </div>
            ) :
            (
                <div>
                    <div>{thankYouMessage}</div>
                </div>
            )}
        </div>
      
          
        
    )
}
```

Der obige Code verwendet native HTML-Komponenten, die den im adaptiven Formular verwendeten Komponenten zugeordnet sind. Beispielsweise ordnen wir die Texteingabe-Komponente des adaptiven Formulars der TextField-Komponente zu. Die im Artikel verwendeten nativen Komponenten [können Sie hier herunterladen](./assets/native-components.zip)


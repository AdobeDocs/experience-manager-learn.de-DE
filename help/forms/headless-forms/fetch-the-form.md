---
title: Abrufen der JSON-Inhalte des einzubettenden adaptiven Formulars
description: Verwenden der API zum Abrufen der JSON-Inhalte des adaptiven Formulars
feature: Adaptive Forms
version: 6.5
jira: KT-13285
topic: Development
role: User
level: Intermediate
exl-id: ee534724-54ea-48e1-8c92-de1c56a928d4
duration: 49
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 100%

---

# Abrufen der JSON-Inhalte des Formulars

Melden Sie sich bei Ihrer AEM Forms-Autoreninstanz an und erstellen Sie ein neues adaptives Formular mithilfe der Vorlage **Leer mit Kernkomponenten**. Veröffentlichen Sie Ihr Formular in Ihrer Veröffentlichungsinstanz.

Um das Formular einzubetten, rufen wir zunächst die JSON-Inhalte des adaptiven Formulars ab, indem wir einen GET-Aufruf an unseren Veröffentlichungs-Server durchführen.

Mit dem folgenden Codesnippet werden die JSON-Inhalte des adaptiven Formulars **contactus** abgerufen.

```javascript
const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/L2NvbnRlbnQvZm9ybXMvYWYvZmlyc3RoZWFkbGVzcw==');
        // Get the form id manually using the listform api
        let formJSON = await resp.json();
        console.log("The contact form json is "+formJSON);
        setForm(formJSON.afModelDefinition)
      }
```

Die vollständige Funktionskomponente für Kontakte ist unten angegeben.

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Panel from './components/Panel';
import { useState,useEffect } from "react";
import { AdaptiveForm } from "@aemforms/af-react-renderer";

export default function Contact(){
   const [selectedForm, setForm] = useState("");
   const extendMappings = {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
      };
    const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/dor/L2NvbnRlbnQvZm9ybXMvYWYvcmlzaGk=');
        let formJSON = await resp.json();
        setForm(formJSON.afModelDefinition)
      
      }
      useEffect( ()=>{
        getForm();
        

    },[]);
    return(
        
        <div>
            <h1>Get in touch with us!!!!</h1>
            <AdaptiveForm mappings={extendMappings} formJson={selectedForm} />
      
          
        </div>
    )
}
```

Der obige Code verwendet native HTML-Komponenten, die den im adaptiven Formular verwendeten Komponenten zugeordnet sind. Beispielsweise ordnen wir die text-input-Komponente des adaptiven Formulars der TextField-Komponente zu. Die im Artikel verwendeten nativen Komponenten können [hier](./assets/native-components.zip) heruntergeladen werden.

## Nächste Schritte

[Auswählen eines Formulars aus der Dropdown-Liste](./select-form-from-drop-down-list.md)

---
title: Rufen Sie die JSON des einzubettenden adaptiven Formulars ab.
description: Verwenden Sie die API zum Abrufen der JSON-Datei des adaptiven Formulars
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
exl-id: ee534724-54ea-48e1-8c92-de1c56a928d4
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 1%

---

# JSON des Formulars abrufen

Melden Sie sich bei Ihrer AEM Forms-Autoreninstanz an und erstellen Sie mit der **Leer mit Kernkomponenten** Vorlage. Veröffentlichen Sie Ihr Formular in Ihrer Veröffentlichungsinstanz.

Um das Formular einzubetten, rufen wir zunächst die JSON-Datei des adaptiven Formulars ab, indem wir einen GET-Aufruf an unseren Veröffentlichungs-Server durchführen.

Das folgende Codefragment ruft die JSON-Datei des adaptiven Formulars mit dem Namen ab **contactus**

```javascript
const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/L2NvbnRlbnQvZm9ybXMvYWYvZmlyc3RoZWFkbGVzcw==');
        // Get the form id manually using the listform api
        let formJSON = await resp.json();
        console.log("The contact form json is "+formJSON);
        setForm(formJSON.afModelDefinition)
      }
```

Der vollständige Code der Komponente Kontaktfunktion ist unten angegeben.

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

Der obige Code verwendet native HTML-Komponenten, die den im adaptiven Formular verwendeten Komponenten zugeordnet sind. Beispielsweise ordnen wir die Texteingabe-Komponente des adaptiven Formulars der TextField-Komponente zu. Die im Artikel verwendeten nativen Komponenten [können Sie hier herunterladen](./assets/native-components.zip)

## Nächste Schritte

[Wählen Sie ein Formular aus der Dropdownliste aus](./select-form-from-drop-down-list.md)

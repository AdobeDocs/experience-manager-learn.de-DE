---
title: Klicken auf eine Karte, um das Formular anzuzeigen
description: Durchführen eines Formular-Drilldowns über die Kartenansicht
feature: Adaptive Forms
version: 6.5
jira: KT-13372
topic: Development
role: User
level: Intermediate
exl-id: c8684cd9-b9c5-4b5b-b990-27c5700cea9f
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '61'
ht-degree: 100%

---

# Anzeigen des Formulars beim Klicken auf eine Karte

Der folgende Code wurde verwendet, damit das Formular angezeigt wird, wenn Benutzende auf eine Karte klicken. Der Pfad des anzuzeigenden Formulars wird mithilfe der Funktion „useParams“ aus der URL extrahiert.

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Panel from './components/Panel';
import { useState,useEffect } from "react";
import {Link, useParams} from 'react-router-dom';
import { AdaptiveForm } from "@aemforms/af-react-renderer";
export default function DisplayForm()
{
   const [selectedForm, setForm] = useState("");
   const params = useParams();
   const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };
    
    
   const getAFForm = async () =>
    {
           
        const resp = await fetch(`/adobe/forms/af/${params.formID}`);
        let formJSON = await resp.json();
        console.log("The contact form json is "+formJSON);
        setForm(formJSON.afModelDefinition)
    }
    
    useEffect( ()=>{
        getAFForm()
        

    },[]);
    return(
       <div>
           <AdaptiveForm mappings={extendMappings} formJson={selectedForm}/>
        </div>
    )
}
```

## Nächste Schritte

[Anzeigen einer Dankesnachricht bei Formularübermittlung](./display-thank-you-message.md)

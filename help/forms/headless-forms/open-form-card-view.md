---
title: Auf Karte klicken, um das Formular anzuzeigen
description: Drilldown des Formulars aus der Kartenansicht
feature: Adaptive Forms
version: 6.5
kt: 13372
topic: Development
role: User
level: Intermediate
source-git-commit: 3bbf80d5c301953b3a34ef8256702ac7445c40da
workflow-type: tm+mt
source-wordcount: '93'
ht-degree: 0%

---

# Formular bei Klick auf die Karte anzeigen

Der folgende Code wurde verwendet, um das Formular anzuzeigen, wenn der Benutzer auf eine Karte klickt. Der Pfad des anzuzeigenden Formulars wird mithilfe der Funktion useParams aus der URL extrahiert.

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
   const pathParam = params["*"];
   const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };
    
    // Add the leading '/' back on 
   const path = '/' + pathParam;
   const getAFForm = async () =>
    {
           
        const resp = await fetch(`${path}/jcr:content/guideContainer.model.json`);
        let formJSON = await resp.json();
        console.log("The contact form json is "+formJSON);
        setForm(formJSON)
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

Im obigen Code wird der Pfad des Formulars, das wiedergegeben werden soll, aus der URL extrahiert und im Abruf verwendet, um die JSON des Formulars abzurufen. Die abgerufene JSON befindet sich dann im folgenden Code

```javascript
        <div>
           <AdaptiveForm mappings={extendMappings} formJson={selectedForm}/>
        </div>
```

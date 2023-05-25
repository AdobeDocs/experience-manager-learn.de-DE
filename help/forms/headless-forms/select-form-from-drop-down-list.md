---
title: Wählen Sie ein Formular aus einer Liste verfügbarer Formulare aus
description: Verwenden der listforms-API zum Ausfüllen der Dropdown-Liste
feature: Adaptive Forms
version: 6.5
kt: 13346
topic: Development
role: User
level: Intermediate
source-git-commit: 6aa3dff44a7e6f1f8ac896e30319958d84ecf57f
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---


# Wählen Sie ein auszufüllendes Formular aus einer Dropdown-Liste aus

Dropdown-Listen bieten eine kompakte und organisierte Möglichkeit, Benutzern eine Liste von Optionen anzuzeigen. Die Elemente in der Dropdown-Liste werden mit den Ergebnissen von [listforms-API](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms)

![Kartenansicht](./assets/forms-drop-down.png)

## Dropdown-Liste

Der folgende Code wurde verwendet, um die Dropdown-Liste mit den Ergebnissen des listforms-API-Aufrufs zu füllen. Basierend auf der Benutzerauswahl wird das adaptive Formular angezeigt, das der Benutzer ausfüllen und senden kann. [Komponenten der Benutzeroberfläche](https://mui.com/) wurden bei der Erstellung dieser Benutzeroberfläche verwendet

```javascript
import * as React from 'react';
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Box from '@mui/material/Box';
import InputLabel from '@mui/material/InputLabel';
import MenuItem from '@mui/material/MenuItem';
import FormControl from '@mui/material/FormControl';
import Select, { SelectChangeEvent } from '@mui/material/Select';
import { AdaptiveForm } from "@aemforms/af-react-renderer";

import { useState,useEffect } from "react";
export default function SelectFormFromDropDownList()
 {
    const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };

const[formPath, setFormPath] = useState('');
const[afForms,SetOptions] = useState([]);
const [selectedForm, setForm] = useState('');
const HandleChange = (event) =>
     {
        console.log("The path is "+event.target.value) 
    
        setFormPath(event.target.value)
        console.log("The formPath"+ formPath);
     };
const getForm = async () =>
     {
        const resp = await fetch(`${formPath}/jcr:content/guideContainer.model.json`);
        let formJSON = await resp.json();
        console.log(formJSON);
        setForm(formJSON);
     }
const getAFForms =async()=>
     {
        const response = await fetch("/adobe/forms/af/listforms")
        //let myresp = await response.status;
        let myForms = await response.json();
        console.log("Got response"+myForms.items[0].title);
        console.log(myForms.items)
        
        //setFormID('test');
        SetOptions(myForms.items)

        
     }
     useEffect( ()=>{
        getAFForms()
        

    },[]);
    useEffect( ()=>{
        getForm()
        

    },[formPath]);

  return (
    <Box sx={{ minWidth: 120 }}>
      <FormControl fullWidth>
        <InputLabel id="demo-simple-select-label">Please select the form</InputLabel>
        <Select
          labelId="demo-simple-select-label"
          id="demo-simple-select"
          value={formPath}
          label="Please select a form"
          onChange={HandleChange}
          
        >
       {afForms.map((afForm,index) => (
    
        
          <MenuItem  key={index} value={afForm.path}>{afForm.title}</MenuItem>
        ))}
        
       
        </Select>
      </FormControl>
      <div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
    </Box>
    

  );
  

}
```

Die beiden folgenden API-Aufrufe wurden bei der Erstellung dieser Benutzeroberfläche verwendet

* [ListForm](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms). Der Aufruf zum Abrufen der Formulare erfolgt nur einmal, wenn die Komponente wiedergegeben wird. Die Ergebnisse des API-Aufrufs werden in der Variablen afForms gespeichert.
Im obigen Code durchlaufen wir afForms mithilfe der Zuordnungsfunktion. Für jedes Element im afForms-Array wird eine MenuItem-Komponente erstellt und der Select-Komponente hinzugefügt.

* Formular abrufen - Ein get -Aufruf wird an den folgenden Endpunkt gesendet, wobei formPath der Pfad zum ausgewählten adaptiven Formular ist, den der Benutzer in der Dropdown-Liste verwendet. Das Ergebnis dieses GET-Aufrufs wird in selectedForm gespeichert.

```
${formPath}/jcr:content/guideContainer.model.json`
```

* Anzeigen des ausgewählten Formulars Der folgende Code wurde zur Anzeige des ausgewählten Formulars verwendet. Das AdaptiveForm-Element wird im npm-Paket aemforms/af-response-renderer bereitgestellt und erwartet die Zuordnungen und die formJson als Eigenschaften

```
<div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
```

## Nächste Schritte

[Formulare im Kartenlayout anzeigen](./display-forms-card-view.md)




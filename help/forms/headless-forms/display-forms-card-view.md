---
title: Anzeigen der abgerufenen Formulare in der Kartenansicht
description: Verwenden der listforms-API zum Anzeigen der Formulare
feature: Adaptive Forms
version: 6.5
kt: 13311
topic: Development
role: User
level: Intermediate
source-git-commit: 6aa3dff44a7e6f1f8ac896e30319958d84ecf57f
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 0%

---


# Formulare im Kartenformat abrufen und anzeigen

Das Kartenansichtsformat ist ein Designmuster, das Informationen oder Daten in Form von Karten darstellt. Jede Karte stellt einen diskreten Inhalt oder Dateneintrag dar und besteht normalerweise aus einem visuell getrennten Container mit spezifischen Elementen, die darin angeordnet sind. In diesem Artikel verwenden wir die [listforms-API](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms) , um die Formulare abzurufen und die Formulare im Kartenformat anzuzeigen, wie unten dargestellt

![Kartenansicht](./assets/card-view-forms.png)

## Kartenvorlage

Der folgende Code wurde zum Entwerfen der Kartenvorlage verwendet. Die Kartenvorlage zeigt den Titel und die Beschreibung des adaptiven Formulars zusammen mit dem Adobe-Logo an. [Komponenten der Benutzeroberfläche](https://mui.com/) wurden bei der Erstellung dieses Layouts verwendet.

```javascript
import Paper from "@mui/material/Paper";
import Grid from "@mui/material/Grid";
import Container from "@mui/material/Container";
import { Typography } from "@mui/material";
import { Box } from "@mui/system";
const FormCard =({headlessForm}) => {
    return (
              <Grid item xs={3}>
                <Paper elevation={3}>
                    <img src="/content/dam/formsanddocuments/registrationform/jcr:content/renditions/cq5dam.thumbnail.48.48.png" className="img"/>
                    <Box padding={3}>
                    <Typography variant="subtititle2" component="h2">
                        {headlessForm.title}
                    
                    </Typography>
                    <Typography variant="subtititle3" component="h4">
                        {headlessForm.description}
                    
                    </Typography>
                    </Box>
                </Paper>
                </Grid>
          


    );
    

};
export default FormCard;
```

## Formulare abrufen

Die listforms-API wurde zum Abrufen der Formulare vom AEM-Server verwendet. Die API gibt ein Array von JSON-Objekten zurück, wobei jedes JSON-Objekt ein Formular darstellt.

```javascript
import { useState,useEffect } from "react";
import React, { Component } from "react";
import FormCard from "./components/FormCard";
import Grid from "@mui/material/Grid";
import Paper from "@mui/material/Paper";
import Container from "@mui/material/Container";
 
export default function ListForm(){
    const [fetchedForms,SetHeadlessForms] = useState([])
    const getForms=async()=>{
        const response = fetch("/adobe/forms/af/listforms")
        let headlessForms = await (await response).json();
        console.log(headlessForms.items);
        SetHeadlessForms(headlessForms.items);
    }
    useEffect( ()=>{
        getForms()
        

    },[]);
    return(
        <div>
             <div>
                <Container>
                   <Grid container spacing={3}>
                       {
                            fetchedForms.map( (afForm,index) =>
                                <FormCard headlessForm={afForm} key={index}/>
                         
                            )
                        }
                    </Grid>
                </Container>
             </div>

        </div>
    )
}
```

Im obigen Code navigieren wir mithilfe der Zuordnungsfunktion durch die abgerufenen Formulare. Für jedes Element im fetchedForms-Array wird eine FormCard-Komponente erstellt und zum Grid-Container hinzugefügt. Sie können jetzt die ListForm-Komponente in Ihrer React-App gemäß Ihren Anforderungen verwenden.

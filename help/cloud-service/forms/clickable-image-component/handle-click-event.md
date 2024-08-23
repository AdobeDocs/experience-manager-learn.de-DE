---
title: Erstellen einer klickbaren Bildkomponente
description: Erstellen einer klickbaren Bildkomponente im AEM Forms-Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: 426020f59c7103829b7b7b74acb0ddb7159b39fa
workflow-type: tm+mt
source-wordcount: '83'
ht-degree: 7%

---

# Handle-Klick-Ereignis

Erstellen Sie eine Client-Bibliothek und verknüpfen Sie die Client-Bibliothek mit der Komponente.

Fügen Sie in Ihrer JavaScript-Datei der Client-Bibliothek den folgenden Code hinzu, der zum Klick-Ereignis verarbeitet werden soll.
Je nach ausgewähltem Status können die entsprechenden vom Endpunkt zurückgegebenen Daten angezeigt werden. Die Details des Endpunkts und die anzuzeigenden Daten hängen von Ihrem Anwendungsfall ab.



```javascript
document.addEventListener("DOMContentLoaded", function(event)
  {
    const apiUrl = 'API end point to return data based on the state selected';
       // Select all <path> elements
    const paths = document.querySelectorAll('path');
    const tooltip = document.getElementById('tooltip');
    const svg = document.getElementById('svg');
    const states = document.querySelectorAll('path');
    // Loop through each <path> element and add an event listener
    states.forEach(state =>
     {
                     state.addEventListener('click', function(event)
                  {
                     alert('path clicked:'+ this.id);
                    fetch(apiUrl+this.id)
                    .then(response =>
                        {
                            // Check if the response is ok (status code in the range 200-299)
                            if (!response.ok)
                            {
                                  throw new Error('Network response was not ok ' + response.statusText);
                            }
                            // Parse the JSON from the response
                            console.log(response);
                            return response.text();
                          })
                        .then(data => {
                                // Handle the data
                              console.log(data);
                            })
                        .catch(error => {
                            // Handle any errors
                            console.error('There was a problem with the fetch operation:', error);
                            });
                     });
        });

});
```

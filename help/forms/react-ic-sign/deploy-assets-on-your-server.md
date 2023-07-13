---
title: Stellen Sie die Beispiel-Assets auf Ihrem Server bereit
description: Nutzungsszenario auf Ihrem lokalen Server
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 44f4261b-d6fe-42ad-a3aa-2a36ca897b5e
source-git-commit: cc24ebca488ea286e8a4605edfb39420c1c10022
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---

# Bereitstellen der Assets

Die folgenden Assets/Konfigurationen wurden auf einem AEM Forms-Veröffentlichungsserver bereitgestellt.

* [Adobe Sign Wrapper Bundle](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [Beispielvorlage für interaktive Kommunikation](assets/waiver-interactive-communication.zip)
* [Bereitstellen des DevelopingWithServiceUser-Bundles](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)
* Fügen Sie den folgenden Eintrag im Apache Sling Service User Mapper Service mithilfe des OSGi configMgr hinzu.
  **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**

## Bereitstellen der Beispiel-React-App

* [Beispielanwendung für Reaktionsmaßnahmen herunterladen](assets/mult-step-form1.zip)
* Entpacken Sie den Inhalt der React-App in einen neuen Ordner.
* Navigieren Sie zum Ordner und führen Sie die folgenden Befehle aus

```java
npm install
npm start
```

Öffnen Sie die Datei EmergencyContact.js und ändern Sie die URL in der Abrufmethode entsprechend Ihrer Umgebung.


```javascript
 const getWebForm=async()=>
     {
        setSpinner(true)
        console.log("inside widgetURL function emergency contact");
        // NOTE: replace the `aemforms.azure.com:4503` with your AEM FORM server
        let res = await fetch("http://aemforms.azure.com:4503/bin/getwidgeturl",
          {
            method: "POST",
            body: JSON.stringify({"icTemplate":"/content/forms/af/waiver/waiver/channels/print","waiver":formData})
                     
         })
 
```

Um das Ausführen von POST-Aufrufen an den AEM-Endpunkt über Ihre REACT-App zu aktivieren, müssen Sie die entsprechenden Berechtigungen im Feld &quot;Zulässiger Ursprung&quot;in der Konfiguration der Adobe Granite-Richtlinie für Cross-Origin Resource Sharing angeben.

![cors-setting](assets/cors-settings.png)

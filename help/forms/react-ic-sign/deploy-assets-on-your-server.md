---
title: Bereitstellen der Beispiel-Assets auf Ihrem Server
description: Starten des Anwendungsfalls auf Ihrem lokalen Server
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: f12f83fa-673a-454c-aa52-6ea769a182b7
duration: 44
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 100%

---

# Bereitstellen der Assets

Die folgenden Assets/Konfigurationen wurden auf einem AEM Forms-Veröffentlichungs-Server bereitgestellt.

* [Adobe Sign-Wrapper-Bundle](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [Vorlage für interaktives Kommunikationsbeispiel](assets/waiver-interactive-communication.zip)
* [Bereitstellen des Bundles DevelopingWithServiceUser](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=de)
* Fügen Sie den folgenden Eintrag im Apache Sling Service User Mapper Service mithilfe des OSGi-configMgr hinzu.
  **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**

## Bereitstellung der Beispiel-React-App

* [Laden Sie die Beispiel-React-App herunter](assets/mult-step-form1.zip)
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

Um das Ausführen von POST-Aufrufen an den AEM-Endpunkt über Ihre REACT-App zu aktivieren, müssen Sie die entsprechenden Einträge im Feld „Zulässige Ursprünge“ in der Konfiguration der Adobe Granite für Cross-Origin Resource Sharing Policy angeben.

![cors-setting](assets/cors-settings.png)

Weitere Informationen zu CORS-Konfigurationsoptionen finden Sie unter [Verstehen von CORS mit AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html?lang=de).

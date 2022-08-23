---
title: Push der Konfiguration von Cloud Services und des Formulardatenmodells in die Cloud-Instanz
description: Erstellen und pushen Sie ein adaptives Formular, das auf dem Azure-Datenmodell zur Datenspeicherung basiert, in die Cloud-Instanz.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 9006
exl-id: 77c00a35-43bf-485f-ac12-0fffb307dc16
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# Cloud-Services-Konfiguration in Ihr Projekt aufnehmen

Erstellen Sie einen Konfigurations-Container mit dem Namen &quot;FormTutorial&quot;für Ihre Cloud Services-Konfiguration Erstellen Sie eine Cloud-Service-Konfiguration für Azure Storage namens &quot;FormsCSAndAzureBlob&quot;im &quot;FormTutorial&quot;-Container, indem Sie die Details zum Azure-Speicherkonto und den Azure-Zugriffsschlüssel angeben.

Öffnen Sie Ihr AEM Projekt in IntelliJ. Stellen Sie sicher, dass Sie den Ordner FormTutorial hinzufügen, wie unten im ui.content-Projekt gezeigt.
![cloud-services-configuration](assets/cloud-services-configuration.png)

Stellen Sie sicher, dass Sie den folgenden Eintrag in die Datei &quot;filter.xml&quot;des ui.content-Projekts einfügen

```xml
<filter root="/conf/FormTutorial" mode="replace"/>
```

![filter-xml](assets/ui-content-filter.png)

## Formulardatenmodell in Ihr Projekt einbeziehen

Erstellen Sie ein Formulardatenmodell basierend auf der Cloud Services-Konfiguration, die Sie im vorherigen Schritt erstellt haben. Um das Formulardatenmodell in Ihr Projekt aufzunehmen, erstellen Sie die entsprechende Ordnerstruktur in Ihrem AEM Projekt in intelliJ. Beispielsweise befindet sich mein Formulardatenmodell in einem Ordner namens Registrierungen
![fdm-content](assets/ui-content-fdm.png)

Fügen Sie den entsprechenden Eintrag in die Datei &quot;filter.xml&quot;des ui.content-Projekts ein.

```xml
<filter root="/content/dam/formsanddocuments-fdm/registrations" mode="replace"/>
```


>[!NOTE]
>
>Wenn Sie jetzt Ihr Projekt mit Cloud Manager erstellen und bereitstellen, müssen Sie Ihren Azure-Zugriffsschlüssel in die Cloud-Services-Konfiguration erneut eingeben. Um zu vermeiden, dass der Zugriffsschlüssel erneut eingegeben wird, wird empfohlen, eine kontextbezogene Konfiguration mithilfe der Umgebungsvariablen zu erstellen, wie im Abschnitt [nächster Artikel](./context-aware-fdm.md)

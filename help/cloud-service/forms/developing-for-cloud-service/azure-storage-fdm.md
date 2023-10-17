---
title: Pushen der Cloud Services-Konfiguration und des Formulardatenmodells in die Cloud-Instanz
description: Erstellen Sie ein adaptives Formular, das auf dem Azure-Datenmodell zur Datenspeicherung basiert, und pushen Sie es in die Cloud-Instanz.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
kt: 9006
exl-id: 77c00a35-43bf-485f-ac12-0fffb307dc16
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: ht
source-wordcount: '227'
ht-degree: 100%

---

# Aufnehmen der Cloud-Services-Konfiguration in Ihr Projekt

Erstellen Sie einen Konfigurations-Container mit dem Namen „FormTutorial“ für Ihre Cloud-Service-Konfiguration
Erstellen Sie eine Cloud-Service-Konfiguration für Azure Storage namens „FormsCSAndAzureBlob“ im Container „FormTutorial“, indem Sie die Details zum Azure-Speicherkonto und den Azure-Zugriffsschlüssel angeben.

Öffnen Sie Ihr AEM-Projekt auf IntelliJ. Stellen Sie sicher, dass Sie den Ordner FormTutorial hinzufügen, wie unten im ui.content-Projekt gezeigt.
![cloud-services-configuration](assets/cloud-services-configuration.png)

Stellen Sie sicher, dass Sie den folgenden Eintrag in die Datei filter.xml des ui.content-Projekts einfügen

```xml
<filter root="/conf/FormTutorial" mode="replace"/>
```

![filter-xml](assets/ui-content-filter.png)

## Einbeziehen des Formulardatenmodells in Ihr Projekt

Erstellen Sie ein Formulardatenmodell basierend auf der Cloud-Service-Konfiguration, die Sie im vorherigen Schritt erstellt haben. Um das Formulardatenmodell in Ihr Projekt aufzunehmen, erstellen Sie die entsprechende Ordnerstruktur in Ihrem AEM-Projekt in intelliJ. Beispielsweise befindet sich das Formulardatenmodell in einem Ordner namens „Registrierungen“
![fdm-content](assets/ui-content-fdm.png)

Fügen Sie den entsprechenden Eintrag in die Datei filter.xml des ui.content-Projekts ein.

```xml
<filter root="/content/dam/formsanddocuments-fdm/registrations" mode="replace"/>
```


>[!NOTE]
>
>Wenn Sie jetzt Ihr Projekt mit Cloud Manager erstellen und bereitstellen, müssen Sie Ihren Azure-Zugriffsschlüssel erneut in die Cloud-Service-Konfiguration eingeben. Um die Wiedereingabe des Zugriffsschlüssels zu vermeiden, wird empfohlen, eine kontextsensitive Konfiguration mithilfe der Umgebungsvariablen zu erstellen, wie im [nächsten Artikel](./context-aware-fdm.md) erklärt wird

## Nächste Schritte

[Erstellen einer kontextsensitiven Konfiguration](./context-aware-fdm.md)

---
title: Konfigurieren der Batch-Datenkonfiguration
description: Konfigurieren der Batch-Datenkonfiguration
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9673
exl-id: db25e5a2-e1a8-40ad-af97-35604d515450
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '135'
ht-degree: 100%

---

# Erstellen einer Batch-Konfiguration

Um eine Batch-API zu verwenden, erstellen Sie eine Batch-Konfiguration und führen Sie einen auf dieser Konfiguration basierenden Durchlauf aus. Das folgende Video zeigt eine Demonstration der Erstellung einer Batch-Konfiguration mithilfe der API

>[!NOTE]
>Stellen Sie sicher, dass der oder die AEM-Benutzende zur ```forms-users```-Gruppe gehört, um API-Aufrufe durchzuführen.


>[!VIDEO](https://video.tv.adobe.com/v/340241?quality=12&learn=on)

## Erstellen einer Batch-Konfiguration

Im Folgenden finden Sie den POST-Endpunkt zum Erstellen der Batch-Konfiguration

```xml
<baseURL>/config
```

Im Folgenden finden Sie die Mindestkonfiguration, die bei der Erstellung der Batch-Konfiguration angegeben werden muss. Diese muss als JSON-Objekt im Text der HTTP-Anfrage übergeben werden

```
{
	"configName": "monthlystatements",
	"dataSourceConfigUri": "/conf/batchapi/settings/forms/usc/batch/batchapitutorial",
	"outputTypes": [
		"PDF"
	],
	"template": "crx:///content/dam/formsanddocuments/formtemplates/custom_fonts.xdp"

}
```

## Überprüfen der Batch-Konfiguration

Um zu überprüfen, ob die Batch-Konfiguration erfolgreich erstellt wurde, können Sie einen GET-Anfrageaufruf an den folgenden Endpunkt senden


```xml
<baseURL>/config/monthlystatements
```

Sie müssen nur ein leeres JSON-Objekt im Text der HTTP-Anfrage übergeben.

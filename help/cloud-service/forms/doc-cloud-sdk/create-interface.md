---
title: Benutzeroberfläche "Dienst erstellen"
description: Definieren Sie die Methoden in der Benutzeroberfläche, die Sie verfügbar machen möchten
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Entwicklung
thumbnail: 7825.jpg
kt: 7825
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '24'
ht-degree: 8%

---

# Benutzeroberfläche

Erstellen Sie eine Schnittstelle mit den folgenden 2 Methodendefinitionen.

```java
package com.aemforms.doccloud.core;

import java.io.InputStream;

import com.adobe.aemfd.docmanager.Document;

public interface DocumentCloudSDKService {
	
	public Document getPDF(String location,String accessToken,String fileName);
	public Document createPDFFromInputStream(InputStream is,String fileName);

}


}
```

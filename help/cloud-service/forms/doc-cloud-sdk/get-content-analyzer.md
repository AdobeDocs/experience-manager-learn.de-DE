---
title: Erstellen von Content Analyzer
description: Erstellen Sie den JSON-Teil mit den Informationen zu den Eingabeparametern für den REST-Aufruf.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
thumbnail: 7836.jpg
kt: 7836
exl-id: 548a21b9-5487-4b48-9782-19b537a48f98
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 3%

---

# Erstellen von Analyzer-Anforderungen

Erstellen Sie ein JSON-Fragment, das Folgendes definiert:

+ Eingabe
+ parameters
+ output.

Die Details dieses [Formularparameters finden Sie hier.](https://documentcloud.adobe.com/document-services/index.html#post-createPDF)

Der unten aufgeführte Beispielcode generiert das JSON-Fragment für alle Office 365-Dokumenttypen.

```java
package com.aemforms.doccloud.core.impl;

import com.google.gson.JsonObject;

public class GetContentAnalyser {
	public static String getContentAnalyserRequest(String fileName)
	{
		JsonObject outerWrapper = new JsonObject();
		
		
		JsonObject documentIn = new JsonObject();
		documentIn.addProperty("cpf:location", "InputFile0");

		if(fileName.endsWith(".pptx"))
		{
			documentIn.addProperty("dc:format","application/vnd.openxmlformats-officedocument.presentationml.presentation");
		}

		if(fileName.endsWith(".docx"))
		{
			
			documentIn.addProperty("dc:format","application/vnd.openxmlformats-officedocument.wordprocessingml.document");
		}
		if(fileName.endsWith(".xlsx"))
		{
			documentIn.addProperty("dc:format","application/vnd.openxmlformats-officedocument.spreadsheetml.sheet");
		}
		if(fileName.endsWith(".xml"))
		{
			documentIn.addProperty("dc:format","text/plain");
		}
		if(fileName.endsWith(".jpg"))
		{
			documentIn.addProperty("dc:format","image/jpeg");
		}

		JsonObject cpfinputs = new JsonObject();
		cpfinputs.add("documentIn", documentIn);
		
		
		//documentInWrapper.add("documentIn",documentIn);
		JsonObject cpfengine = new JsonObject();
		cpfengine.addProperty("repo:assetId", "urn:aaid:cpf:Service-1538ece812254acaac2a07799503a430");
		
		JsonObject documentOut = new JsonObject();
		
		documentOut.addProperty("cpf:location", "multipartLabelOut");
		documentOut.addProperty("dc:format", "application/pdf");
		JsonObject documentOutWrapper = new JsonObject();
		documentOutWrapper.add("documentOut",documentOut);
	
		outerWrapper.add("cpf:inputs",cpfinputs);
		outerWrapper.add("cpf:engine", cpfengine);
		outerWrapper.add("cpf:outputs", documentOutWrapper);
		System.out.println("The content Analyser is "+outerWrapper.toString());
		
		return outerWrapper.toString();
		
		
		
		
		
	}

}
```

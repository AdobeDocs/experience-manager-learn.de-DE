---
title: Benutzerdefinierte OSGi-Konfiguration erstellen
description: Benutzerdefinierte OSGi-Konfiguration zur Erfassung von Dokument Cloud-spezifischen Details
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Entwicklung
thumbnail: 7818.jpg
kt: 7818
source-git-commit: 84499d5a7c8adac87196f08c6328e8cb428c0130
workflow-type: tm+mt
source-wordcount: '64'
ht-degree: 3%

---

# Einführung

Erstellen Sie eine benutzerdefinierte OSGi-Konfiguration, um die Anmeldeinformationen Ihres Dokument Cloud-Kontos zu erfassen.


Um eine benutzerdefinierte OSGi-Konfiguration zu erstellen, müssen wir zunächst eine Schnittstelle erstellen, deren öffentliche Methoden die Felder in der Konfiguration darstellen.

![doc-cloud-config](assets/doc-cloud-configuration.JPG)


Erstellen Sie eine Schnittstelle mit dem Namen DocumentCloudConfiguration und fügen Sie den folgenden Code darin ein.

```java
package com.aemforms.doccloud.core;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name="Document Cloud Service Configuration", description = "Connect AEM Forms With Document Cloud")
public @interface DocumentCloudConfiguration {
	  @AttributeDefinition(name="Client ID", description="Client ID")
	  String getClientID() default "";
	  
	  @AttributeDefinition(name="Client Secret", description="Client Secret")
	  public String getClientSecret() default "26dc4de1-f7f0-46b6-9e8e-86270ad34f58";
	  
	  @AttributeDefinition(name="Technical Account ID", description="Technical Account ID")
	  public String getTechnicalAccount() default "42FF05A7606F71BB0A495FBE@techacct.adobe.com";

	  @AttributeDefinition(name="Organization ID", description="Organization ID")
	  String getOrganizationID() default "299805FF5FC9199D0A495EBC@AdobeOrg";
	  
	  @AttributeDefinition(name="Metascope", description="Metascope")
	  String getMetascope() default "ent_documentcloud_sdk";


}
```

---
title: Grundlagen zur Entwicklung von OSGi-Diensten
description: Erfahren Sie mehr über die Grundlagen zur Entwicklung eines OSGi-Dienstes.
role: Developer
level: Beginner
topic: Development
feature: OSGI
jira: KT-8227
thumbnail: 335476.jpeg
last-substantial-update: 2022-09-16T00:00:00Z
exl-id: a3a9bf59-e9a2-4322-ac93-9c12c70b9a75
duration: 505
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '122'
ht-degree: 100%

---

# OSGi-Dienste

Lernen Sie die Grundlagen zur Entwicklung von OSGi-Diensten kennen, darunter:

+ Konvertieren eines Java POJO in einen OSGi-Dienst
+ Binden eines OSGi-Dienstes an eine Java-Oberfläche

>[!VIDEO](https://video.tv.adobe.com/v/335476?quality=12&learn=on)

## Ressourcen

+ [@Component JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/service/component/annotations/Component.html)
+ [@ProviderType JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/annotation/versioning/ProviderType.html)
+ [@Version JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/annotation/versioning/Version.html)

## Code

### Activities.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/Activities.java`

```java
package com.adobe.aem.wknd.examples.core.adventures;

import org.osgi.annotation.versioning.ProviderType;

@ProviderType
public interface Activities {    
    String getRandomActivity();
}
```

### ActivitiesImpl.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/impl/ActivitiesImpl.java`

```java
package com.adobe.aem.wknd.examples.core.adventures.impl;

import java.util.Random;

import com.adobe.aem.wknd.examples.core.adventures.Activities;

import org.osgi.service.component.annotations.Component;

@Component(
    service = { Activities.class }
)
public class ActivitiesImpl implements Activities {

    private static final String[] ACTIVITIES = new String[] { 
        "Camping", "Skiing",  "Skateboarding"
    };

    //private final int randomIndex = new Random().nextInt(ACTIVITIES.length);
    private final Random random = new Random();

    /**
     * @return the name of a random WKND adventure activity
     */
    public String getRandomActivity() {
        int randomIndex = random.nextInt(ACTIVITIES.length);
        return ACTIVITIES[randomIndex];
    }    
}
```

### package-info.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/package-info.java`

```java
@Version("1.0")
package com.adobe.aem.wknd.examples.core.adventures;

import org.osgi.annotation.versioning.Version;
```

Das Hinzufügen einer `package-info.java` ist erforderlich, um sicherzustellen, dass andere OSGi-Bundles in AEM die OSGi-Dienstschnittstelle (oder jede Java-Klasse) auflösen können. Wenn die `package-info.java` fehlt, werden das Java-Paket und seine Java-Schnittstellen oder -Klassen nicht exportiert. Andere OSGi-Bundles, die versuchen, diese Java-Schnittstellen oder -Klassen aus diesem Java-Paket zu importieren, erhalten in der OSGi-Bundle-Konsole von AEM die Fehlermeldung __Kann nicht aufgelöst werden__.

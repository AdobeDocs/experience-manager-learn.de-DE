---
title: Benutzerdefinierte Namespaces
description: Erfahren Sie, wie Sie benutzerdefinierte Namespaces definieren und für AEM as a Cloud Service bereitstellen.
version: Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
kt: 11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
source-git-commit: 79804ac1ec18f8ac4815bd5ee6f309ef85110cb9
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 6%

---

# Benutzerdefinierte Namespaces

Erfahren Sie, wie Sie benutzerdefinierte [Namespaces](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html) auf as a Cloud Service AEM.

Benutzerdefinierte Namespaces sind der optionale Teil einer JCR-Eigenschaft, der einer `:`. AEM verwendet mehrere Namespaces, z. B.:

+ `jcr` für JCR-Systemeigenschaften
+ `cq` für AEM (ehemals &quot;Adobe CQ&quot;)-Eigenschaften
+ `dam` für AEM Eigenschaften, die für DAM-Assets spezifisch sind
+ `dc` für Dublin Core Properties

... und viele andere.

Namespaces können verwendet werden, um den Umfang und den Zweck einer Eigenschaft zu kennzeichnen. Durch die Erstellung eines benutzerdefinierten Namespace, häufig Ihres Unternehmensnamens, können Knoten oder Eigenschaften, die für Ihre AEM-Implementierung spezifisch sind, eindeutig identifiziert werden und unternehmensspezifische Daten enthalten.

Benutzerdefinierte Namespaces werden in [Initialisierung des Sling-Repositorys (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html) Skripten und stellt für AEM bereit, die als OSGi-Konfigurationen as a Cloud Service sind - und zu Ihren [AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=de) `ui.config` Projekt.

>[!VIDEO](https://video.tv.adobe.com/v/3412319/?quality=12&learn=on)

## Ressourcen

+ [Dokumentation zur Initialisierung des Sling-Repositorys (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## Code

Der folgende Code wird zum Konfigurieren eines `wknd` Namespace.

### OSGi-Konfiguration von RepositoryInitializer

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

Dies ermöglicht benutzerdefinierte Eigenschaften mithilfe der `wknd` Namespace, wie als erster Parameter nach dem `register namespace` Anweisung, die in AEM verwendet werden soll. Erweiterte Skriptdefinitionen finden Sie in den Beispielen im Abschnitt [Dokumentation zur Initialisierung des Sling-Repositorys (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).

---
title: Verwenden AEM Modernisierungs-Tools für den Umstieg auf AEM as a Cloud Service
description: Erfahren Sie, wie AEM Modernisierungs-Tools verwendet werden, um ein vorhandenes AEM Projekt und einen vorhandenen Inhalt zu aktualisieren und so AEM as a Cloud Service kompatibel zu sein.
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
source-git-commit: 3657e7798774f9cc673ff6ccd8af1a555b1d4013
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 9%

---


# AEM-Modernisierungs-Tools

Erfahren Sie, wie AEM Modernisierungs-Tools verwendet werden, um einen vorhandenen AEM Sites-Inhalt zu aktualisieren, damit er AEM as a Cloud Service kompatibel ist und mit Best Practices übereinstimmt.

>[!VIDEO](https://video.tv.adobe.com/v/336965/?quality=12&learn=on)

## Verwenden AEM Modernisierungs-Tools

![Lebenszyklus AEM Moderationstools](./assets/aem-modernization-tools.png)

AEM Modernisierungs-Tools konvertieren automatisch vorhandene AEM Seiten, die aus älteren statischen Vorlagen, Foundation-Komponenten und ParSys bestehen, um moderne Ansätze wie bearbeitbare Vorlagen, AEM WCM-Kernkomponenten und Layout-Container zu verwenden.

### Wichtigste Aktivitäten

+ Klonen Sie AEM 6.x-Produktion, um AEM Modernisierungs-Tools für auszuführen.
+ Laden Sie die [Die neuesten AEM-Modernisierungs-Tools](https://github.com/adobe/aem-modernize-tools/releases/latest) über den AEM 6.x-Produktionsklon über Package Manager

+ [Seitenstrukturkonverter](https://opensource.adobe.com/aem-modernize-tools/pages/tools/page-structure.html) Aktualisiert vorhandenen Seiteninhalt von einer statischen Vorlage in eine zugeordnete bearbeitbare Vorlage mithilfe von Layout-Containern.
   + Definieren von Konversionsregeln mithilfe der OSGi-Konfiguration
   + Seitenstrukturkonverter für vorhandene Seiten ausführen

+ [Komponenten-Konverter](https://opensource.adobe.com/aem-modernize-tools/pages/tools/component.html) Aktualisiert vorhandenen Seiteninhalt von einer statischen Vorlage in eine zugeordnete bearbeitbare Vorlage mithilfe von Layout-Containern.
   + Definieren von Konvertierungsregeln über JCR-Knotendefinitionen/XML
   + Ausführen des Komponenten Converter-Tools für vorhandene Seiten

+ [Policy Importer](https://opensource.adobe.com/aem-modernize-tools/pages/tools/policy-importer.html) erstellt Richtlinien aus der Designkonfiguration
   + Konvertierungsregeln mithilfe von JCR-Knotendefinitionen/XML definieren
   + Richtlinien-Importer mit vorhandenen Designdefinitionen ausführen
   + Anwenden importierter Richtlinien auf AEM Komponenten und Container

+ [Dialog Converter](https://opensource.adobe.com/aem-modernize-tools/pages/tools/dialog.html) konvertiert die Komponentendialogfelder Classic (ExtJS) und CoralUI 2 in TouchUI 3-basierte Dialogfelder von CoralUI 3.
   + Führen Sie das Dialog Converter-Tool für vorhandene benutzeroberflächenbasierte Dialogfelder von ExtJS oder Coral2 aus.
   + Konvertierte Dialogfelder wieder in Git-Repository synchronisieren

### Sonstige -Ressourcen

+ [Herunterladen AEM Modernisierungs-Tools](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [Dokumentation zu AEM Modernisierungs-Tools](https://opensource.adobe.com/aem-modernize-tools/)
+ [AEM Gems - Einführung in die AEM-Modernisierungs-Suite](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)

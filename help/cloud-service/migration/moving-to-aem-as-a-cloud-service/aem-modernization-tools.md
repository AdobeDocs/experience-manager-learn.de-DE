---
title: Verwenden von AEM-Modernisierungs-Tools für den Wechsel zu AEM as a Cloud Service
description: Erfahren Sie, wie AEM-Modernisierungs-Tools verwendet werden, um ein vorhandenes AEM-Projekt und einen vorhandenen Inhalt zu aktualisieren, um so mit AEM as a Cloud Service kompatibel zu sein.
version: Cloud Service
topic: Migration, Upgrade
feature: Migration
role: Developer
level: Experienced
jira: KT-8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
duration: 2527
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '385'
ht-degree: 100%

---


# AEM-Modernisierungs-Tools

Erfahren Sie, wie AEM-Modernisierungs-Tools verwendet werden, um einen vorhandenen AEM Sites-Inhalt zu aktualisieren, damit er mit AEM as a Cloud Service kompatibel ist und den Best Practices entspricht.

## All-in-One-Converter

>[!VIDEO](https://video.tv.adobe.com/v/338802?quality=12&learn=on)

## Seitenkonvertierung

>[!VIDEO](https://video.tv.adobe.com/v/338799?quality=12&learn=on)

## Komponentenkonvertierung

>[!VIDEO](https://video.tv.adobe.com/v/338788?quality=12&learn=on)

## Richtlinienimport

>[!VIDEO](https://video.tv.adobe.com/v/338797?quality=12&learn=on)

## Verwenden von AEM-Modernisierungs-Tools

![Lebenszyklus von AEM-Modernisierungs-Tools](./assets/aem-modernization-tools.png)

AEM-Modernisierungs-Tools konvertieren automatisch vorhandene AEM-Seiten, die aus älteren statischen Vorlagen, Foundation-Komponenten und Absatzsystemen bestehen, um moderne Ansätze wie bearbeitbare Vorlagen, AEM WCM-Kernkomponenten und Layout-Container zu verwenden.

## Wichtigste Aktivitäten

+ Klonen Sie die AEM 6.x-Produktion, um AEM-Modernisierungs-Tools damit auszuführen.
+ Laden Sie die [neuesten AEM-Modernisierungs-Tools](https://github.com/adobe/aem-modernize-tools/releases/latest) mithilfe von Package Manager herunter und installieren Sie sie für den Klon der AEM 6.x-Produktion.

+ Der [Seitenstruktur-Converter](https://opensource.adobe.com/aem-modernize-tools/pages/structure/about.html) aktualisiert vorhandene Seiteninhalte von einer statischen Vorlage mithilfe von Layout-Containern in eine zugeordnete bearbeitbare Vorlage.
   + Definieren von Konversionsregeln mithilfe der OSGi-Konfiguration
   + Ausführen des Seitenstrukturkonverters für vorhandene Seiten

+ Der [Komponenten-Converter](https://opensource.adobe.com/aem-modernize-tools/pages/component/about.html) aktualisiert vorhandenen Seiteninhalt von einer statischen Vorlage mithilfe von Layout-Containern in eine zugeordnete bearbeitbare Vorlage.
   + Definieren von Konvertierungsregeln über JCR-Knotendefinitionen/XML
   + Ausführen des Komponenten-Converter-Tools für vorhandene Seiten

+ Das [Richtlinien-Import-Tool](https://opensource.adobe.com/aem-modernize-tools/pages/policy/about.html) erstellt Richtlinien aus der Designkonfiguration.
   + Definieren von Konvertierungsregeln mithilfe von JCR-Knotendefinitionen/XML
   + Ausführen des Richtlinien-Import-Tools mit vorhandenen Design-Definitionen
   + Anwenden importierter Richtlinien auf AEM-Komponenten und -Container

## Praktische Übung

Wenden Sie Ihr Wissen an, indem Sie ausprobieren, was Sie mit dieser praktischen Übung gelernt haben.

Vergewissern Sie sich, dass Sie das obige Video und die folgenden Materialien gesehen und verstanden haben, bevor Sie die praktische Übung durchführen:

+ [Anders denken über AEM as a Cloud Service](./introduction.md)
+ [Repository-Modernisierung](./repository-modernization.md)
+ [Veränderliche und unveränderliche Inhalte](../../developing/basics/mutable-immutable.md)
+ [Struktur von AEM-Projekten](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html?lang=de)

Stellen Sie außerdem sicher, dass Sie die vorherige praktische Übung abgeschlossen haben:

+ [Praktisches Training zu BPA und CAM](./bpa-and-cam.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology"><img alt="Praktische GitHub-Repository-Übung" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Übung zur AEM-Modernisierung</div>
            <p style="margin:1rem 0">
                Erfahren Sie, wie Sie mit AEM-Modernisierungs-Tools eine alte WKND-Site aktualisieren können, um den Best Practices für AEM as a Cloud Service zu entsprechen.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ausprobieren der AEM-Modernisierungs-Tools.</span>
</a>
        </td>
    </tr>
</table>

## Sonstige Ressourcen

+ [Herunterladen der AEM-Modernisierungs-Tools](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [Dokumentation zu den AEM-Modernisierungs-Tools](https://opensource.adobe.com/aem-modernize-tools/)
+ [AEM GEMs – Einführung in die AEM-Modernisierungssuite](https://helpx.adobe.com/de/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)

1. Stellen Sie die modernisierte wknd-legacy-Site im lokalen AEM SDK bereit. AEM ASK kann hier heruntergeladen werden:
   + [Software Distribution-Portal](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html).

---
title: Farb-Management mit AEM Dynamic Media
description: In diesem Video erforschen wir Dynamic Media Color Management und wie es verwendet werden kann, um Funktionen zur Vorschau von Farbkorrekturen in für AEM Assets bereitzustellen.
sub-product: dynamic-media
feature: Profile, Video-Profile
version: 6.3, 6.4, 6.5
topic: Content Management
role: Entwickler
level: Zwischenschaltung
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 24%

---


# Farb-Management mit AEM Dynamic Media{#understanding-color-management-with-aem-dynamic-media}

In diesem Video erforschen wir Dynamic Media Color Management und wie es verwendet werden kann, um Funktionen zur Vorschau von Farbkorrekturen in für AEM Assets bereitzustellen.

>[!VIDEO](https://video.tv.adobe.com/v/16792/?quality=9&learn=on)

>[!NOTE]
>
>[Aktivieren Sie die AEM für dynamische ](https://docs.adobe.com/docs/en/aem/6-0/administer/integration/dynamic-media/enabling-dynamic-media.html) Medien, um diese Funktion zu verwenden.

Diese Funktion ist für AEM 6.1- und 6.2-Versionen als Feature Pack verfügbar.

## XML-Vorlage für den Konfigurationsknoten Farbmanagement {#xml-template-for-the-color-management-configuration-node}

Im Folgenden finden Sie die XML-Vorlage für den Konfigurationsknoten &quot;Farbmanagement&quot;. Diese XML-Vorlage kann in das AEM Entwicklungsprojekt kopiert und mit den projektspezifischen Konfigurationen konfiguriert werden.

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!--
    XML Node definition for: /etc/dam/imageserver/configuration/jcr:content/settings

 Adobe Docs

 * Image Server Configuration: https://docs.adobe.com/docs/en/aem/6-2/administer/content/dynamic-media/config-dynamic.html#Configuring%20Dynamic%20Media%20Image%20Settings

* Default Color Profile Configuration: https://docs.adobe.com/docs/en/aem/6-1/administer/content/dynamic-media/config-dynamic.html#Configuring%20the%20default%20color%20profiles

    iccprofileXXX values:
        Node name of color profile found at: /etc/dam/imageserver/profiles

    iccblackpointcompensation values:
        true | false

    iccdither values:
        true | false

    iccrenderintent values:
        0 for perceptual
        1 for relative colorimetric
        2 for saturation
        3 for absolute colorimetric

-->

<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
    xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:primaryType="nt:unstructured"

        bkgcolor="FFFFFF"
        defaultpix="300,300"
        defaultthumbpix="100,100"
        expiration="{Long}36000000"
        jpegquality="80"
        maxpix="2000,2000"
        resmode="SHARP2"
        resolution="72"
        thumbnailtime="[1%,11%,21%,31%,41%,51%,61%,71%,81%,91%]"
        iccprofilergb=""
        iccprofilecmyk=""
        iccprofilegray=""
        iccprofilesrcrgb=""
        iccprofilesrccmyk=""
        iccprofilesrcgray=""
        iccblackpointcompensation="{Boolean}true"
        iccdither="{Boolean}false"
        iccrenderintent="{Long}0"
/>
```

### Die Liste der standardmäßigen Adoben-Farbfelder sind unten {#list-of-default-adobe-color-profiles-are-listed-below} aufgeführt.

| Name | Farbraum | Beschreibung |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RGB | Adobe RGB (1998) |
| AppleRGB | RGB | Apple RGB |
| CIERGB | RGB | CIE RGB |
| CoatedFogra27 | CMYK | Coated FOGRA27 (ISO 12647-2:2004) |
| CoatedFogra39 | CMYK | Coated FOGRA39 (ISO 12647-2:2004) |
| CoatedGraCol | CMYK | Coated GRACoL 2006 (ISO 12647-2:2004) |
| ColorMatchRGB | RGB | ColorMatch RGB |
| EuropeISOCoated | CMYK | Europe ISO Coated FOGRA27 |
| EuroscaleCoated | CMYK | Euroscale Coated v2 |
| EuroscaleUngestrichen | CMYK | Euroscale Ungestrichen v2 |
| JapanColorCoated | CMYK | Japan Color 2001 Coated |
| JapanColorNewspaper | CMYK | Japan Color 2002 Newspaper |
| JapanColorUngestern | CMYK | Japan Color 2001 Ungestrichen |
| JapanColorWebCoated | CMYK | Japan Color 2003 Web Coated |
| JapanWebCoated | CMYK | Japan Web Coated (Ad) |
| NewsprintSNAP2007 | CMYK | US Newsprint (SNAP 2007) |
| NTSC | RGB | NTSC (1953) |
| PAL | RGB | PAL/SECAM |
| ProPhoto | RGB | ProPhoto RGB |
| PS4Default | CMYK | Photoshop 4 Standard-CMYK |
| PS5Standard | CMYK | Photoshop 5 Standard-CMYK |
| SheetfeedCoated | CMYK | U.S. Sheetfeed Coated v2 |
| BlattmatteUngestrichen | CMYK | U.S. Sheetfeed Unstructured v2 |
| SMPTE | RGB | SMPTE-C |
| sRGB | RGB sRGB | IEC 61966-2.1 |
| UngestrichenFogra29 | CMYK | Ungestrichenes FOGRA29 (ISO 12647-2:2004) |
| WebCoated | CMYK | U.S. Web Coated (SWOP) v2 |
| WebCoatedFogra28 | CMYK | Web Coated FOGRA28 (ISO 12647-2:2004) |
| WebCoatedGrade3 | CMYK | Web Coated SWOP 2006 Grade 3 Paper |
| WebCoatedGrade5 | CMYK | Webbeschichtetes SWOP 2006 Grade 5 Papier |
| WebUngestern | CMYK | U.S. Web Ungestern v2 |
| WideGamutRGB | RGB | RGB-Farbraum |

## Zusätzliche Ressourcen{#additional-resources}

* [Konfigurieren des Farbmanagements für dynamische Medien](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)

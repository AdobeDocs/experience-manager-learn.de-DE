---
title: Verwenden von Berichten in AEM Assets
description: 'AEM Assets bietet ein Berichte-Framework auf Unternehmensebene, das für große Repositorys durch eine intuitive Benutzererfahrung skaliert wird. '
feature: reports
topics: authoring, operations, performance, metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
kt: 648
translation-type: tm+mt
source-git-commit: 3b5dd583a458393a41dbce1d8eeb0095a22db734
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 2%

---


# Verwenden von Berichten in AEM Assets{#using-reports-in-aem-assets}

AEM Assets bietet ein Berichte-Framework auf Unternehmensebene, das für große Repositorys durch eine intuitive Benutzererfahrung skaliert wird.

>[!VIDEO](https://video.tv.adobe.com/v/22140/?quality=12&learn=on)

## Microsoft Excel-Formeln {#excel-formulas}

Die folgenden Formeln werden im Video verwendet, um das Diagramm Assets nach Größe in Microsoft Excel zu generieren.

### Normalisierung der Asset-Größe in Byte {#asset-size-normalization-to-bytes}

```
=IF(RIGHT(D2,2)="KB",
      LEFT(D2,(LEN(D2)-2))*1024,
  IF(RIGHT(D2,2)="MB",
      LEFT(D2,(LEN(D2)-2))*1024*1024,
  IF(RIGHT(D2,2)="GB",
      LEFT(D2,(LEN(D2)-2))*1024*1024*1024,
  IF(RIGHT(D2,2)="TB",
      LEFT(D2,(LEN(D2)-2))*1024*1024*1024*1024, 0))))
```

### Asset-Anzahl nach Größe {#asset-count-by-size}

#### Weniger als 200 KB {#less-than-kb}

```
=COUNTIFS(E2:E1000,"< 200000")
```

#### 200 KB bis 500 KB {#kb-to-kb}

```
=COUNTIFS(E2:E1000,">= 200000", E2:E1000,"<= 500000")
```

#### Mehr als 500 KB {#greater-than-kb}

```
=COUNTIFS(E2:E1000,"> 500000")
```

## Zusätzliche Ressourcen{#additional-resources}

Excel-Datei [mit Diagramm herunterladen](./assets/asset-reports/all-assets.xlsx)

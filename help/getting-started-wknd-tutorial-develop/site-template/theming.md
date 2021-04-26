---
title: Arbeitsablauf für Designs
seo-title: Erste Schritte mit AEM Sites - Arbeitsablauf für Designs
description: Erfahren Sie, wie Sie die Themenquellen einer Adobe Experience Manager-Site aktualisieren, um markenspezifische Stile anzuwenden. Erfahren Sie, wie Sie mit einem Proxy-Server eine Live-Vorschau von CSS- und JavaScript-Updates Ansicht haben. In diesem Lernprogramm wird auch erläutert, wie Designaktualisierungen mithilfe von GitHub-Aktionen auf einer AEM Site bereitgestellt werden können.
sub-product: Sites
version: Cloud Service
type: Tutorial
feature: Kernkomponenten
topic: Content-Management, Entwicklung
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 1%

---


# Designarbeitsablauf {#theming}

>[!CAUTION]
>
> Die hier präsentierten Funktionen zur schnellen Site-Erstellung werden in der zweiten Jahreshälfte 2021 veröffentlicht. Die zugehörige Dokumentation steht zu Vorschauen zur Verfügung.

In diesem Kapitel werden wir die Themenquellen einer Adobe Experience Manager-Site aktualisieren, um markenspezifische Stile anzuwenden. Wir werden verdienen, wie Sie einen Proxy-Server zur Ansicht einer Vorschau von CSS und Javascript Updates während wir Code gegen die Live-Site. In diesem Lernprogramm wird auch erläutert, wie Designaktualisierungen mithilfe von GitHub-Aktionen auf einer AEM Site bereitgestellt werden können.

Am Ende wird unsere Website aktualisiert werden, um Stile, die mit der Marke WKND übereinstimmen.

## Voraussetzungen {#prerequisites}

Dies ist ein mehrteiliges Lernprogramm und es wird davon ausgegangen, dass die im Kapitel [Seitenvorlagen](./page-templates.md) beschriebenen Schritte abgeschlossen wurden.

## Ziele

1. Erfahren Sie, wie die Themenquellen für eine Site heruntergeladen und geändert werden können.
1. Erfahren Sie, wie Code für eine Echtzeit-Vorschau mit der Live-Site verwechselt wird.
1. Machen Sie sich mit dem durchgängigen Arbeitsablauf der Bereitstellung kompilierter CSS- und JavaScript-Updates als Teil eines Designs mithilfe von GitHub-Aktionen vertraut.

## Aktualisieren eines Designs {#theme-update}

Nehmen Sie dann Änderungen an den Themenquellen vor, damit die Site dem Erscheinungsbild der Marke WKND entspricht.

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

Schritte auf hoher Ebene für das Video:

1. Erstellen Sie einen lokalen Benutzer in AEM zur Verwendung mit einem Proxyentwicklungsserver.
1. Laden Sie die Designquellen von AEM herunter und öffnen Sie sie mit einer lokalen IDE, wie VSCode.
1. Ändern Sie die Designquellen und verwenden Sie einen Proxydev-Server, um CSS- und JavaScript-Änderungen in Echtzeit Vorschau.
1. Aktualisieren Sie die Themenquellen, sodass der Zeitschriftenartikel mit den Markenstilen und -modellen von WKND übereinstimmt.

### Lösungsdateien

Herunterladen der fertigen Stile für das [WKND-Design](assets/theming/WKND-THEME-src.zip)

## Bereitstellen eines Designs {#deploy-theme}

Stellen Sie mithilfe von GitHub-Aktionen Updates für ein Design in einer AEM Umgebung bereit.

>[!VIDEO](https://video.tv.adobe.com/v/332919/?quality=12&learn=on)

Schritte auf hoher Ebene für das Video:

1. hinzufügen Sie Ihr Design Sources-Projekt an [GitHub als neues Repository](https://docs.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line).
1. Erstellen Sie [ein persönliches Zugriffstoken in GitHub](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) und speichern Sie es an einem sicheren Speicherort.
1. Konfigurieren Sie die GitHub-Einstellungen in Ihrer AEM Umgebung, um auf Ihr GitHub-Repository zu verweisen und Ihr persönliches Zugriffstoken einzuschließen.
1. Erstellen Sie die folgenden [verschlüsselten Geheimnisse](https://docs.github.com/en/actions/reference/encrypted-secrets) in Ihrem GitHub-Repository:
   * **AEM_SITE**  - Stamm Ihrer AEM (d.h.  `wknd`)
   * **AEM_URL**  - URL Ihrer AEM Author-Umgebung
   * **GIT_TOKEN**  - Persönliches Zugriffstoken aus Schritt 2.
1. Führen Sie die GitHub-Aktion aus: **Erstellen und Bereitstellen von Github-Artefakten**. Dies hat die nachfolgende Wirkung der Aktion: **Aktualisieren Sie die Designkonfiguration auf AEM mit Artefakt-ID**, wodurch die Designquellen wie in `AEM_URL` und `AEM_SITE` angegeben auf der AEM Umgebung bereitgestellt werden.

### Beispielberichte

Es gibt einige Beispiele für GitHub-Berichte, die als Referenz verwendet werden können:

* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e) - Wird als Beispiel für &quot;echte&quot;Projekte verwendet.
* [https://github.com/godanny86/wknd-theme](https://github.com/godanny86/wknd-theme)  - Wird als Beispiel für diejenigen verwendet, die dem Tutorial folgen.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben soeben eine Aktualisierung erstellt und ein Thema AEM!

### Nächste Schritte {#next-steps}

Tauchen Sie ein in die AEM Entwicklung und lernen Sie die zugrunde liegende Technologie kennen, indem Sie mit dem [AEM Projekt Archetype](../project-archetype/overview.md) eine Site erstellen.

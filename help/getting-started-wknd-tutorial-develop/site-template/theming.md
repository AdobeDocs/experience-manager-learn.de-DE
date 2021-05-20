---
title: Designarbeitsablauf
seo-title: Erste Schritte mit AEM Sites - Designarbeitsablauf
description: Erfahren Sie, wie Sie die Themenquellen einer Adobe Experience Manager-Site aktualisieren, um markenspezifische Stile anzuwenden. Erfahren Sie, wie Sie mit einem Proxy-Server eine Live-Vorschau von CSS- und JavaScript-Aktualisierungen anzeigen können. In diesem Tutorial wird auch beschrieben, wie Sie mit GitHub-Aktionen Designaktualisierungen auf einer AEM Site bereitstellen.
sub-product: Sites
version: Cloud Service
type: Tutorial
feature: Kernkomponenten
topic: Content Management, Entwicklung
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 1%

---


# Designarbeitsablauf {#theming}

>[!CAUTION]
>
> Die hier vorgestellten Funktionen zur schnellen Site-Erstellung werden im zweiten Halbjahr 2021 veröffentlicht. Die zugehörige Dokumentation steht für Vorschauzwecke zur Verfügung.

In diesem Kapitel werden wir die Themenquellen einer Adobe Experience Manager-Site aktualisieren, um markenspezifische Stile anzuwenden. Wir werden erfahren, wie Sie mit einem Proxy-Server eine Vorschau von CSS- und JavaScript-Aktualisierungen anzeigen können, während wir Code für die Live-Site verwenden. In diesem Tutorial wird auch beschrieben, wie Sie mit GitHub-Aktionen Designaktualisierungen auf einer AEM Site bereitstellen.

Am Ende wird unsere Site aktualisiert, um Stile zur Marke WKND hinzuzufügen.

## Voraussetzungen {#prerequisites}

Dies ist ein mehrteiliges Tutorial, und es wird davon ausgegangen, dass die im Kapitel [Seitenvorlagen](./page-templates.md) beschriebenen Schritte abgeschlossen sind.

## Ziele

1. Erfahren Sie, wie die Themenquellen für eine Site heruntergeladen und geändert werden können.
1. Erfahren Sie, wie Sie Code mit der Live-Site vergleichen, um eine Echtzeitvorschau zu erhalten.
1. Machen Sie sich mit dem durchgängigen Arbeitsablauf der Bereitstellung kompilierter CSS- und JavaScript-Aktualisierungen als Teil eines Themas mit GitHub-Aktionen vertraut.

## Aktualisieren eines Designs {#theme-update}

Nehmen Sie als Nächstes Änderungen an den Themenquellen vor, damit die Site dem Erscheinungsbild der WKND-Marke entspricht.

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

Allgemeine Schritte für das Video:

1. Erstellen Sie einen lokalen Benutzer in AEM zur Verwendung mit einem Proxy-Entwicklungsserver.
1. Laden Sie die Designquellen von AEM herunter und öffnen Sie sie mit einer lokalen IDE, wie VSCode.
1. Ändern Sie die Designquellen und verwenden Sie einen Proxy-Dev-Server, um CSS- und JavaScript-Änderungen in Echtzeit in der Vorschau anzuzeigen.
1. Aktualisieren Sie die Themenquellen so, dass der Zeitschriftenartikel mit den Markenstilen und -mockups der WKND übereinstimmt.

### Lösungsdateien

Laden Sie die fertigen Stile für das [WKND-Design](assets/theming/WKND-THEME-src.zip) herunter.

## Bereitstellen eines Designs {#deploy-theme}

Stellen Sie mithilfe von GitHub-Aktionen Aktualisierungen an einem Design in einer AEM Umgebung bereit.

>[!VIDEO](https://video.tv.adobe.com/v/332919/?quality=12&learn=on)

Allgemeine Schritte für das Video:

1. Fügen Sie Ihr Design-Quellprojekt [GitHub als neues Repository](https://docs.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line) hinzu.
1. Erstellen Sie [ein persönliches Zugriffstoken in GitHub](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) und speichern Sie es an einem sicheren Ort.
1. Konfigurieren Sie die GitHub-Einstellungen in Ihrer AEM-Umgebung so, dass sie auf Ihr GitHub-Repository verweisen und Ihr persönliches Zugriffstoken einschließen.
1. Erstellen Sie die folgenden [verschlüsselten Geheimnisse](https://docs.github.com/en/actions/reference/encrypted-secrets) in Ihrem GitHub-Repository:
   * **AEM_SITE**  - Stamm Ihrer AEM Site (d. h.  `wknd`)
   * **AEM_URL**  - URL Ihrer AEM-Autorenumgebung
   * **GIT_TOKEN**  - Persönliches Zugriffstoken aus Schritt 2.
1. Führen Sie die GitHub-Aktion aus: **Erstellen und Bereitstellen von Github-Artefakten**. Dies hat den nachgelagerten Effekt, die Aktion auszuführen: **Aktualisieren Sie die Designkonfiguration auf AEM mit der Artefakt-ID**, die die Designquellen in der AEM -Umgebung bereitstellt, wie in `AEM_URL` und `AEM_SITE` angegeben.

### Beispielberichte

Es gibt einige Beispiele für GitHub-Repos, die als Referenz verwendet werden können:

* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e)  - Wird als Beispiel für &quot;reale&quot;Projekte verwendet.
* [https://github.com/godanny86/wknd-theme](https://github.com/godanny86/wknd-theme)  - Wird als Beispiel für diejenigen verwendet, die dem Tutorial folgen.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben soeben ein Thema aktualisiert und AEM bereitgestellt!

### Nächste Schritte {#next-steps}

Nehmen Sie einen tieferen Einblick in AEM Entwicklung und lernen Sie die zugrunde liegende Technologie kennen, indem Sie eine Website mit dem Projektarchetyp [AEM erstellen.](../project-archetype/overview.md)

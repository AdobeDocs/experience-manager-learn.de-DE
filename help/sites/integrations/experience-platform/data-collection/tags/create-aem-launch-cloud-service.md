---
title: Erstellen einer Tags-Cloud-Service-Konfiguration in AEM Sites
description: Erfahren Sie, wie Sie eine Tags-Cloud-Service-Konfiguration in AEM erstellen.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5982
thumbnail: 38566.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: a72ddced-37de-4b62-9e28-fa5b6c8ce5b7
duration: 99
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 100%

---

# Erstellen einer Tags-Cloud-Service-Konfiguration in AEM {#create-launch-cloud-service}

Erfahren Sie, wie Sie eine Tags-Cloud-Service-Konfiguration in Adobe Experience Manager erstellen. Die Tags-Cloud-Service-Konfiguration von AEM kann dann auf eine bestehende Site angewendet werden und das Laden der Tags-Bibliotheken kann sowohl in der Autoren- als auch in der Veröffentlichungsumgebung beobachtet werden.

## Erstellen eines Tags-Cloud-Service

Erstellen Sie die Tags-Cloud-Service-Konfiguration mithilfe der folgenden Schritte.

1. Wählen Sie aus dem **Tools**-Menü **Cloud-Services** aus und klicken Sie auf **Adobe Launch-Konfigurationen**
1. Wählen Sie den Konfigurationsordner Ihrer Site aus oder wählen Sie **WKND-Site** (bei Verwendung des WKND-Guide-Projekts) und klicken Sie auf **Erstellen**
1. Geben Sie auf der Registerkarte _Allgemein_ einen Namen für Ihre Konfiguration ein, indem Sie das **Titelfeld** benutzen, und wählen Sie **Adobe Launch** aus der Dropdown-Liste _Zugehörige Adobe IMS-Konfiguration_ aus. Wählen Sie dann Ihren Unternehmensnamen aus der _Firma_ Dropdown-Liste aus und wählen Sie die zuvor erstellte Eigenschaft aus der _Eigenschaften_-Dropdown-Liste.
1. Behalten Sie in den Registerkarten _Staging_ und _Produktion_ die Standardkonfigurationen bei. Es wird jedoch empfohlen, die Konfigurationen für die reale Produktionseinrichtung zu überprüfen und zu ändern, insbesondere den Umschalter _Bibliothek asynchron laden_, je nach Leistungs- und Optimierungsanforderungen. Beachten Sie außerdem, dass der _Bibliotheks-URI_-Wert für Staging und Produktion unterschiedlich ist.
1. Klicken Sie abschließend auf **Erstellen**, um die Tags-Cloud-Services zu vervollständigen.

   ![Tags-Cloud-Service-Konfiguration](assets/launch-cloud-services-config.png)

## Anwenden des Tags-Cloud-Service auf die Site

Um die Tag-Eigenschaft und ihre Bibliotheken auf die AEM-Site zu laden, wird die Tags-Cloud-Service-Konfiguration auf die Site angewendet. Im vorherigen Schritt wird die Cloud-Service-Konfiguration unter dem Site-Namen-Ordner (WKND-Site) erstellt, sodass sie automatisch angewendet werden sollte. Überprüfen wir es.

1. Wählen Sie im **Navigationsmenü** das **Sites**-Symbol.

1. Wählen Sie die Stammseite der AEM-Site aus und klicken Sie auf **Eigenschaften**. Navigieren Sie dann zur Registerkarte **Erweitert** und überprüfen Sie unter **Konfiguration**, ob der Cloud-Konfigurationswert auf Ihren Site-spezifischen `conf`-Ordner zeigt.

   ![Anwenden der Cloud-Services-Konfiguration auf die Site](assets/apply-cloud-services-config-to-site.png)

## Überprüfen des Ladens der Tag-Eigenschaft auf Autoren- und Veröffentlichungsseiten

Jetzt ist es an der Zeit, zu überprüfen, ob die Tag-Eigenschaft und ihre Bibliotheken auf die AEM-Site-Seite geladen werden.

1. Öffnen Sie Ihre bevorzugte Site-Seite im Modus **Als veröffentlicht anzeigen**. In der Browser-Konsole sollte die Protokollmeldung angezeigt werden. Es handelt sich um dieselbe Meldung aus dem JavaScript-Code-Snippet der Tag-Eigenschaftsregel, die ausgelöst wird, wenn das Ereignis _Bibliothek geladen (Seitenanfang)_ ausgelöst wird.

1. Um dies in der Veröffentlichungsinstanz zu überprüfen, veröffentlichen Sie zunächst Ihre **Tags-Cloud-Service**-Konfiguration und öffnen Sie die Site-Seite in der Veröffentlichungsinstanz.

   ![Tag-Eigenschaft auf Autoren- und Veröffentlichungsseiten](assets/tag-property-on-author-publish-pages.png)

Herzlichen Glückwunsch! Sie haben die AEM- und Datenerfassungs-Tag-Integration abgeschlossen, die JavaScript-Code in Ihre AEM-Site einfügt, ohne den AEM-Projektcode zu aktualisieren.

## Herausforderung – Regel in Tag-Eigenschaft aktualisieren und veröffentlichen

Nutzen Sie die Erfahrungen aus der vorherigen Dokumentation [Erstellen einer Tag-Eigenschaft](./create-tag-property.md), um die einfache Herausforderung abzuschließen. Aktualisieren Sie die vorhandene Regel, um zusätzliche Konsolenanweisungen hinzuzufügen, und verwenden Sie den _Veröffentlichungsfluss_ zur Bereitstellung auf der AEM-Site.

## Nächste Schritte

[Debugging einer Tags-Implementierung](debug-tags-implementation.md)

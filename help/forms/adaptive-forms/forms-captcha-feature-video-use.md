---
title: Verwenden von CAPTCHAs mit AEM adaptiven Forms
seo-title: Verwenden von CAPTCHAs mit AEM adaptiven Forms
description: Hinzufügen und Verwenden eines CAPTCHA mit AEM adaptiven Forms.
seo-description: Hinzufügen und Verwenden eines CAPTCHA mit AEM adaptiven Forms.
feature: Adaptive Forms,Workflow
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
uuid: bd63e207-4f4d-4f34-9ac4-7572ed26f646
discoiquuid: 5e184e44-e385-4df7-b7ed-085239f2a642
topic: Entwicklung
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 11%

---


# Verwenden von CAPTCHAs mit AEM adaptiven Forms{#using-captchas-with-aem-adaptive-forms}

Hinzufügen und Verwenden eines CAPTCHA mit AEM adaptiven Forms.

Besuchen Sie die Seite [AEM Forms samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0) für einen Link zu einer Live-Demo dieser Funktion.

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*Dieses Video führt Sie durch den Prozess, ein CAPTCHA zu einem AEM adaptiven Formular hinzuzufügen, das sowohl den integrierten CAPTCHA-Dienst AEM als auch den reCAPTCHA-Dienst von Google verwendet.*

>[!NOTE]
>
>Diese Funktion ist nur ab AEM 6.3 verfügbar.

>[!NOTE]
>
>**Gehen Sie wie folgt vor, um reCaptcha in der Veröffentlichungsinstanz zu konfigurieren**
>
>Konfigurieren von reCaptach in der Autoreninstanz
>
>Öffnen Sie die Felix [Web Console](http://localhost:4502/system/console/bundles) in der Autoreninstanz.
>
>Suchen Sie nach com.adobe.granite.crypto.file bundle .
>
>Notieren Sie die Bundle-ID. In meiner Instanz ist es 20
>
>Navigieren Sie zur Bundle-ID im Dateisystem in Ihrer Autoreninstanz.
>
>* &lt;Autor-AEM-Installationsverzeichnis>/crx-quickstart/launchpad/felix/bundle20/data
* Kopieren Sie die HMAC- und die Master-Dateien

Öffnen Sie die [felix-Webkonsole](http://localhost:4502/system/console/bundles) in Ihrer Veröffentlichungsinstanz. Suchen Sie nach dem com.adobe.granite.crypto.file -Bundle. Beachten Sie die Bundle-ID
Navigieren Sie zur Bundle-ID im Dateisystem Ihrer Veröffentlichungsinstanz.
* &lt;Veröffentlichung-AEM-Installationsverzeichnis>/crx-quickstart/launchpad/felix/bundle20/data
* Löschen Sie die vorhandenen HMAC- und Übergeordneten Dateien.
* Fügen Sie die HMAC- und Übergeordneten Dateien ein, die aus der Autoreninstanz kopiert wurden.

AEM Veröffentlichungsserver neu starten

## Unterstützende Materialien {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)


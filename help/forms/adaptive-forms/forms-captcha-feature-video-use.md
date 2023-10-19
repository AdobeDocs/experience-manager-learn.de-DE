---
title: Verwendung von CAPTCHAs mit adaptiven Formularen von AEM
description: Hinzufügen und Verwenden eines CAPTCHA mit adaptiven Formularen von AEM.
feature: Adaptive Forms,Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 7e5dcc6e-fe56-49af-97e3-7dfaa9c8738f
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '204'
ht-degree: 100%

---

# Verwendung von CAPTCHAs mit adaptiven Formularen von AEM{#using-captchas-with-aem-adaptive-forms}

Hinzufügen und Verwenden eines CAPTCHA mit adaptiven Formularen von AEM.

>[!VIDEO](https://video.tv.adobe.com/v/18336?quality=12&learn=on)

*Dieses Video führt Sie durch den Prozess, ein CAPTCHA zu einem adaptiven Formular von AEM hinzuzufügen, das sowohl den integrierten AEM-CAPTCHA-Dienst als auch den reCAPTCHA-Dienst von Google verwendet.*

>[!NOTE]
>
>Diese Funktion ist nur ab AEM 6.3 verfügbar.

>[!NOTE]
>
>**Gehen Sie wie folgt vor, um reCaptcha in der Veröffentlichungsinstanz zu konfigurieren**
>
>Konfigurieren von reCaptcha in der Autoreninstanz:
>
>Öffnen Sie die Felix-[Web-Konsole](http://localhost:4502/system/console/bundles) in der Autoreninstanz.
>
>Suchen Sie nach dem Bundle „com.adobe.granite.crypto.file“.
>
>Notieren Sie sich die Bundle-ID. In meiner Instanz ist es 20.
>
>Navigieren Sie zur Bundle-ID im Dateisystem in Ihrer AuthAutoreninstanz:
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
>* Kopieren Sie die HMAC- und die Primärdateien.
>
>Öffnen Sie die [Felix-Web-Konsole](http://localhost:4502/system/console/bundles) in Ihrer Veröffentlichungsinstanz Suchen Sie nach dem Bundle „com.adobe.granite.crypto.file“. Notieren Sie sich die Bundle-ID.
>
>Navigieren Sie zur Bundle-ID im Dateisystem Ihrer Veröffentlichungsinstanz:
>
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* Löschen Sie die vorhandenen HMAC- und Primärdateien.
* Fügen Sie die aus der Authoring-Instanz kopierten HMAC- und Primärdateien ein.
>
Starten Sie Ihren AEM Publish-Server neu.

## Hilfsmaterialien {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

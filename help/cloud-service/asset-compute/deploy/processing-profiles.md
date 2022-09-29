---
title: Integrieren von Asset compute-Workern mit AEM-Verarbeitungsprofilen
description: AEM as a Cloud Service Integration mit Asset compute-Workern, die über AEM Assets-Verarbeitungsprofile in Adobe I/O Runtime bereitgestellt werden. Verarbeitungsprofile werden im Autorendienst so konfiguriert, dass bestimmte Assets mit benutzerdefinierten Sekundären verarbeitet und die von den Sekundären generierten Dateien als Asset-Ausgabedarstellungen gespeichert werden.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '625'
ht-degree: 3%

---

# Integration mit AEM Verarbeitungsprofilen

Damit Asset compute-Sekundäre benutzerdefinierte Ausgabeformate in AEM as a Cloud Service generieren können, müssen sie über Verarbeitungsprofile AEM as a Cloud Service Autorendienst registriert sein. Bei allen Assets, die diesem Verarbeitungsprofil unterliegen, wird der Worker beim Hochladen oder erneuten Verarbeiten aufgerufen und die benutzerdefinierte Ausgabedarstellung wird generiert und über die Ausgabeformate des Assets bereitgestellt.

## Definieren eines Verarbeitungsprofils

Erstellen Sie zunächst ein neues Verarbeitungsprofil, das den Worker mit den konfigurierbaren Parametern aufruft.

![Verarbeitungsprofil](./assets/processing-profiles/new-processing-profile.png)

1. Melden Sie sich beim as a Cloud Service Autorendienst AEM als __AEM Administrator__. Da es sich um ein Tutorial handelt, empfehlen wir die Verwendung einer Entwicklungsumgebung oder einer Umgebung in einer Sandbox.
1. Gehen Sie zu __Tools > Assets > Verarbeitungsprofile__
1. Tippen __Erstellen__ button
1. Benennen Sie das Verarbeitungsprofil. `WKND Asset Renditions`
1. Tippen Sie auf __Benutzerdefiniert__ Registerkarte und tippen Sie auf __Neu hinzufügen__
1. Definieren des neuen Dienstes
   + __Name des Ausgabeformats:__ `Circle`
      + Der Dateiname der Ausgabedarstellung, mit dem diese Ausgabedarstellung in AEM Assets identifiziert wird
   + __Erweiterung:__ `png`
      + Die Erweiterung der generierten Ausgabedarstellung. Legen Sie fest auf `png` da dies das unterstützte Ausgabeformat ist, das der Webdienst des Arbeitnehmers unterstützt, und zu einem transparenten Hintergrund hinter dem ausgeteilten Kreis führt.
   + __Endpunkt:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Dies ist die URL zum Worker, der über abgerufen wurde. `aio app get-url`. Stellen Sie sicher, dass die URL auf den richtigen Arbeitsbereich verweist, der auf der AEM as a Cloud Service Umgebung basiert.
      + Stellen Sie sicher, dass die Worker-URL auf den richtigen Arbeitsbereich verweist. AEM as a Cloud Service Bühne sollte die Staging-Workspace-URL verwenden, und AEM as a Cloud Service Produktion sollte die URL des Produktionsarbeitsbereichs verwenden.
   + __Dienstparameter__
      + Tippen __Parameter hinzufügen__
         + Schlüssel: `size`
         + Wert: `1000`
      + Tippen __Parameter hinzufügen__
         + Schlüssel: `contrast`
         + Wert: `0.25`
      + Tippen __Parameter hinzufügen__
         + Schlüssel: `brightness`
         + Wert: `0.10`
      + Diese Schlüssel-Wert-Paare, die an den Asset compute Worker übergeben und über verfügbar sind `rendition.instructions` JavaScript-Objekt.
   + __MIME-Typen__
      + __Umfasst:__ `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + Diese MIME-Typen sind die einzigen, die die npm-Module des Sekundärs enthalten. Diese Liste beschränkt, welche vom benutzerdefinierten Worker verarbeitet werden.
      + __Schließt Folgendes aus:__ `Leave blank`
         + Verarbeiten Sie niemals Assets mit diesen MIME-Typen mithilfe dieser Dienstkonfiguration. In diesem Fall verwenden wir nur eine Zulassungsliste.
1. Tippen __Speichern__ oben rechts

## Anwenden und Aufrufen eines Verarbeitungsprofils

1. Wählen Sie das neu erstellte Verarbeitungsprofil aus. `WKND Asset Renditions`
1. Tippen __Profil auf Ordner anwenden__ in der oberen Aktionsleiste
1. Wählen Sie einen Ordner aus, auf den das Verarbeitungsprofil angewendet werden soll, z. B. `WKND` und tippen __Anwenden__
1. Navigieren Sie zu dem Ordner, auf den das Verarbeitungsprofil nicht angewendet wurde über __AEM > Assets > Dateien__ und tippen Sie auf `WKND`.
1. Hochladen einiger neuer Bild-Assets ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg)und [sample-3.jpg](../assets/samples/sample-3.jpg)) in einem beliebigen Ordner unter dem Ordner, auf den das Verarbeitungsprofil angewendet wurde, und warten Sie, bis das hochgeladene Asset verarbeitet wird.
1. Tippen Sie auf das Asset, um seine Details zu öffnen.
   + Standardausgabeformate können in AEM schneller generiert und angezeigt werden als benutzerdefinierte Ausgabeformate.
1. Öffnen Sie die __Ausgabeformate__ Ansicht über die linke Seitenleiste
1. Tippen Sie auf das Asset mit dem Namen `Circle.png` und überprüfen Sie die generierte Ausgabedarstellung.

   ![Generierte Ausgabe](./assets/processing-profiles/rendition.png)

## Beendet!

Herzlichen Glückwunsch! Sie haben die [Tutorial](../overview.md) auf der Erweiterung AEM as a Cloud Service Asset compute Microservices! Sie sollten jetzt benutzerdefinierte Asset compute-Sekundäre einrichten, entwickeln, testen, debuggen und bereitstellen können, die von Ihrem AEM as a Cloud Service Autorendienst verwendet werden.

### Überprüfen Sie den vollständigen Projektquellcode auf Github.

Das endgültige Asset compute-Projekt ist auf Github verfügbar unter:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github enthält den endgültigen Status des Projekts, der vollständig mit den Arbeits- und Testfällen gefüllt ist, jedoch keine Anmeldeinformationen enthält, d. h. `.env`, `.config.json` oder `.aio`._

## Fehlerbehebung

+ [Benutzerdefinierte Ausgabedarstellung fehlt im Asset in AEM](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [Die Asset-Verarbeitung schlägt AEM fehl](../troubleshooting.md#asset-processing-fails)

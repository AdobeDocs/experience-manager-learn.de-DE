---
title: Integrieren von Asset compute-Workern mit AEM-Verarbeitungsprofilen
description: AEM as a Cloud Service kann mit Asset compute-Workern integriert werden, die über AEM Assets-Verarbeitungsprofile in Adobe I/O Runtime bereitgestellt werden. Verarbeitungsprofile werden im Autorendienst so konfiguriert, dass bestimmte Assets mit benutzerdefinierten Sekundären verarbeitet und die von den Sekundären generierten Dateien als Asset-Ausgabedarstellungen gespeichert werden.
feature: asset compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
topic: Integrationen, Entwicklung
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '635'
ht-degree: 2%

---


# Integration mit AEM Verarbeitungsprofilen

Damit Asset compute-Sekundäre benutzerdefinierte Ausgabeformate in AEM as a Cloud Service generieren können, müssen sie über Verarbeitungsprofile in AEM als Cloud Service-Autorendienst registriert sein. Bei allen Assets, die diesem Verarbeitungsprofil unterliegen, wird der Worker beim Hochladen oder erneuten Verarbeiten aufgerufen und die benutzerdefinierte Ausgabedarstellung wird generiert und über die Ausgabeformate des Assets bereitgestellt.

## Definieren eines Verarbeitungsprofils

Erstellen Sie zunächst ein neues Verarbeitungsprofil, das den Worker mit den konfigurierbaren Parametern aufruft.

![Verarbeitungsprofil](./assets/processing-profiles/new-processing-profile.png)

1. Melden Sie sich bei AEM als Cloud Service-Autorendienst als __AEM Administrator__ an. Da es sich um ein Tutorial handelt, empfehlen wir die Verwendung einer Entwicklungsumgebung oder einer Umgebung in einer Sandbox.
1. Navigieren Sie zu __Tools > Assets > Verarbeitungsprofile__ .
1. Tippen Sie auf die Schaltfläche __Erstellen__
1. Benennen Sie das Verarbeitungsprofil `WKND Asset Renditions`.
1. Tippen Sie auf die Registerkarte __Benutzerdefiniert__ und dann auf __Neu hinzufügen__
1. Definieren des neuen Dienstes
   + __Name des Ausgabeformats:__ `Circle`
      + Die Dateinamenausgabe, die zur Identifikation dieser Ausgabedarstellung in AEM Assets verwendet wird
   + __Erweiterung:__ `png`
      + Die Erweiterung der Ausgabedarstellung, die generiert wird. Setzen Sie dies auf `png` , da dies das unterstützte Ausgabeformat ist, das der Webdienst des Sekundärs unterstützt, und zu transparentem Hintergrund hinter dem ausgeschnittenen Kreis führt.
   + __Endpunkt:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Dies ist die URL zum Worker, der über `aio app get-url` abgerufen wurde. Stellen Sie sicher, dass die URL auf den richtigen Arbeitsbereich verweist, basierend auf der AEM als Cloud Service-Umgebung.
      + Stellen Sie sicher, dass die Worker-URL auf den richtigen Arbeitsbereich verweist. AEM as a Cloud Service Stage sollte die Staging-Workspace-URL verwenden und AEM as a Cloud Service Production sollte die URL des Produktionsarbeitsbereichs verwenden.
   + __Dienstparameter__
      + Tippen Sie auf __Parameter__ hinzufügen
         + Schlüssel: `size`
         + Wert: `1000`
      + Tippen Sie auf __Parameter__ hinzufügen
         + Schlüssel: `contrast`
         + Wert: `0.25`
      + Tippen Sie auf __Parameter__ hinzufügen
         + Schlüssel: `brightness`
         + Wert: `0.10`
      + Diese Schlüssel-Wert-Paare, die an den Asset compute Worker übergeben und über das JavaScript-Objekt `rendition.instructions` verfügbar sind.
   + __MIME-Typen__
      + __Umfasst:__ `image/jpeg`,  `image/png`,  `image/gif`,  `image/bmp`,  `image/tiff`
         + Diese MIME-Typen sind die einzigen, die die npm-Module des Sekundärs enthalten. Diese Liste beschränkt, welche Assets vom benutzerdefinierten Worker verarbeitet werden.
      + __Schließt Folgendes aus:__ `Leave blank`
         + Verarbeiten Sie niemals Assets mit diesen MIME-Typen mithilfe dieser Dienstkonfiguration. In diesem Fall verwenden wir nur eine Zulassungsliste.
1. Tippen Sie oben rechts auf __Speichern__ .

## Anwenden und Aufrufen eines Verarbeitungsprofils

1. Wählen Sie das neu erstellte Verarbeitungsprofil `WKND Asset Renditions` aus.
1. Tippen Sie in der oberen Aktionsleiste auf __Profil auf Ordner anwenden__ .
1. Wählen Sie einen Ordner aus, auf den das Verarbeitungsprofil angewendet werden soll, z. B. `WKND` und tippen Sie auf __Anwenden__
1. Navigieren Sie zum Ordner, auf den das Verarbeitungsprofil nicht angewendet wurde, über __AEM > Assets > Dateien__ und tippen Sie auf `WKND`.
1. Laden Sie einige neue Bild-Assets ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg) und [sample-3.jpg](../assets/samples/sample-3.jpg)) in einen beliebigen Ordner unter dem Ordner hoch, auf den das Verarbeitungsprofil angewendet wurde, und warten Sie, bis das hochgeladene Asset verarbeitet wird.
1. Tippen Sie auf das Asset, um seine Details zu öffnen.
   + Standardausgabeformate können in AEM schneller generiert und angezeigt werden als benutzerdefinierte Ausgabeformate.
1. Öffnen Sie die Ansicht __Ausgabeformate__ in der linken Seitenleiste.
1. Tippen Sie auf das Asset mit dem Namen `Circle.png` und überprüfen Sie das generierte Ausgabeformat.

   ![Generierte Ausgabe](./assets/processing-profiles/rendition.png)

## Beendet!

Herzlichen Glückwunsch! Sie haben das [Tutorial](../overview.md) zum Erweitern von AEM as a Cloud Service Asset compute Microservices abgeschlossen! Sie sollten jetzt benutzerdefinierte Asset compute-Sekundäre einrichten, entwickeln, testen, debuggen und bereitstellen können, um sie für Ihre AEM als Cloud Service-Autorendienst zu verwenden.

### Überprüfen Sie den vollständigen Projektquellcode auf Github.

Das endgültige Asset compute-Projekt ist auf Github verfügbar unter:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github enthält den endgültigen Status des Projekts, der vollständig mit den Arbeits- und Testfällen gefüllt ist, jedoch keine Anmeldeinformationen enthält, d. h. `.env`, `.config.json` oder `.aio`._

## Fehlerbehebung

+ [Benutzerdefinierte Ausgabedarstellung fehlt im Asset in AEM](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [Die Asset-Verarbeitung schlägt AEM fehl](../troubleshooting.md#asset-processing-fails)

---
title: Integrieren von Asset Compute-Sekundären mit AEM-Verarbeitungsprofilen
description: AEM as a Cloud Service kann in Asset Compute-Sekundäre integriert werden, die über AEM Assets-Verarbeitungsprofile für Adobe I/O Runtime bereitgestellt werden. Verarbeitungsprofile werden im Author-Service so konfiguriert, dass bestimmte Assets mit benutzerdefinierten Sekundären verarbeitet und die von den Sekundären generierten Dateien als Asset-Ausgabedarstellungen gespeichert werden.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
duration: 179
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '622'
ht-degree: 100%

---

# Integrieren mit AEM-Verarbeitungsprofilen

Damit Asset Compute-Sekundäre benutzerdefinierte Ausgabedarstellungen in AEM as a Cloud Service generieren können, müssen sie über Verarbeitungsprofile im Author-Service von AEM as a Cloud Service registriert sein. Bei allen Assets, die diesem Verarbeitungsprofil unterliegen, wird der Sekundär beim Hochladen oder erneuten Verarbeiten aufgerufen. Dann wird die benutzerdefinierte Ausgabedarstellung generiert und über die Ausgabedarstellungen des Assets bereitgestellt.

## Definieren eines Verarbeitungsprofils

Erstellen Sie zunächst ein neues Verarbeitungsprofil, das den Sekundär mit den konfigurierbaren Parametern aufruft.

![Verarbeitungsprofil](./assets/processing-profiles/new-processing-profile.png)

1. Melden Sie sich beim Author-Service von AEM as a Cloud Service als __AEM-Admin__ an. Da es sich hierbei um ein Tutorial handelt, empfehlen wir die Verwendung einer Entwicklungsumgebung oder einer Umgebung in einer Sandbox.
1. Gehen Sie zu __Tools > Assets > Verarbeitungsprofile__
1. Klicken Sie auf die Schaltfläche __Erstellen__.
1. Nennen Sie das Verarbeitungsprofil `WKND Asset Renditions`.
1. Klicken Sie auf die Registerkarte __Benutzerdefiniert__ und dann auf __Neu hinzufügen__.
1. Definieren Sie den neuen Service:
   + __Name der Ausgabedarstellung:__ `Circle`
      + Der Dateiname der Ausgabedarstellung, mit dem diese Ausgabedarstellung in AEM Assets identifiziert wird.
   + __Erweiterung:__ `png`
      + Die Erweiterung der generierten Ausgabedarstellung. Legen Sie sie auf `png` fest, da dies das unterstützte Ausgabeformat ist, das der Webservice des Sekundärs unterstützt und das zu einem transparenten Hintergrund hinter dem ausgeschnittenen Kreis führt.
   + __Endpunkt:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Dies ist die über `aio app get-url` abgerufene URL zum Sekundär. Stellen Sie sicher, dass die URL auf den richtigen Arbeitsbereich verweist, basierend auf der AEM as a Cloud Service-Umgebung.
      + Stellen Sie sicher, dass die Sekundär-URL auf den richtigen Arbeitsbereich verweist. Für das AEM as a Cloud Service-Staging sollte die URL des Staging-Arbeitsbereichs verwenden werden und für die AEM as a Cloud Service-Produktion die URL des Produktionsarbeitsbereichs.
   + __Dienstparameter__
      + Klicken Sie auf __Parameter hinzufügen__.
         + Schlüssel: `size`
         + Wert: `1000`
      + Klicken Sie auf __Parameter hinzufügen__.
         + Schlüssel: `contrast`
         + Wert: `0.25`
      + Klicken Sie auf __Parameter hinzufügen__.
         + Schlüssel: `brightness`
         + Wert: `0.10`
      + Diese Schlüssel-Wert-Paare werden an den Asset Compute-Sekundär weitergegeben und sind über das JavaScript-Objekt `rendition.instructions` verfügbar.
   + __MIME-Typen__
      + __Eingeschlossen:__ `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + Diese MIME-Typen sind die einzigen, die in den npm-Modulen des Sekundärs enthalten sind. Diese Liste beschränkt, welche vom benutzerdefinierten Sekundär verarbeitet werden.
      + __Ausgeschlossen:__ `Leave blank`
         + Verarbeiten Sie über diese Service-Konfiguration niemals Assets mit diesen MIME-Typen. In diesem Fall verwenden wir nur eine Zulassungsliste.
1. Klicken Sie oben rechts auf __Speichern__.

## Anwenden und Aufrufen eines Verarbeitungsprofils

1. Wählen Sie das neu erstellte Verarbeitungsprofil `WKND Asset Renditions` aus.
1. Klicken Sie in der oberen Aktionsleiste auf __Profil auf Ordner anwenden__.
1. Wählen Sie einen Ordner aus, auf den das Verarbeitungsprofil angewendet werden soll, beispielsweise `WKND`, und klicken Sie auf __Anwenden__.
1. Navigieren Sie über __AEM > Assets > Dateien__ zu dem Ordner, auf den das Verarbeitungsprofil nicht angewendet wurde, und tippen Sie auf `WKND`.
1. Laden Sie einige neue Bild-Assets ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg) und [sample-3.jpg](../assets/samples/sample-3.jpg)) in einen beliebigen Ordner unter dem Ordner mit dem angewandten Verarbeitungsprofil hoch und warten Sie, bis das hochgeladene Asset verarbeitet wird.
1. Tippen Sie auf das Asset, um dessen Details zu öffnen.
   + Standardausgabedarstellungen können in AEM schneller generiert und angezeigt werden als benutzerdefinierte Ausgabedarstellungen.
1. Öffnen Sie die Ansicht __Ausgabedarstellungen__ in der linken Seitenleiste
1. Tippen Sie auf das Asset namens `Circle.png` und überprüfen Sie die generierte Ausgabedarstellung

   ![Generierte Ausgabedarstellung](./assets/processing-profiles/rendition.png)

## Fertig!

Herzlichen Glückwunsch! Sie haben das [Tutorial](../overview.md) zur Erweiterung von Asset Compute-Microservices in AEM as a Cloud Service abgeschlossen! Sie sollten jetzt benutzerdefinierte Asset Compute-Sekundäre einrichten, entwickeln, testen, debuggen und bereitstellen können, die von Ihrem Author-Service in AEM as a Cloud Service verwendet werden.

### Überprüfen Sie den vollständigen Quell-Code des Projekts auf Github.

Das fertige Asset Compute-Projekt ist auf Github verfügbar unter:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github enthält den fertigen Status des Projekts, in dem es vollständig mit dem Sekundär und Testfällen gefüllt ist, jedoch keine Anmeldeinformationen enthält, d. h. `.env`, `.config.json` oder `.aio`._

## Fehlerbehebung

+ [Benutzerdefinierte Ausgabedarstellung fehlt in einem Asset in AEM](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [Asset-Verarbeitung schlägt in AEM fehl](../troubleshooting.md#asset-processing-fails)

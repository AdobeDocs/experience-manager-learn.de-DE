---
title: Metadatengesteuerte Berechtigungen in AEM Assets
description: Metadatengesteuerte Berechtigungen sind eine Funktion, mit der der Zugriff auf basierend auf Asset-Metadateneigenschaften und nicht auf der Ordnerstruktur eingeschränkt wird.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-13757
doc-type: Tutorial
last-substantial-update: 2024-05-03T00:00:00Z
exl-id: 57478aa1-c9ab-467c-9de0-54807ae21fb1
source-git-commit: 98b26eb15c2fe7d1cf73fe028b2db24087c813a5
workflow-type: tm+mt
source-wordcount: '738'
ht-degree: 54%

---

# Metadatengesteuerte Berechtigungen{#metadata-driven-permissions}

Metadatengesteuerte Berechtigungen sind eine Funktion, mit der Zugriffssteuerungsentscheidungen in der AEM Assets-Autoreninstanz auf der Grundlage von Asset-Metadateneigenschaften und nicht auf der Ordnerstruktur festgelegt werden können. Mit dieser Funktion können Sie Zugriffssteuerungsrichtlinien definieren, die Attribute wie den Asset-Status, den -Typ oder eine von Ihnen definierte benutzerdefinierte Metadateneigenschaft auswerten.

Sehen wir uns ein Beispiel an. Kreative laden ihre Arbeit in AEM Assets in den Ordner hoch, der mit der Kampagne in Verbindung steht. Möglicherweise handelt es sich um ein laufendes Asset, das noch nicht zur Verwendung freigegeben wurde. Wir möchten sicherstellen, dass Marketing-Fachkräfte nur genehmigte Assets für diese Kampagne sehen. Wir können die Metadateneigenschaft verwenden, um anzugeben, dass ein Asset genehmigt wurde und von Marketing-Fachkräften verwendet werden kann.

## Funktionsweise

Die Aktivierung metadatengesteuerter Berechtigungen umfasst die Definition, welche Asset-Metadateneigenschaften Zugriffsbeschränkungen wie „Status“ oder „Marke“ darstellen. Diese Eigenschaften können dann verwendet werden, um Zugriffssteuerungseinträge zu erstellen, die angeben, welche Benutzergruppen Zugriff auf Assets mit bestimmten Eigenschaftswerten haben.

## Voraussetzungen

Der Zugriff auf eine AEM as a Cloud Service-Umgebung, die auf die neueste Version aktualisiert wurde, ist erforderlich, um metadatengesteuerte Berechtigungen einzurichten.

## OSGi-Konfiguration {#configure-permissionable-properties}

Um metadatengesteuerte Berechtigungen zu implementieren, muss ein Entwickler eine OSGi-Konfiguration bereitstellen, um as a Cloud Service zu AEM, sodass bestimmte Asset-Metadateneigenschaften metadatengesteuerte Berechtigungen ermöglichen.

1. Bestimmen Sie, welche Asset-Metadateneigenschaften für die Zugriffssteuerung verwendet werden. Die Eigenschaftsnamen sind die JCR-Eigenschaftsnamen auf der Asset-Seite `jcr:content/metadata` Ressource. In unserem Fall handelt es sich um eine Eigenschaft namens `status`.
1. Erstellen einer OSGi-Konfiguration `com.adobe.cq.dam.assetmetadatarestrictionprovider.impl.DefaultRestrictionProviderConfiguration.cfg.json` in Ihrem AEM Maven-Projekt.
1. Fügen Sie die folgende JSON-Datei in die erstellte Datei ein:

   ```json
   {
     "restrictionPropertyNames":[
       "status",
       "brand"
     ],
     "enabled":true
   }
   ```

1. Ersetzen Sie die Eigenschaftsnamen durch die erforderlichen Werte.

## Zurücksetzen der grundlegenden Asset-Berechtigungen

Bevor Sie einschränkungsbasierte Zugriffssteuerungseinträge hinzufügen, sollte ein neuer Eintrag auf oberster Ebene hinzugefügt werden, um zunächst den Lesezugriff für alle Gruppen zu verweigern, die einer Auswertung der Berechtigungen für Assets unterliegen (z. B. „Mitwirkende“ oder Ähnliches):

1. Navigieren Sie zum __Tools > Sicherheit > Berechtigungen__ Bildschirm
1. Wählen Sie die __Mitarbeiter__ Gruppe (oder eine andere benutzerspezifische Gruppe, zu der alle Benutzergruppen gehören)
1. Klicks __ACE hinzufügen__ in der oberen rechten Ecke des Bildschirms
1. Auswählen `/content/dam` für __Pfad__
1. Eingabe `jcr:read` für __Berechtigungen__
1. Auswählen `Deny` für __Berechtigungstyp__
1. Wählen Sie unter Einschränkungen die Option `rep:ntNames` und eingeben `dam:Asset` als __Beschränkungswert__
1. Klicken Sie auf __Speichern__.

![Verweigern des Zugriffs](./assets/metadata-driven-permissions/deny-access.png)

## Zugriff auf Assets über Metadaten gewähren

Zugriffssteuerungseinträge können nun hinzugefügt werden, um den Benutzergruppen Lesezugriff zu gewähren, basierend auf der [konfigurierte Asset-Metadateneigenschaftswerte](#configure-permissionable-properties).

1. Navigieren Sie zum __Tools > Sicherheit > Berechtigungen__ Bildschirm
1. Wählen Sie die Benutzergruppen aus, die Zugriff auf die Assets haben sollen
1. Klicks __ACE hinzufügen__ in der oberen rechten Ecke des Bildschirms
1. Auswählen `/content/dam` (oder einen Unterordner) für __Pfad__
1. Eingabe `jcr:read` für __Berechtigungen__
1. Auswählen `Allow` für __Berechtigungstyp__
1. under __Einschränkungen__, wählen Sie eine der [konfigurierte Asset-Metadaten-Eigenschaftsnamen in der OSGi-Konfiguration](#configure-permissionable-properties)
1. Geben Sie den erforderlichen Metadaten-Eigenschaftswert im __Beschränkungswert__ field
1. Klicken Sie auf __+__ -Symbol, um die Einschränkung zum Zugriffskontrolleintrag hinzuzufügen.
1. Klicken Sie auf __Speichern__.

![Zulassen des Zugriffs](./assets/metadata-driven-permissions/allow-access.png)

## Metadatengesteuerte Berechtigungen sind in Kraft

Beispielordner enthält einige Assets.

![Admin-Ansicht](./assets/metadata-driven-permissions/admin-view.png)

Nachdem Sie Berechtigungen konfiguriert und die Asset-Metadateneigenschaften entsprechend festgelegt haben, sehen Benutzerinnen und Benutzer (in unserem Fall Marketing-Benutzerinnen und -Benutzer) nur genehmigte Assets.

![Marketing-Ansicht](./assets/metadata-driven-permissions/marketeer-view.png)

## Vorteile und Überlegungen

Zu den Vorteilen von metadatengesteuerten Berechtigungen zählen:

- Präzise Kontrolle über den Asset-Zugriff basierend auf bestimmten Attributen.
- Entkopplung von Richtlinien zur Zugriffssteuerung von der Ordnerstruktur, wodurch eine flexiblere Asset-Organisation möglich ist.
- Möglichkeit zur Definition komplexer Zugriffssteuerungsregeln basierend auf mehreren Metadateneigenschaften.

>[!NOTE]
>
> Beachten Sie Folgendes:
> 
> - Metadateneigenschaften werden anhand der Einschränkungen mit __Zeichenfolgengleichheit__ (`=`) (andere Datentypen oder Operatoren werden noch nicht unterstützt, für größer als (`>`) oder Datumseigenschaften)
> - Um mehrere Werte für eine Einschränkungseigenschaft zuzulassen, können dem Zugriffssteuerungseintrag zusätzliche Einschränkungen hinzugefügt werden, indem Sie dieselbe Eigenschaft aus der Dropdown-Liste „Typ auswählen“ auswählen und einen neuen Beschränkungswert eingeben (z B. `status=approved`, `status=wip`) und auf „+“ klicken, um die Einschränkung zum Eintrag hinzuzufügen
> ![Zulassen von mehreren Werten](./assets/metadata-driven-permissions/allow-multiple-values.png)
> - __UND-Beschränkungen__ werden über mehrere Einschränkungen in einem einzigen Zugriffskontrolleintrag mit unterschiedlichen Eigenschaftsnamen (z. B. `status=approved`, `brand=Adobe`) wird als AND-Bedingung ausgewertet, d. h. die ausgewählte Benutzergruppe erhält Lesezugriff auf Assets mit `status=approved AND brand=Adobe`
> ![Zulassen von mehreren Einschränkungen](./assets/metadata-driven-permissions/allow-multiple-restrictions.png)
> - __ODER-Beschränkungen__ werden unterstützt, indem ein neuer Zugriffskontrolleintrag mit einer Metadaten-Eigenschaftsbeschränkung hinzugefügt wird, um eine ODER-Bedingung für die Einträge zu erstellen, z. B. einen einzelnen Eintrag mit Einschränkungen `status=approved` und einem einzigen Eintrag mit `brand=Adobe` wird bewertet als `status=approved OR brand=Adobe`
> ![Zulassen von mehreren Einschränkungen](./assets/metadata-driven-permissions/allow-multiple-aces.png)

---
title: Metadatengesteuerte Berechtigungen in AEM Assets
description: Metadatengesteuerte Berechtigungen sind eine Funktion, mit der der Zugriff auf basierend auf Asset-Metadateneigenschaften und nicht auf der Ordnerstruktur eingeschränkt wird.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-13757
thumbnail: xx.jpg
doc-type: Tutorial
exl-id: 57478aa1-c9ab-467c-9de0-54807ae21fb1
source-git-commit: 03cb7ef0cf79a21ec1b96caf6c11e6f5119f777c
workflow-type: tm+mt
source-wordcount: '682'
ht-degree: 1%

---

# Metadatengesteuerte Berechtigungen{#metadata-driven-permissions}

Metadatengesteuerte Berechtigungen sind eine Funktion, mit der Zugriffssteuerungsentscheidungen in der AEM Assets-Autoreninstanz auf der Grundlage von Asset-Metadateneigenschaften und nicht auf der Ordnerstruktur festgelegt werden können. Mit dieser Funktion können Sie Zugriffssteuerungsrichtlinien definieren, die Attribute wie den Asset-Status, den Typ oder eine von Ihnen definierte benutzerdefinierte Metadateneigenschaft auswerten.

Sehen wir uns ein Beispiel an. Kreative Benutzer laden ihre Arbeit in AEM Assets in den Ordner hoch, der mit der Kampagne in Verbindung steht. Möglicherweise handelt es sich um ein laufendes Asset, das noch nicht zur Verwendung freigegeben wurde. Wir möchten sicherstellen, dass Marketing-Experten nur genehmigte Assets für diese Kampagne sehen. Wir können die Metadateneigenschaft verwenden, um anzugeben, dass ein Asset genehmigt wurde und von Marketing-Experten verwendet werden kann.

## Funktionsweise

Die Aktivierung metadatengesteuerter Berechtigungen umfasst die Definition, welche Asset-Metadateneigenschaften Zugriffsbeschränkungen wie &quot;Status&quot;oder &quot;Marke&quot;aktivieren. Diese Eigenschaften können dann verwendet werden, um Zugriffssteuerungseinträge zu erstellen, die angeben, welche Benutzergruppen Zugriff auf Assets mit bestimmten Eigenschaftswerten haben.

## Voraussetzungen

Der Zugriff auf eine AEM as a Cloud Service Umgebung, die auf die neueste Version aktualisiert wurde, ist erforderlich, um metadatengesteuerte Berechtigungen einzurichten.


## Entwicklungsschritte

So implementieren Sie metadatengesteuerte Berechtigungen:

1. Bestimmen Sie, welche Asset-Metadateneigenschaften für die Zugriffskontrolle verwendet werden. In unserem Fall handelt es sich um eine Eigenschaft namens `status`.
1. Erstellen einer OSGi-Konfiguration `com.adobe.cq.dam.assetmetadatarestrictionprovider.impl.DefaultRestrictionProviderConfiguration.cfg.json` in Ihrem Projekt.
1. Fügen Sie die folgende JSON in die erstellte Datei ein.

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


Bevor Sie einschränkungsbasierte Zugriffssteuerungseinträge hinzufügen, sollte ein neuer Eintrag auf oberster Ebene hinzugefügt werden, um zunächst den Lesezugriff für alle Gruppen zu verweigern, die einer Berechtigungsprüfung für Assets unterliegen (z. B. &quot;Mitarbeiter&quot;oder Ähnliches):

1. Navigieren Sie zum Bildschirm Tools > Sicherheit > Berechtigungen .
1. Wählen Sie die Gruppe &quot;Mitwirkende&quot;(oder eine andere benutzerdefinierte Gruppe, zu der alle Benutzergruppen gehören) aus.
1. Klicken Sie oben rechts im Bildschirm auf &quot;ACE hinzufügen&quot;.
1. /content/dam für Pfad auswählen
1. Geben Sie jcr:read für Berechtigungen ein
1. Wählen Sie Ablehnen für Berechtigungstyp aus.
1. Wählen Sie unter Einschränkungen rep:ntNames aus und geben Sie dam:Asset als Beschränkungswert ein.
1. Klicken Sie auf „Speichern“.

![Zugriff verweigern](./assets/metadata-driven-permissions/deny-access.png)

Zugriffssteuerungseinträge können jetzt hinzugefügt werden, um Benutzergruppen Lesezugriff auf der Grundlage von Asset-Metadaten-Eigenschaftswerten zu gewähren.

1. Navigieren Sie zum Bildschirm Tools > Sicherheit > Berechtigungen .
1. Auswählen der gewünschten Gruppe
1. Klicken Sie oben rechts im Bildschirm auf &quot;ACE hinzufügen&quot;.
1. Wählen Sie /content/dam (oder einen Unterordner) für Pfad aus.
1. Geben Sie jcr:read für Berechtigungen ein
1. Wählen Sie Zulassen für Berechtigungstyp aus.
1. Wählen Sie unter Einschränkungen einen der konfigurierten Asset-Metadaten-Eigenschaftsnamen aus (die in der OSGi-Konfiguration definierten Eigenschaften sind hier enthalten).
1. Geben Sie den erforderlichen Metadateneigenschaftswert in das Feld &quot;Restriction Value&quot;ein.
1. Klicken Sie auf das Symbol &quot;+&quot;, um die Einschränkung zum Zugriffskontrolleintrag hinzuzufügen.
1. Klicken Sie auf „Speichern“.

![Zugriff erlauben](./assets/metadata-driven-permissions/allow-access.png)

Beispielordner enthält einige Assets.

![Administratoransicht](./assets/metadata-driven-permissions/admin-view.png)

Nachdem Sie Berechtigungen konfiguriert und die Asset-Metadateneigenschaften entsprechend festgelegt haben, sehen Benutzer (in unserem Fall Marketing-Benutzer) nur genehmigte Assets.

![Marketing-Ansicht](./assets/metadata-driven-permissions/marketeer-view.png)

## Vorteile und Überlegungen

Zu den Vorteilen von metadatengesteuerten Berechtigungen gehören:

- Präzise Kontrolle über den Asset-Zugriff basierend auf bestimmten Attributen.
- Entkopplung von Richtlinien zur Zugriffskontrolle von der Ordnerstruktur, wodurch eine flexiblere Asset-Organisation möglich ist.
- Möglichkeit zur Definition komplexer Zugriffssteuerungsregeln basierend auf mehreren Metadateneigenschaften.

>[!NOTE]
>
> Beachten Sie Folgendes:
> 
> - Metadateneigenschaften werden anhand der Einschränkungen mit Zeichenfolgengleichheit bewertet (andere Datentypen werden noch nicht unterstützt, z. B. Datum)
> - Um mehrere Werte für eine Einschränkungseigenschaft zuzulassen, können dem Zugriffssteuerungseintrag zusätzliche Einschränkungen hinzugefügt werden, indem Sie dieselbe Eigenschaft aus der Dropdown-Liste &quot;Typ auswählen&quot;auswählen und einen neuen Beschränkungswert eingeben (z. B. `status=approved`, `status=wip`) und auf &quot;+&quot;klicken, um die Einschränkung zum Eintrag hinzuzufügen
> ![Mehrere Werte zulassen](./assets/metadata-driven-permissions/allow-multiple-values.png)
> - Mehrere Einschränkungen in einem einzelnen Zugriffssteuerungseintrag mit verschiedenen Eigenschaftsnamen (z. B. `status=approved`, `brand=Adobe`) wird als AND-Bedingung ausgewertet, d. h. die ausgewählte Benutzergruppe erhält Lesezugriff auf Assets mit `status=approved AND brand=Adobe`
> ![Mehrere Einschränkungen zulassen](./assets/metadata-driven-permissions/allow-multiple-restrictions.png)
> - Durch Hinzufügen eines neuen Zugriffssteuerungseintrags mit einer Metadaten-Eigenschaftsbeschränkung wird eine ODER-Bedingung für die Einträge festgelegt, z. B. ein einzelner Eintrag mit Einschränkungen `status=approved` und einem einzigen Eintrag mit `brand=Adobe` wird bewertet als `status=approved OR brand=Adobe`
> ![Mehrere Einschränkungen zulassen](./assets/metadata-driven-permissions/allow-multiple-aces.png)

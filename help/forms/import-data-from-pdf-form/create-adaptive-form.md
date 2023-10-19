---
title: Schema erstellen
description: Erstellen Sie ein Schema basierend auf den Daten, die in das adaptive Formular importiert werden müssen
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 14196
source-git-commit: 17ab178f385619b589a9dde6089410bfa4515ffa
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 20%

---

# Einführung

Der erste Schritt besteht darin, ein Schema zu erstellen, das auf den Daten basiert, die zum Ausfüllen des adaptiven Formulars verwendet werden.

## XFA basiert auf einem Schema

Verwenden Sie das Schema, um Ihr adaptives Formular zu erstellen

## XFA basiert nicht auf einem Schema

* Öffnen Sie die XDP in AEM Forms Designer.
* Klicken Sie auf „Datei auswählen“ > „Formular-Eigenschaften“ > „Vorschau“.
* Klicken Sie auf Vorschaudaten generieren .
* Klicken Sie auf Erzeugen.
* Geben Sie einen aussagekräftigen Dateinamen an, z. B. `form-data.xml`

Sie können eines der kostenlosen Online-Tools verwenden, um aus den im vorherigen Schritt generierten XML-Daten ein [XSD](https://www.freeformatter.com/xsd-generator.html) zu generieren.

Erstellen Sie ein adaptives Formular basierend auf dem Schema aus dem vorherigen Schritt.

>[!NOTE]
>Es wird immer empfohlen, die Daten zu untersuchen, die bei der Übermittlung des adaptiven Formulars generiert werden. Auf diese Weise erhalten Sie eine gute Vorstellung vom XML-Format der Daten, die mit dem adaptiven Formular zusammengeführt werden müssen.

Daten, die aus dem adaptiven Formular übermittelt werden
![sent-data](./assets/af-submitted-data.png)

Aus der PDF exportierte Daten
![exporting-data](./assets/exported-data.png)

Aus den exportierten Daten müssen Sie die **_topmostSubform_** Knoten mit den entsprechenden Namespaces beibehalten, um Daten erfolgreich mit dem adaptiven Formular zusammenzuführen.

## Nächste Schritte

[Erstellen eines OSGi-Dienstes](./create-osgi-service.md)






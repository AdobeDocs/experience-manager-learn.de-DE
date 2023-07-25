---
title: Adaptive Formulardaten in Azure-Speicher speichern
description: Erstellen und konfigurieren Sie ein adaptives Formular zum Speichern von Daten in Azure Storage
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
thumbnail: 335423.jpg
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 2%

---

# Zusammenfassung

Jetzt verfügen wir über alle erforderlichen Konfigurationen/Integrationen, die für den Anwendungsfall erforderlich sind. Der letzte Schritt besteht darin, ein adaptives Formular zu erstellen, das auf dem Formulardatenmodell basiert, das von Azure Storage unterstützt wird.

Erstellen Sie ein adaptives Formular und stellen Sie sicher, dass es auf der richtigen Vorlage für das adaptive Formular basiert. Dadurch wird sichergestellt, dass unser benutzerdefinierter Code, der mit der Vorlage verknüpft ist, jedes Mal ausgeführt wird, wenn ein adaptives Formular wiedergegeben wird.

## Beispiel für ein adaptives Formular

Im Formular haben wir zwei ausgeblendete Felder hinzugefügt

* Blob-ID - Dieses Feld wird mit einer GUID ausgefüllt, wenn das Feld initialisiert wird. Der Wert dieses Felds wird als Blob-Wert verwendet, um die Blob-Speicherung der Formulardaten eindeutig zu identifizieren. Dieser Blob wird zur Identifizierung der Formulardaten verwendet.
* Blob-ID zurückgegeben - Dieses Feld wird mit dem Ergebnis des Dienstaufrufs zum Speichern von Daten in Azure Storage gefüllt. Dieser Wert entspricht der Blob-ID des vorherigen Schritts.

Das Formular hat die folgenden Geschäftsregeln

* Die Schaltfläche Formular speichern wird angezeigt, wenn der Benutzer die E-Mail-Adresse eingibt. Durch Klicken auf die Schaltfläche Formular speichern werden die Formulardaten in Azure Storage unter Verwendung des Aufrufdienst-Vorgangs des Formulardatenmodells gespeichert.
* Die vom Aufrufdienst zurückgegebene BlobID wird im Feld Blob ID gespeichert. Wenn sich dieser Wert ändert, wird dem Antragsteller über SendGrid eine E-Mail gesendet. Die E-Mail enthält den Link zum Öffnen des teilweise ausgefüllten Formulars, das durch die Blob-ID identifiziert wird.
* Dem Benutzer wird ein Bestätigungstext angezeigt, wenn die Daten erfolgreich in Azure Storage gespeichert wurden

### Nächste Schritte

[Bereitstellen der Beispiel-Assets](./deploy-sample-assets.md)


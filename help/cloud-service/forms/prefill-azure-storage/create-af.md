---
title: Speichern von adaptiven Formulardaten in Azure Storage
description: Erstellen und Konfigurieren eines adaptiven Formulars zum Speichern von Daten in Azure Storage
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
thumbnail: 335423.jpg
exl-id: 0b543c6b-9cfd-4fac-b8d0-33153c036f4b
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: ht
source-wordcount: '286'
ht-degree: 100%

---

# Zusammenfassung

Jetzt verfügen wir über alle erforderlichen Konfigurationen/Integrationen, die für den Anwendungsfall erforderlich sind. Der letzte Schritt besteht darin, ein adaptives Formular zu erstellen, das auf dem Formulardatenmodell basiert, das von Azure Storage unterstützt wird.

Erstellen Sie ein adaptives Formular und stellen Sie sicher, dass es auf der richtigen Vorlage für adaptive Formulare basiert. Dadurch wird sichergestellt, dass unser benutzerdefinierter Code, der mit der Vorlage verknüpft ist, jedes Mal ausgeführt wird, wenn ein adaptives Formular wiedergegeben wird.

## Beispiel für ein adaptives Formular

Im Formular haben wir zwei ausgeblendete Felder hinzugefügt

* Blob-ID – Dieses Feld wird mit einer GUID ausgefüllt, wenn das Feld initialisiert wird. Der Wert dieses Felds wird als Blob-ID verwendet, um die Blob-Speicherung der Formulardaten eindeutig zu identifizieren. Diese Blob-ID wird zur Identifizierung der Formulardaten verwendet.
* Blob-ID zurückgegeben – Dieses Feld wird mit dem Ergebnis des Dienstaufrufs zum Speichern von Daten in Azure Storage gefüllt. Dieser Wert entspricht der Blob-ID des vorherigen Schritts.

Das Formular hat die folgenden Geschäftsregeln

* Die Schaltfläche „Formular speichern“ wird angezeigt, wenn die Benutzenden die E-Mail-Adresse eingeben. Durch Klicken auf die Schaltfläche „Formular speichern“ werden die Formulardaten in Azure Storage unter Verwendung des Aufrufdienst-Vorgangs des Formulardatenmodells gespeichert.
* Die vom Aufrufdienst zurückgegebene Blob-ID wird im Feld „Blob-ID“ gespeichert. Wenn sich dieser Wert ändert, wird der Person, die den Antrag gestellt hat, über SendGrid eine E-Mail gesendet. Die E-Mail enthält den Link zum Öffnen des teilweise ausgefüllten Formulars, das durch die Blob-ID identifiziert wird.
* Den Benutzenden wird ein Bestätigungstext angezeigt, wenn die Daten erfolgreich in Azure Storage gespeichert wurden

### Nächste Schritte

[Bereitstellen der Beispiel-Assets](./deploy-sample-assets.md)

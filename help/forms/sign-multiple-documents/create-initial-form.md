---
title: Erstellen des anfänglichen Formulars zum Auslösen des Prozesses
description: Erstellen Sie das anfängliche Formular, um die E-Mail-Benachrichtigung auszulösen und den Signiervorgang zu starten.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
kt: 6892
thumbnail: 6892.jpg
exl-id: d7c55dc8-d886-4629-bb50-d927308d12e3
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: ht
source-wordcount: '177'
ht-degree: 100%

---

# Erstellen des anfänglichen Formulars

Das anfängliche Formular (Refinanzierungsformular) wird zum Signieren mehrerer Formulare verwendet, indem der AEM-Workflow **Mehrere Formulare signieren** ausgelöst wird. Sie können Werte Ihrer Wahl eingeben. Stellen Sie jedoch sicher, dass dem Formular die folgenden Felder hinzugefügt werden.

| Feldtyp | Name | Zweck | Ausgeblendet | Standardwert |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| Textfeld | unterzeichnet | So geben Sie den Signaturstatus an | J | N |
| Textfeld | GUID | Für die eindeutige Identifizierung des Formulars | J | 3889 |
| Textfeld | Kundenname | Fürs Erfassen des Kundennamens | N |
| Textfeld | Kunden-E-Mail | Kunden-E-Mail zum Senden einer Benachrichtigung | N |
| Kontrollkästchen | Zu unterzeichnende Formulare | Die Elemente identifizieren die Formulare im Paket. | N |

Das anfängliche Formular muss so konfiguriert werden, dass ein AEM-Workflow mit dem Namen **signmultipleforms** ausgelöst wird
Stellen Sie sicher, dass der Datendateipfad auf **Data.xml** gesetzt ist. Dies ist sehr wichtig, da der Beispiel-Code in der Payload nach einer Datei namens „Data.xml“ sucht, um die Formularübermittlung zu verarbeiten.

## Assets

Das anfängliche Formular (Refinanzierungsformular) kann [hier heruntergeladen](assets/refinance-form.zip) werden

## Nächste Schritte

[Erstellen von Formularen zum Signieren](./create-forms-for-signing.md)

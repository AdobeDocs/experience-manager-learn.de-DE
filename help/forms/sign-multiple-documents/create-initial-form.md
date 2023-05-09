---
title: Erstellen des Anfangsformulars zum Trigger des Prozesses
description: Erstellen Sie das erste Formular, um die E-Mail-Benachrichtigung Trigger und den Signiervorgang zu starten.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
kt: 6892
thumbnail: 6892.jpg
exl-id: d7c55dc8-d886-4629-bb50-d927308d12e3
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 7%

---

# Erstellen eines anfänglichen Formulars

Das ursprüngliche Formular (Refinance-Formular) wird zum Signieren mehrerer Formulare verwendet, indem das **Mehrere Forms signieren** AEM. Sie können Werte Ihrer Wahl eingeben. Stellen Sie jedoch sicher, dass dem Formular die folgenden Felder hinzugefügt werden.

| Feldtyp | Name | Zweck | Ausgeblendet | Standardwert |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| TextField | signed | So geben Sie den Signaturstatus an | Y | N |
| TextField | guid | Eindeutige Identifizierung des Formulars | Y | 3889 |
| TextField | customerName | Erfassen des Kundennamens | N |
| TextField | customerEmail | Kunden-E-Mail zum Senden einer Benachrichtigung | N |
| Kontrollkästchen | formsToSign | Die Elemente identifizieren die Formulare im Paket | N |

Das anfängliche Formular muss so konfiguriert werden, dass ein AEM Workflow mit dem Namen **signmultipleforms**
Stellen Sie sicher, dass der Datendateipfad auf **Data.xml**. Dies ist sehr wichtig, da der Beispielcode in der Payload nach einer Datei namens Data.xml sucht, in der der Prozess der Formularübermittlung ausgeführt wird.

## Assets

Das ursprüngliche Formular (Refinanzierungsformular) kann [heruntergeladen von hier](assets/refinance-form.zip)

## Nächste Schritte

[Erstellen von Formularen zum Signieren](./create-forms-for-signing.md)

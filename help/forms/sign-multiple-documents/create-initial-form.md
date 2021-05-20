---
title: Erstellen des Anfangsformulars zum Trigger des Prozesses
description: Erstellen Sie das erste Formular, um die E-Mail-Benachrichtigung Trigger und den Signiervorgang zu starten.
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6892
thumbnail: 6892.jpg
topic: Entwicklung
role: Business Practitioner
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 13%

---


# Erstellen eines anfänglichen Formulars

Das anfängliche Formular (Refinance Form) wird zum Signieren mehrerer Formulare verwendet, indem der AEM Workflow **Sign Multiple Forms** ausgelöst wird. Sie können Werte Ihrer Wahl eingeben. Stellen Sie jedoch sicher, dass dem Formular die folgenden Felder hinzugefügt werden.



| Feldtyp | Name | Zweck | Ausgeblendet  | Standardwert |
------------------------|---------------------------------------|--------------------|--------|-----------------
| TextField | signed | So geben Sie den Signaturstatus an | J | N |
| TextField | guid | Eindeutige Identifizierung des Formulars | J | 3889 |
| TextField | customerName | Erfassen des Kundennamens | N |
| TextField | customerEmail | Kunden-E-Mail zum Senden einer Benachrichtigung | N |
| Kontrollkästchen | formsToSign | Die Elemente identifizieren die Formulare im Paket | N |



Das anfängliche Formular muss so konfiguriert werden, dass ein AEM Workflow mit dem Namen **signmultipleforms** Trigger wird
Stellen Sie sicher, dass der Datendateipfad auf **Data.xml** festgelegt ist. Dies ist sehr wichtig, da der Beispielcode in der Payload nach einer Datei namens Data.xml sucht, in der der Prozess der Formularübermittlung ausgeführt wird.

## Assets

Das anfängliche Formular (Refinance Form) kann [von hier heruntergeladen werden](assets/refinance-form.zip)






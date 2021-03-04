---
title: Erstellen Sie das erste Formular, um den Prozess Trigger.
description: Erstellen Sie ein erstes Formular, um die E-Mail-Benachrichtigung an den Beginn des Signiervorgangs Trigger.
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6892
thumbnail: 6892.jpg
topic: Entwicklung
role: Geschäftspraktiker
level: Zwischenschaltung
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 13%

---


# Erstellen eines Ausgangsformulars

Das anfängliche Formular (Refinanzierung des Formulars) wird zum Signieren mehrerer Formulare verwendet, indem der Arbeitsablauf **Mehrere Forms signieren** AEM ausgelöst wird. Sie können Werte Ihrer Wahl eingeben, stellen Sie jedoch sicher, dass folgende Felder zum Formular hinzugefügt werden.



| Feldtyp | Name | Zweck | Ausgeblendet  | Standardwert |
------------------------|---------------------------------------|--------------------|--------|-----------------
| TextField | signed | So geben Sie den Unterschriftsstatus an | J | N |
| TextField | guid | So identifizieren Sie das Formular eindeutig | J | 3889 |
| TextField | customerName | So erfassen Sie den Kundennamen | N |
| TextField | customerEmail | Kunden-E-Mail zum Senden der Benachrichtigung | N |
| CheckBox | formsToSign | Die Elemente identifizieren die Formulare im Paket | N |



Das anfängliche Formular muss so konfiguriert werden, dass ein AEM Arbeitsablauf namens **signmultipleforms** Trigger wird
Stellen Sie sicher, dass der Datendateipfad auf **Data.xml** eingestellt ist. Dies ist sehr wichtig, da der Beispielcode nach einer Datei namens &quot;Data.xml&quot;in der Payload sucht, während der Prozess der Formularübermittlung gesendet wird.

## Assets

Das anfängliche Formular (Refinanzierungsformular) kann [von hier heruntergeladen werden](assets/refinance-form.zip)






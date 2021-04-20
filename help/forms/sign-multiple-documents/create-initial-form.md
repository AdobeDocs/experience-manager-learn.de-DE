---
title: Erstellen Sie das erste Formular, um den Prozess Trigger.
description: Erstellen Sie ein erstes Formular, um die E-Mail-Benachrichtigung an den Beginn des Signiervorgangs Trigger.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6892
thumbnail: 6892.jpg
topic: Development
role: Business Practitioner
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
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
Stellen Sie sicher, dass der Datendateipfad auf **Data.xml** eingestellt ist. Dies ist sehr wichtig, da der Beispielcode nach einer Datei namens &quot;Data.xml&quot;in der Nutzlast des Prozesses der Formularübermittlung sucht.

## Assets

Das anfängliche Formular (Refinanzierungsformular) kann [von hier heruntergeladen werden](assets/refinance-form.zip)






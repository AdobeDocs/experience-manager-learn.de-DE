---
title: Vorbefüllen von auf Kernkomponenten basierenden adaptiven Formularen
description: Erfahren Sie, wie Sie adaptive Formulare mit Daten vorbefüllen
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14675
duration: 23
exl-id: a94deebd-e86e-4360-b0ed-193f13197ee2
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '119'
ht-degree: 100%

---

# Testen der Lösung

Nachdem der Code bereitgestellt wurde, erstellen Sie basierend auf Kernkomponenten ein adaptives Formular. Verknüpfen Sie das adaptive Formular mit dem Vorbefüllungsdienst, wie im Screenshot unten dargestellt.
![prefill-service](assets/pre-fill-service.png)

Jedes Mal, wenn das Formular wiedergegeben wird, wird der zugehörige Vorbefüllungsdienst ausgeführt und das Formular wird mit den vom Vorbefüllungsdienst zurückgegebenen Daten ausgefüllt.

Beispiel: Um das Formular mit den Daten auszufüllen, die mit dem GUID **d815a2b3-5f4c-4422-8197-d0b73479bf0e** verknüpft sind, wird die folgende URL verwendet.
Der Code im Vorbefüllungsdienst extrahiert den Wert des GUID-Parameters und ruft die mit dem GUID verknüpften Daten aus der Datenquelle ab.

```html
http://localhost:4502/content/dam/formsanddocuments/contactus/jcr:content?wcmmode=disabled&guid=d815a2b3-5f4c-4422-8197-d0b73479bf0e
```

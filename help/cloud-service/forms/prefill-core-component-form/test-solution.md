---
title: Vorfüllen von auf Kernkomponenten basierenden adaptiven Formularen
description: Erfahren Sie, wie Sie adaptive Formulare mit Daten vorab ausfüllen
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14675
source-git-commit: d9612adbc2ff3e601c2efe5a779c03ad24769276
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 3%

---

# Testen der Lösung

Nachdem der Code bereitgestellt wurde, erstellen Sie ein adaptives Formular basierend auf Kernkomponenten. Verknüpfen Sie das adaptive Formular mit dem Vorbefüllungs-Dienst, wie im Screenshot unten dargestellt.
![prefill-service](assets/pre-fill-service.png)

Jedes Mal, wenn das Formular wiedergegeben wird, wird der zugehörige Vorbefüllungsdienst ausgeführt und das Formular wird mit den vom Vorbefüllungs-Dienst zurückgegebenen Daten ausgefüllt.

Beispiel: Um das Formular mit den Daten auszufüllen, die mit dem Guide verknüpft sind **d815a2b3-5f4c-4422-8197-d0b73479bf0e**verwendet wird, wird die folgende URL verwendet.
Der Code im Vorbefüllungs-Dienst extrahiert den Wert des guid-Parameters und ruft die mit der GUID verknüpften Daten aus der Datenquelle ab.

```html
http://localhost:4502/content/dam/formsanddocuments/contactus/jcr:content?wcmmode=disabled&guid=d815a2b3-5f4c-4422-8197-d0b73479bf0e
```

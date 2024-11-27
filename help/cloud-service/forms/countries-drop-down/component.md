---
title: Erstellen Sie die Struktur für die Länderkomponente
description: Erstellen der Komponentenstruktur in crx
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
source-git-commit: f9a1fb40aabb6fdc1157e1f2576f9c0d9cf1b099
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 6%

---

# Erstellen Sie die Struktur für die Länderkomponente

Melden Sie sich bei Ihrer AEM Forms-Instanz an und führen Sie die folgenden Schritte aus, um eine neue Komponente basierend auf der vordefinierten Dropdown-Komponente zu erstellen:

1. Navigieren Sie zu &quot;/apps/&lt;Ihr Projekt>/components/adaptiveForm/dropdown&quot;in CRXDE Lite.
2. Kopieren Sie die Dropdown-Komponente und fügen Sie sie auf derselben Ordnerebene ein.
3. Benennen Sie die kopierte Komponente in Länder um.
4. Aktualisieren Sie die Eigenschaft jcr:title des Knotens cq:template auf Countries.
5. Speichern Sie die Änderungen.

Sie haben jetzt eine neue Komponente mit dem Namen Länder , die exakt die vordefinierte Dropdown-Komponente wiedergibt. Dies dient als Grundlage für weitere Anpassungen.

## Erstellen der HTL-Datei

So erstellen Sie die HTL-Datei für die Komponente Länder :

1. Navigieren Sie zum Ordner Länder im CRX-Repository.
2. Erstellen Sie eine neue Datei mit dem Namen countries.html.
3. Öffnen Sie die Datei /apps/core/fd/components/form/dropdown/v1/dropdown/dropdown.html im CRX-Repository und kopieren Sie den Inhalt.
4. Fügen Sie den kopierten Inhalt in &quot;countries.html&quot;ein.
5. Ändern Sie den Code, um das neue Sling-Modell zu verwenden, wie im Screenshot gezeigt.
6. Speichern Sie Ihre Änderungen.

![sling-model](assets/countriesdropdown.png)

Synchronisieren Sie abschließend Ihr Projekt mit diesen Aktualisierungen, um sicherzustellen, dass die Änderungen im CRX-Repository in Ihrem AEM Projekt übernommen werden.


## Nächste Schritte

[cq-dialog erstellen](./dialog.md)

---
title: Festlegen bestimmter Felder als durchsuchbar
description: Ein mehrteiliges Tutorial, das Sie durch die Schritte führt, die beim Abfragen von im Azure-Portal gespeicherten Formularübermittlungen erforderlich sind.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: 1fb7ca83-0ba6-48a3-b3d3-079d0ef89245
duration: 32
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '107'
ht-degree: 100%

---

# Festlegen bestimmter Felder als durchsuchbar

Durchsuchbare Felder in einem Formular beziehen sich normalerweise auf die Felder im Formular, die als Kriterien für die Suche oder Filterung übermittelter Daten verwendet werden können.
Im Rahmen dieses Anwendungsfalls wurden die folgenden Feldtypen erweitert, um sie durchsuchbar zu machen:

* checkboxgroup
* dropdown
* radiobutton

Die Formularautorinnen und Formularautoren können diese Feldtypen wie unten gezeigt als durchsuchbar markieren:
![Durchsuchbares Feld](assets/searchable-fields.png)

Die Felder wurden durch die folgende Struktur erweitert:

![Erweiterte Felder](assets/extend-component.png)

Dies ist der Inhalt der .content.xml-Datei:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
          jcr:primaryType="nt:unstructured"
          jcr:title="Check box group"
          sling:resourceType="cq/gui/components/authoring/dialog">
    <content
            jcr:primaryType="nt:unstructured"
            sling:resourceType="granite/ui/components/coral/foundation/container">
        <items jcr:primaryType="nt:unstructured">
            <tabs
                    jcr:primaryType="nt:unstructured"
                    sling:resourceType="granite/ui/components/coral/foundation/tabs"
                    maximized="{Boolean}false">
                <items jcr:primaryType="nt:unstructured">

                    <properties
                            jcr:primaryType="nt:unstructured"
                            jcr:title="Additional Properties"
                            sling:resourceType="granite/ui/components/coral/foundation/container"
                            margin="{Boolean}true">
                        <items jcr:primaryType="nt:unstructured">
                            <columns
                                    jcr:primaryType="nt:unstructured"
                                    sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns"
                                    margin="{Boolean}true">
                                <items jcr:primaryType="nt:unstructured">
                                    <column
                                            jcr:primaryType="nt:unstructured"
                                            sling:resourceType="granite/ui/components/coral/foundation/container">
                                        <items jcr:primaryType="nt:unstructured">
                                            <Searchable
                                                    jcr:primaryType="nt:unstructured"
                                                    sling:resourceType="granite/ui/components/coral/foundation/form/checkbox"
                                                    emptyText="Want to include in search?"
                                                    fieldDescription="Indicate if you want to use in search"
                                                    text="Want to use this field in query"
                                                    value="{Boolean}true"
                                                    uncheckedValue="{Boolean}false"

                                                    name="./Searchable"
                                                    checked="{Boolean}false"
                                                    required="{Boolean}false"/>


                                        </items>
                                    </column>
                                </items>
                            </columns>
                        </items>
                    </properties>
                </items>
            </tabs>
        </items>
    </content>
</jcr:root>
```

## Nächste Schritte

[Erstellen eines benutzerdefinierten Übermittlungs-Handlers](./part2.md)

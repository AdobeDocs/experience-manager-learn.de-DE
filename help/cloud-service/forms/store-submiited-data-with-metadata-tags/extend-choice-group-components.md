---
title: Tutorial zum Hinzufügen benutzerdefinierter Metadaten-Tags
description: Erfahren Sie, wie Sie adaptive Formulardaten aus dem Azure Storage-Konto speichern und abrufen.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 14501
source-git-commit: b770fc33ee0752911135d1a94144406bad8f295b
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Erweitern von Auswahlgruppenkomponenten

Die Kernkomponenten checkboxGroup, Dropdown und radiobutton wurden um die Registerkarte Zusätzliche Eigenschaften erweitert. Die Registerkarte &quot;Zusätzliche Eigenschaften&quot;verfügt über ein Kontrollkästchen, mit dem angegeben wird, ob das Feld als Blob-Index-Registerkarte verwendet werden soll.
![additonal-properties](assets/drop-down-additonal-properties.png). Wenn das Kontrollkästchen aktiviert wird, wird eine Eigenschaft mit dem Namen Searchable erstellt und ihr Wert im jcr-Repository auf true gesetzt, wie im folgenden Screenshot gezeigt
![durchsuchbar](assets/searchable-true.png).

Die folgende .content.xml wurde im Ordner _cq_dialog erstellt.

![Dropdown-Projekt-Ansicht](assets/drop-down-project-view.png)

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

[Erstellen der Azure Portal-Konfiguration](./create-osgi-configuration.md)




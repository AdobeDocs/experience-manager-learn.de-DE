---
title: Erstellen des Dialogfelds für die Länderkomponente
description: Erstellen des Dialogfelds für die Komponente
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: e1e5745e-96fb-46c6-aa7f-43cdf2dfddbc
source-git-commit: a3e29b1e02fe3664af1e5718a8a0a48b2307c7de
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 100%

---

# Erstellen des Dialogfelds für die Länderkomponente

Die Länderkomponente übernimmt die Dialogstruktur der Dropdown-Komponente, führt jedoch eine neue Eigenschaft namens „Kontinent“ ein. Darüber hinaus wird das Dialogfeld angepasst, um bestimmte von der Dropdown-Komponente übernommene Felder auszublenden und es gleichzeitig Autorinnen und Autoren zu ermöglichen, den gewünschten Kontinent auszuwählen.

Die einfachste Möglichkeit, dieses Dialogfeld zu erstellen, ist die folgende:

1. Erstellen Sie in Ihrem AEM-Projekt einen Ordner mit dem Namen „_cq_dialog“ unter dem Länderkomponentenordner.
2. Erstellen Sie im Ordner „_cq_dialog“ eine Datei mit dem Namen „.content.xml“.
3. Fügen Sie den unten angegebenen XML-Code in diese Datei ein.
4. Speichern Sie Ihre Änderungen und synchronisieren Sie Ihr Projekt mit AEM.

Dadurch wird die Dialogfeldkonfiguration für die Länderkomponente hinzugefügt.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    jcr:primaryType="nt:unstructured"
    jcr:title="Country of Residence"
    sling:resourceType="cq/gui/components/authoring/dialog">
    <content
        jcr:primaryType="nt:unstructured"
        sling:resourceType="granite/ui/components/coral/foundation/container">
        <items jcr:primaryType="nt:unstructured">
            <tabs
                jcr:primaryType="nt:unstructured"
                sling:resourceType="granite/ui/components/coral/foundation/tabs"
                maximized="true">
                <items jcr:primaryType="nt:unstructured">
                    <basic
                        jcr:primaryType="nt:unstructured"
                        jcr:title="Basic"
                        sling:resourceType="granite/ui/components/coral/foundation/container">
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
                                            <saveValueType
                                                granite:class="cmp-adaptiveform-dropdown__savevaluetype"
                                                jcr:primaryType="nt:unstructured"
                                                sling:orderBefore="placeholder"
                                                sling:resourceType="granite/ui/components/coral/foundation/form/select"
                                                fieldLabel="Save value as"
                                                name="./typeIndex"
                                                typeHint="String">
                                                <items jcr:primaryType="nt:unstructured">
                                                    <String
                                                        jcr:primaryType="nt:unstructured"
                                                        text="String"
                                                        value="0"/>
                                                    <Number
                                                        jcr:primaryType="nt:unstructured"
                                                        text="Number"
                                                        value="1"/>
                                                    <Boolean
                                                        jcr:primaryType="nt:unstructured"
                                                        text="Boolean"
                                                        value="2"/>
                                                </items>
                                            </saveValueType>
                                            <type
                                                jcr:primaryType="nt:unstructured"
                                                sling:resourceType="granite/ui/components/coral/foundation/form/hidden"
                                                name="./type"/>
                                            <enums
                                                jcr:primaryType="nt:unstructured"
                                                sling:hideResource="{Boolean}true"
                                                sling:orderBefore="bindref"
                                                sling:resourceType="granite/ui/components/coral/foundation/container">
                                                <items jcr:primaryType="nt:unstructured">
                                                    <enumOptions
                                                        jcr:primaryType="nt:unstructured"
                                                        sling:orderBefore="bindref"
                                                        sling:resourceType="granite/ui/components/coral/foundation/form/multifield"
                                                        fieldDescription="Provide the pair of enum (data value) and enumName (display text) for each option."
                                                        fieldLabel="Options">
                                                        <field
                                                            jcr:primaryType="nt:unstructured"
                                                            sling:resourceType="granite/ui/components/coral/foundation/container"
                                                            name="./enum">
                                                            <items jcr:primaryType="nt:unstructured">
                                                                <enum
                                                                    granite:class="cmp-adaptiveform-base__enum"
                                                                    jcr:primaryType="nt:unstructured"
                                                                    sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                                    fieldDescription="Specify the content to submit, when the option is selected."
                                                                    fieldLabel="Data value"
                                                                    name="./enum"
                                                                    required="{Boolean}true"/>
                                                                <enumNames
                                                                    granite:class="cmp-adaptiveform-base__enumNames"
                                                                    jcr:primaryType="nt:unstructured"
                                                                    sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                                    fieldDescription="Specify the content to display in the adaptive form."
                                                                    fieldLabel="Display text"
                                                                    name="./enumNames"
                                                                    required="{Boolean}false"/>
                                                                <richTextEnumNames
                                                                    jcr:primaryType="nt:unstructured"
                                                                    sling:hideResource="{Boolean}true"/>
                                                            </items>
                                                        </field>
                                                    </enumOptions>
                                                    <enumNames
                                                        granite:hidden="true"
                                                        jcr:primaryType="nt:unstructured"
                                                        sling:resourceType="granite/ui/components/coral/foundation/form/multifield">
                                                        <field
                                                            granite:class="cmp-adaptiveform-base__enumNamesHidden"
                                                            granite:hidden="true"
                                                            jcr:primaryType="nt:unstructured"
                                                            sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                            name="./enumNames"/>
                                                    </enumNames>
                                                    <areOptionsRichText
                                                        jcr:primaryType="nt:unstructured"
                                                        sling:hideResource="true"/>
                                                </items>
                                            </enums>
                                            <multiSelect
                                                granite:class="cmp-adaptiveform-dropdown__allowmultiselect"
                                                jcr:primaryType="nt:unstructured"
                                                sling:orderBefore="saveValueType"
                                                sling:resourceType="granite/ui/components/coral/foundation/form/checkbox"
                                                name="./multiSelect"
                                                text="Allow multiple selection"
                                                value="true"/>
                                            <multiSelect-typehint
                                                jcr:primaryType="nt:unstructured"
                                                sling:resourceType="granite/ui/components/coral/foundation/form/hidden"
                                                name="./multiSelect@TypeHint"
                                                value="Boolean"/>
                                            <defaultValueSS
                                                jcr:primaryType="nt:unstructured"
                                                sling:orderBefore="placeholder"
                                                sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                fieldDescription="Specified default option is preselected on form load."
                                                fieldLabel="Default option"
                                                name="./default"
                                                wrapperClass="cmp-adaptiveform-dropdown__defaultvalue"/>
                                            <defaultValueMS
                                                jcr:primaryType="nt:unstructured"
                                                sling:orderBefore="placeholder"
                                                sling:resourceType="granite/ui/components/coral/foundation/form/multifield"
                                                fieldDescription="Specified default options are preselected on form load."
                                                fieldLabel="Default options"
                                                typeHint="String[]"
                                                wrapperClass="cmp-adaptiveform-dropdown__defaultvaluemultiselect">
                                                <field
                                                    jcr:primaryType="nt:unstructured"
                                                    sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                    name="./default"
                                                    required="{Boolean}false"/>
                                            </defaultValueMS>
                                            <continent
                                                jcr:primaryType="nt:unstructured"
                                                sling:resourceType="granite/ui/components/coral/foundation/form/select"
                                                fieldLabel="Select Continent"
                                                name="./continent"
                                                required="true">
                                                <items jcr:primaryType="nt:unstructured">
                                                    <northAmerica
                                                        jcr:primaryType="nt:unstructured"
                                                        text="North America"
                                                        value="northamerica"/>
                                                    <europe
                                                        jcr:primaryType="nt:unstructured"
                                                        text="Europe"
                                                        value="europe"/>
                                                    <asia
                                                        jcr:primaryType="nt:unstructured"
                                                        text="Asia"
                                                        value="asia"/>
                                                    <africa
                                                        jcr:primaryType="nt:unstructured"
                                                        text="Africa"
                                                        value="africa"/>
                                                </items>
                                            </continent>
                                        </items>
                                    </column>
                                </items>
                            </columns>
                        </items>
                    </basic>
                </items>
            </tabs>
        </items>
    </content>
</jcr:root>
```

## Nächste Schritte

[Erstellen eines Sling-Modells.](./slingmodel.md)

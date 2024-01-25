---
title: Tutorial zum Hinzufügen benutzerdefinierter Metadaten-Tags
description: Erstellen Sie eine benutzerdefinierte Übermittlung, um Formulardaten mit Metadaten-Tags in Azure zu speichern.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14501
duration: 54
exl-id: eb9bcd1b-c86f-4894-bcf8-9c17e74dd6ec
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '115'
ht-degree: 1%

---

# Blob-Index-Tags hinzufügen

Wenn Datensätze größer werden, kann es schwierig sein, ein bestimmtes Objekt in einem Meer von Daten zu finden. Blob-Index-Tags bieten Datenmanagement- und Erkennungsfunktionen durch Verwendung von Schlüssel-Wert-Index-Tag-Attributen. Sie können Objekte in einem einzelnen Container oder über alle Container in Ihrem Speicherkonto kategorisieren und suchen. Beispiel: Blob-Index-Tag _**CustomerType=Platinum**_, wobei Platinum der Wert des Felds CustomerType ist.

![index-tags](assets/blob-with-index-tags1.png)
Der folgende Code erstellt die Blob-Index-Daten-Tag-Zeichenfolge mit den entsprechenden Werten aus den gesendeten Daten

```java
@Override
    public String getMetaDataTags(String submittedFormName,String formPath,Session session,String formData) {

        JsonObject jsonObject = JsonParser.parseString(formData).getAsJsonObject();
        List<String>metaDataTags = new ArrayList<String>();
        metaDataTags.add("formName="+submittedFormName);
        Map< String, String > map = new HashMap< String, String >();
        map.put("path", formPath);
        map.put("1_property", "Searchable");
        map.put("1_property.value","true");
        Query query = queryBuilder.createQuery(PredicateGroup.create(map),session);
        query.setStart(0);
        query.setHitsPerPage(20);
        SearchResult result = query.getResult();
        logger.debug("Get result hits " + result.getHits().size());
        for (Hit hit: result.getHits()) {
            try {
                    logger.debug(hit.getPath());
                    String jsonElementName = (String) hit.getProperties().get("name");
                    String fieldName = hit.getProperties().get("name").toString();
                    if(jsonObject.get(jsonElementName).isJsonArray())
                    {
                        
                        JsonArray arrayOfValues = jsonObject.get(jsonElementName).getAsJsonArray();
                        StringBuilder valuesString = new StringBuilder();
                        for(int j=0;j<arrayOfValues.size();j++)
                        {
                            valuesString.append(arrayOfValues.get(j).getAsString());
                            if(j < arrayOfValues.size() -1)
                            {
                                valuesString.append(" and ");
                            }
                        }

                        
                        metaDataTags.add(fieldName + "=" + valuesString.toString());

                    }
                    else
                    {
                        logger.debug("The searchable field name is " + fieldName + "the json element name is " + jsonElementName);
                        metaDataTags.add(fieldName + "=" + jsonObject.get(jsonElementName).getAsString());
                    }

            } catch (RepositoryException e) {
                throw new RuntimeException(e);
            }
        }
        return String.join("&",metaDataTags);
    }
```

## Nächste Schritte

[Benutzerdefinierten Sende-Handler erstellen](./create-custom-submit.md)

---
title: Tutorial zum Hinzufügen der von Benutzenden angegebenen Metadaten-Tags
description: Erstellen Sie eine benutzerdefinierte Übermittlung, um die Formulardaten mit Metadaten-Tags in Azure zu speichern
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-14501
duration: 43
exl-id: eb9bcd1b-c86f-4894-bcf8-9c17e74dd6ec
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '115'
ht-degree: 100%

---

# Hinzufügen von Blob-Index-Tags

Je umfangreicher Datensätze werden, desto schwieriger kann es sein, ein bestimmtes Objekt in den vielen Daten zu finden. Blob-Index-Tags bieten Funktionen für Daten-Management und -suche durch die Verwendung von Schlüsselwert-Index-Tag-Attributen. Sie können Objekte innerhalb eines einzelnen Containers oder in allen Containern in Ihrem Speicherkonto kategorisieren und suchen. Beispiel: das Blob-Index-Tag _**CustomerType=Platinum**_, wobei „Platinum“ der Wert des Feldes „CustomerType“ ist.

![index-tags](assets/blob-with-index-tags1.png)
Der folgende Code erstellt die Zeichenfolge der Blob-Indexdaten-Tags mit den entsprechenden Werten aus den übermittelten Daten

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

[Erstellen eines benutzerdefinierten Übermittlungs-Handlers](./create-custom-submit.md)

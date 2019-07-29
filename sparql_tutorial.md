
```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX efrbroo: <http://erlangen-crm.org/efrbroo/>
PREFIX ecrm: <http://erlangen-crm.org/current/>
PREFIX mmms: <http://ldf.fi/mmm/schema/>

SELECT * WHERE {
  BIND (<http://ldf.fi/mmm/place/tgn_7008038> AS ?place_uri)
  ?manuscript a efrbroo:F4_Manifestation_Singleton.
  ?event_uri ecrm:P30_transferred_custody_of|mmms:observed_manuscript ?manuscript ;
    ecrm:P7_took_place_at ?place_uri .
  ?place_uri skos:prefLabel ?place_label .
}
```

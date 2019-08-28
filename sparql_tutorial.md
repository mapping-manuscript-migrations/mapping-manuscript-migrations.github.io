## SPARQL tutorials

Refer to the [MMM Data Model](data_model/mmm.md) and the [MMM Schema](data_model/mmm-schema.md) for more information about the classes and properties used in MMM. Click on the YASGUI link at the end of each query description to open that query in [yasgui.org](http://yasgui.org), where you can query the MMM triplestore.

#### Provenance events in specific places

These sets of queries return data related to manuscripts (manifestation singletons) and their associated provenance events.

------

**Query 1: Find manuscripts, their provenance events, and the places where those events occurred.**

After defining our manuscript variable as a manifestation singleton (efrbroo:F4_Manifestation_Singleton), we then find the provenance events associated with those manuscripts. Provenance events in MMM can refer to either transactions (ecrm:P30_transferred_custody_of), or simply the ownership of a manuscript (mmms:observed_manuscript). If the location where that event occurred is known, the provenance event will link to that place via the ecrm:P7_took_place_at property.

[http://yasgui.org/short/B6U1RkSbP](http://yasgui.org/short/B6U1RkSbP)

```sql
SELECT * WHERE {
  ?manuscript a efrbroo:F4_Manifestation_Singleton.
  ?event_uri ecrm:P30_transferred_custody_of|mmms:observed_manuscript ?manuscript ;
    ecrm:P7_took_place_at ?place_uri .
  ?place_uri skos:prefLabel ?place_label .
}
```
------

**Query 1.1: Find manuscripts with provenance events that occurred in a specific place.**

Modify the Query 1 with a BIND clause, mandating a specific URI for your place.  Use the URI provided by the [Getty Thesaurus of Geographic Names](http://www.getty.edu/research/tools/vocabularies/tgn/index.html), which is the geographic hierarchy used in MMM. The query below limits the results to manifestation singletons that had provenance events that occurred in Paris.

[http://yasgui.org/short/dBZ4IqLv7](http://yasgui.org/short/dBZ4IqLv7)

```sql
SELECT * WHERE {
  BIND (<http://ldf.fi/mmm/place/tgn_7008038> AS ?place_uri)
  ?manuscript a efrbroo:F4_Manifestation_Singleton.
  ?event_uri ecrm:P30_transferred_custody_of|mmms:observed_manuscript ?manuscript ;
    ecrm:P7_took_place_at ?place_uri .
  ?place_uri skos:prefLabel ?place_label .
}
```
------

**Query 1.2: Find manuscripts with provenance events that occurred in multiple different locations.**

Use a VALUES clause to specify each location's uri as the desired values for the ?place_uri variable in your query. The query below returns provenance events that occurred in Paris or Rouen.

[http://yasgui.org/short/0zH7QGjuG](http://yasgui.org/short/0zH7QGjuG)
```sql
SELECT * WHERE {
  VALUES ?place_uri {<http://ldf.fi/mmm/place/tgn_7008929> <http://ldf.fi/mmm/place/tgn_7008038>}
  ?event_uri ecrm:P7_took_place_at ?place_uri ;
    ecrm:P30_transferred_custody_of|mmms:observed_manuscript ?manuscript .
  ?place_uri skos:prefLabel ?place_label .
  ?manuscript a efrbroo:F4_Manifestation_Singleton.
}
```
------

**Query 1.3: Find manuscripts with provenance events that occurred in a general region.**

To find all provenance events that occurred within a broader region or country, use the ```gvp:broaderPreferred*``` predicate, with the desired region's uri as its object. By adding the asterisk to the end of this predicate, the query will include results not just for the place specified, but also any location within that place, according to the Getty Thesaurus of Geographic Names hierarchy. The query below returns provenance events that took place in France.

[http://yasgui.org/short/647ASfkms](http://yasgui.org/short/647ASfkms)
```sql
SELECT * WHERE {    
  ?place_uri gvp:broaderPreferred* <http://ldf.fi/mmm/place/tgn_1000070> .
  ?event_uri ecrm:P7_took_place_at ?place_uri ;
    ecrm:P30_transferred_custody_of|mmms:observed_manuscript ?manuscript .
  ?place_uri skos:prefLabel ?place_label .
  ?manuscript a efrbroo:F4_Manifestation_Singleton.
}
```
------

**Query 1.4: Count the total number of entities that appear in the results of Query 1.3.**

To count the total number of entities that occurred in France, use the same query as Query 1.3, but edit the SELECT statement to include the COUNT aggregate function.

[http://yasgui.org/short/lJkT4mpHg](http://yasgui.org/short/lJkT4mpHg)
```sql
SELECT COUNT (*) WHERE {
  ?place_uri gvp:broaderPreferred* <http://ldf.fi/mmm/place/tgn_1000070> .
  ?event_uri ecrm:P7_took_place_at ?place_uri ;
    ecrm:P30_transferred_custody_of|mmms:observed_manuscript ?manuscript .
  ?place_uri skos:prefLabel ?place_label .
  ?manuscript a efrbroo:F4_Manifestation_Singleton.
}
```
------

**Query 1.5: Count the total number of provenance events that occured in France.**

To count only unique provenance events that occurred in France, use the same query as Query 1.4, but add the DISTINCT modifier to the COUNT function, and specify that the ?event_uri variable is the variable you want to count.

[http://yasgui.org/short/eZ_9E6fi5](http://yasgui.org/short/eZ_9E6fi5)
```sql
SELECT COUNT (DISTINCT ?event_uri) WHERE {
  ?place_uri gvp:broaderPreferred* <http://ldf.fi/mmm/place/tgn_1000070> .
  ?event_uri ecrm:P7_took_place_at ?place_uri ;
    ecrm:P30_transferred_custody_of|mmms:observed_manuscript ?manuscript .
  ?place_uri skos:prefLabel ?place_label .
  ?manuscript a efrbroo:F4_Manifestation_Singleton.
}
```
------

**Query 1.6: Count the total number of manuscripts that had a provenance event that occurred in France.**

To count the number of manuscripts that had a provenance event that occurred in France, change the variable in the SELECT statement in Query 1.5 from ?event_uri to ?manuscript. A single manuscript may have many different provenance events in the same location over the course of its history, making the DISTINCT modifier particularly important in this case.

[http://yasgui.org/short/ZorfClIqZ](http://yasgui.org/short/ZorfClIqZ)
```sql
SELECT COUNT (DISTINCT ?manuscript) WHERE {
  ?place_uri gvp:broaderPreferred* <http://ldf.fi/mmm/place/tgn_1000070> .
  ?event_uri ecrm:P7_took_place_at ?place_uri ;
    ecrm:P30_transferred_custody_of|mmms:observed_manuscript ?manuscript .
  ?place_uri skos:prefLabel ?place_label .
  ?manuscript a efrbroo:F4_Manifestation_Singleton.
}
```

------
#### Actor queries
These sets of queries relate to Actors: persons or institutions who produced, bought, or sold manuscripts (manifestation singletons).

**Query 2: Find owners of manuscripts**

To find the names of Actors who owned manuscripts, use the query below. Manuscripts (manifestation singletons) link to their owners via the ```ecrm:P51_has_former_or_current_owner``` predicate. MMM uses ```skos:prefLabel``` to link human-readable labels to owners.

[http://yasgui.org/short/LwjrUN1b8](http://yasgui.org/short/LwjrUN1b8)

```sql
SELECT ?manuscript ?owner_label WHERE {
  ?manuscript a efrbroo:F4_Manifestation_Singleton ;
	ecrm:P51_has_former_or_current_owner ?owner .
  ?owner skos:prefLabel ?owner_label.

}
```
------
**Query 2.1: Find a specific Actor's uri**

To find a specific Actor's uri, modify Query 2 with a FILTER clause that includes the CONTAINS function. The query below searches for the uri of an entity that has the string "Phillipps" in its preferred label. This returns the uri for Sir Thomas Phillipps, an important manuscript collector of the 19th century. To search for a different Actor, edit the string within the quotation marks of the CONTAINS function.

[http://yasgui.org/short/Cal0-QKZg](http://yasgui.org/short/Cal0-QKZg)

```sql
SELECT ?manuscript ?owner ?owner_label WHERE {
  ?manuscript a efrbroo:F4_Manifestation_Singleton ;
	ecrm:P51_has_former_or_current_owner ?owner .
  ?owner skos:prefLabel ?owner_label.
  FILTER (CONTAINS (?owner_label,"Phillipps"))
}
```
------
**Query 2.2: Find manuscripts owned by Sir Thomas Phillipps**

We found Sir Thomas Phillipps' uri in Query 2.1. Now we can use that uri to find all of the manuscripts (manifestation singletons) he owned. Modify Query 2 using the VALUES clause to specify that the ?owner variable must link to Sir Thomas Phillipps' uri.

[http://yasgui.org/short/4OdGumVNL](http://yasgui.org/short/4OdGumVNL)
```sql
SELECT ?manuscript ?owner ?owner_label WHERE {
  VALUES ?owner { <http://ldf.fi/mmm/actor/bodley_person_73979081> }
  ?manuscript a efrbroo:F4_Manifestation_Singleton ;
	ecrm:P51_has_former_or_current_owner ?owner .
  ?owner skos:prefLabel ?owner_label.
}
```
------
**Query 2.3: Find manuscripts owned by Sir Thomas Phillipps and Chester Beatty**

You can search for manuscripts owned by more than one Actor by including multiple VALUE clauses in your query. The query below modifies Query 2.2 to return manuscripts (manifestation singletons) owned by both Sir Thomas Phillipps and Chester Beatty. We first created two separate VALUE clauses for our two separate owners (adding Chester Beatty as ?owner2). We then added ?owner2 as an object of our ?manuscript variable via the same ```ecrm:P51_has_former_or_current_owner``` predicate.

[http://yasgui.org/short/Qxo3zPUd7](http://yasgui.org/short/Qxo3zPUd7)
```sql
SELECT ?manuscript ?manuscript_label ?owner ?owner_label WHERE {
  VALUES ?owner { <http://ldf.fi/mmm/actor/bodley_person_73979081> }
	VALUES ?owner2 { <http://ldf.fi/mmm/actor/bibale_6119> }
  ?manuscript a efrbroo:F4_Manifestation_Singleton ;
              skos:prefLabel ?manuscript_label ;
	ecrm:P51_has_former_or_current_owner ?owner ;
	ecrm:P51_has_former_or_current_owner ?owner2 .
  ?owner skos:prefLabel ?owner_label.
}
```
------

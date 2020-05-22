<script src="https://unpkg.com/vanilla-back-to-top@7.2.1/dist/vanilla-back-to-top.min.js"></script>
<script>addBackToTop({
  diameter: 56,
  backgroundColor: 'rgb(103, 58, 183)',
  textColor: '#fff'
})</script>


## SPARQL tutorials
The queries described here provide the building blocks you will need to explore the MMM dataset using SPARQL.

Refer to the [MMM Data Model](/mapping-manuscript-migrations.github.io/data_model/MMMdatamodel.pdf) and the [MMM Schema](/mapping-manuscript-migrations.github.io/data_model/mmm-schema) for more information about the classes and properties used in MMM. Click on the YASGUI link at the end of each query description to open that query in [yasgui.org](https://yasgui.triply.cc/), where you can run the query yourself against the MMM triplestore.

<br>

#### Provenance event queries

Use these queries to find data about manuscripts and their associated provenance events.

------

**Query 1: Find manuscripts**

To build a query about manuscripts, you must define what a manuscript is. In MMM, we've followed the FRBRoo model and defined manuscripts as Manifestation Singletons. It is beyond the scope of this tutorial to explain the full implications of what this means, but in brief: in the FRBR hierarchical world of Work -> Expression -> Manifestation -> Item, a Manifestation Singleton is both a unique manifestation and a unique item combined into one object. You can learn more about FRBRoo [here](https://www.ifla.org/files/assets/cataloguing/FRBRoo/frbroo_v_2.4.pdf). The good news is that you don't really need to understand FRBR to successfully query MMM data. Just remember that most data related to manuscripts maps to the class (```efrbroo:F4_Manifestation_Singleton```), and include the following statement in any query you build about manuscripts.

In the following queries, we'll call the manuscript variable ```?manuscript```, but you could call it ```?manifestation_singleton```, or ```very_old_book```, or whatever you like. Variable names don't matter, but everything else in the query does. This includes punctuation and capitalization.

We'll use a shortcut to connect our variable with our object, ```a```. This is a shorthand for the RDF type predicate. Essentially what this statement says is: a manuscript is a Manifestation Singleton. In our SPARQL query, that statement looks like this:

```?manuscript``` a ```efrbroo:F4_Manifestation_Singleton .```

[https://api.triplydb.com/s/nZwMIDBR](https://api.triplydb.com/s/nZwMIDBR)

```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX efrbroo: <http://erlangen-crm.org/efrbroo/>

SELECT * WHERE {
  ?manuscript a efrbroo:F4_Manifestation_Singleton .
}
LIMIT 10
```
<br>
------

**Query 1.2 Find provenance events and the places associated with them**

Now that we've successfully found manuscripts, we can find provenance events associated with them. Provenance events in MMM can refer to either transactions (```ecrm:P30_transferred_custody_of```), or simply the ownership of a manuscript (```mmms:observed_manuscript```). If the location where that event occurred is known, the provenance event will link to that place via the ```ecrm:P7_took_place_at``` property.

[https://api.triplydb.com/s/6H_EHcba](https://api.triplydb.com/s/6H_EHcba)

```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX efrbroo: <http://erlangen-crm.org/efrbroo/>
PREFIX ecrm: <http://erlangen-crm.org/current/>
PREFIX mmms: <http://ldf.fi/mmm/schema/>

SELECT * WHERE {
  ?manuscript a efrbroo:F4_Manifestation_Singleton.
  ?event_uri ecrm:P30_transferred_custody_of|mmms:observed_manuscript ?manuscript ;
              ecrm:P7_took_place_at ?place_uri .
  ?place_uri skos:prefLabel ?place_label .
}
LIMIT 10
```
<br>
------

**Query 1.3: Find manuscripts with provenance events that occurred in a specific place**

Modify Query 1.2 with a BIND clause, mandating a specific URI for your place. The query below limits the results to manifestation singletons with provenance events that occurred in Paris. To build a query using a different place, change the ID number at the end of the URI with any other Getty ID number. You can look up Getty ID numbers at the [Getty Thesaurus of Geographic Names](http://www.getty.edu/research/tools/vocabularies/tgn/index.html).

[https://api.triplydb.com/s/yeGSQFqR](https://api.triplydb.com/s/yeGSQFqR)

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
LIMIT 10
```  
<br>
------

**Query 1.4: Find manuscripts with provenance events that occurred in multiple different locations**

Use a ```VALUES``` clause to specify each location's uri as the desired values for the ```?place_uri``` variable in your query. The query below returns provenance events that occurred in Paris or Rouen.

[https://api.triplydb.com/s/vQuQX43Q](https://api.triplydb.com/s/vQuQX43Q)
```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX efrbroo: <http://erlangen-crm.org/efrbroo/>
PREFIX ecrm: <http://erlangen-crm.org/current/>
PREFIX mmms: <http://ldf.fi/mmm/schema/>

SELECT * WHERE {
  VALUES ?place_uri {<http://ldf.fi/mmm/place/tgn_7008929> <http://ldf.fi/mmm/place/tgn_7008038>}
  ?event_uri ecrm:P7_took_place_at ?place_uri ;
              ecrm:P30_transferred_custody_of|mmms:observed_manuscript ?manuscript .
  ?place_uri skos:prefLabel ?place_label .
  ?manuscript a efrbroo:F4_Manifestation_Singleton.
}
LIMIT 100
```
<br>
------

**Query 1.5: Find manuscripts with provenance events that occurred in a general region**

To find all provenance events that occurred within a broader region or country, use the ```gvp:broaderPreferred*``` predicate, with the desired region's uri as its object. By adding the asterisk to the end of this predicate, the query will include results not just for the place specified, but also any location within that place, according to the Getty Thesaurus of Geographic Names hierarchy. The query below returns provenance events that took place in France.

[https://api.triplydb.com/s/Cr8csSSo](https://api.triplydb.com/s/Cr8csSSo)
```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX efrbroo: <http://erlangen-crm.org/efrbroo/>
PREFIX ecrm: <http://erlangen-crm.org/current/>
PREFIX mmms: <http://ldf.fi/mmm/schema/>
PREFIX gvp: <http://vocab.getty.edu/ontology#>

SELECT * WHERE {    
  ?place_uri gvp:broaderPreferred* <http://ldf.fi/mmm/place/tgn_1000070> .
  ?event_uri ecrm:P7_took_place_at ?place_uri ;
              ecrm:P30_transferred_custody_of|mmms:observed_manuscript ?manuscript .
  ?place_uri skos:prefLabel ?place_label .
  ?manuscript a efrbroo:F4_Manifestation_Singleton.
}
LIMIT 500
```
<br>
------

**Query 1.6: Count the total number of entities that appear in the results of Query 1.3**

To count the total number of entities that occurred in France, use the same query as Query 1.3, but edit the ```SELECT``` statement to include the ```COUNT``` aggregate function.

[https://api.triplydb.com/s/5Ed0HVVH](https://api.triplydb.com/s/5Ed0HVVH)
```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX efrbroo: <http://erlangen-crm.org/efrbroo/>
PREFIX ecrm: <http://erlangen-crm.org/current/>
PREFIX mmms: <http://ldf.fi/mmm/schema/>
PREFIX gvp: <http://vocab.getty.edu/ontology#>

SELECT COUNT (*) WHERE {
  ?place_uri gvp:broaderPreferred* <http://ldf.fi/mmm/place/tgn_1000070> .
  ?event_uri ecrm:P7_took_place_at ?place_uri ;
    ecrm:P30_transferred_custody_of|mmms:observed_manuscript ?manuscript .
  ?place_uri skos:prefLabel ?place_label .
  ?manuscript a efrbroo:F4_Manifestation_Singleton.
}
```
<br>
------

**Query 1.7: Count the total number of provenance events that occured in France**

To count only unique provenance events that occurred in France, use the same query as Query 1.4, but add the ```DISTINCT``` modifier to the ```COUNT``` function, and specify that the ```?event_uri``` variable is the variable you want to count.

[https://api.triplydb.com/s/1ft7tdAf](https://api.triplydb.com/s/1ft7tdAf)
```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX efrbroo: <http://erlangen-crm.org/efrbroo/>
PREFIX ecrm: <http://erlangen-crm.org/current/>
PREFIX mmms: <http://ldf.fi/mmm/schema/>
PREFIX gvp: <http://vocab.getty.edu/ontology#>

SELECT COUNT (DISTINCT ?event_uri) WHERE {
  ?place_uri gvp:broaderPreferred* <http://ldf.fi/mmm/place/tgn_1000070> .
  ?event_uri ecrm:P7_took_place_at ?place_uri ;
    ecrm:P30_transferred_custody_of|mmms:observed_manuscript ?manuscript .
  ?place_uri skos:prefLabel ?place_label .
  ?manuscript a efrbroo:F4_Manifestation_Singleton.
}
```
<br>
------

**Query 1.8: Count the total number of manuscripts that had a provenance event that occurred in France**

To count the number of manuscripts that had a provenance event that occurred in France, change the variable in the ```SELECT``` statement in Query 1.5 from ```?event_uri``` to ```?manuscript```. A single manuscript may have many different provenance events in the same location over the course of its history, making the ```DISTINCT``` modifier particularly important in this case.

[https://api.triplydb.com/s/yo0wZHE3](https://api.triplydb.com/s/yo0wZHE3)
```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX efrbroo: <http://erlangen-crm.org/efrbroo/>
PREFIX ecrm: <http://erlangen-crm.org/current/>
PREFIX mmms: <http://ldf.fi/mmm/schema/>
PREFIX gvp: <http://vocab.getty.edu/ontology#>

SELECT COUNT (DISTINCT ?manuscript) WHERE {
  ?place_uri gvp:broaderPreferred* <http://ldf.fi/mmm/place/tgn_1000070> .
  ?event_uri ecrm:P7_took_place_at ?place_uri ;
            ecrm:P30_transferred_custody_of|mmms:observed_manuscript ?manuscript .
  ?place_uri skos:prefLabel ?place_label .
  ?manuscript a efrbroo:F4_Manifestation_Singleton.
}
```
<br>
------
#### Actor queries
These sets of queries relate to Actors: persons or institutions who produced, bought, or sold manuscripts (manifestation singletons).

------

**Query 2: Find owners of manuscripts**

To find the names of Actors who owned manuscripts, use the query below. Manuscripts (manifestation singletons) link to their owners via the ```ecrm:P51_has_former_or_current_owner``` predicate. MMM uses ```skos:prefLabel``` to link human-readable labels to owners.

[https://api.triplydb.com/s/NnS-ONgC](https://api.triplydb.com/s/NnS-ONgC)

```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX efrbroo: <http://erlangen-crm.org/efrbroo/>
PREFIX ecrm: <http://erlangen-crm.org/current/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?manuscript ?owner_label WHERE {
  ?manuscript a efrbroo:F4_Manifestation_Singleton ;
	            ecrm:P51_has_former_or_current_owner ?owner .
  ?owner skos:prefLabel ?owner_label.
}
LIMIT 100
```
<br>
------

**Query 2.1: Find a specific Actor's uri**

To find a specific Actor's uri, modify Query 2 with a ```FILTER``` clause that includes the ```CONTAINS``` function. The query below searches for the uri of an entity that has the string "Phillipps" in its preferred label. This returns the uri for Sir Thomas Phillipps, one of the most important manuscript collectors of the 19th century. To search for a different Actor, edit the string within the quotation marks of the ```CONTAINS``` function.

[https://api.triplydb.com/s/mPniASzI](https://api.triplydb.com/s/mPniASzI)

```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX efrbroo: <http://erlangen-crm.org/efrbroo/>
PREFIX frbroo: <http://iflastandards.info/ns/fr/frbr/frbroo/>
PREFIX ecrm: <http://erlangen-crm.org/current/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?manuscript ?owner ?owner_label WHERE {
  ?manuscript a efrbroo:F4_Manifestation_Singleton ;
	ecrm:P51_has_former_or_current_owner ?owner .
  ?owner skos:prefLabel ?owner_label.
  FILTER (CONTAINS (?owner_label,"Phillipps"))
}
LIMIT 100
```
<br>
------

**Query 2.2: Find manuscripts owned by Sir Thomas Phillipps**

We found Sir Thomas Phillipps' uri in Query 2.1. Now we can use that uri to find all of the manuscripts (manifestation singletons) he owned. Modify Query 2 using the ```VALUES``` clause to specify that the ```?owner``` variable must link to Sir Thomas Phillipps' uri.

[https://api.triplydb.com/s/syvFvlD0](https://api.triplydb.com/s/syvFvlD0)
```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX efrbroo: <http://erlangen-crm.org/efrbroo/>
PREFIX frbroo: <http://iflastandards.info/ns/fr/frbr/frbroo/>
PREFIX ecrm: <http://erlangen-crm.org/current/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?manuscript ?owner ?owner_label WHERE {
  VALUES ?owner { <http://ldf.fi/mmm/actor/bodley_person_73979081> }
  ?manuscript a efrbroo:F4_Manifestation_Singleton ;
	            ecrm:P51_has_former_or_current_owner ?owner .
  ?owner skos:prefLabel ?owner_label.
}
```
<br>
------

**Query 2.3: Find manuscripts owned by Sir Thomas Phillipps and Chester Beatty**

Search for manuscripts owned by more than one Actor by including multiple ```VALUE``` clauses in your query. The query below modifies Query 2.2 to return manuscripts (manifestation singletons) owned by both Sir Thomas Phillipps and Chester Beatty. We first created two separate ```VALUE``` clauses for our two separate owners (adding Chester Beatty as ```?owner2```). We then added ```?owner2``` as an object of our ```?manuscript``` variable via the same ```ecrm:P51_has_former_or_current_owner``` predicate.

[https://api.triplydb.com/s/GjlRSUPY](https://api.triplydb.com/s/GjlRSUPY)
```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX efrbroo: <http://erlangen-crm.org/efrbroo/>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX ecrm: <http://erlangen-crm.org/current/>

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
<br>
------

**Query 2.4: Count manuscripts owned by Sir Thomas Phillipps and Chester Beatty**

Modify the previous query to count the number of unique manuscripts collected by Phillipps and Beatty. Add the ```COUNT``` function to the ```SELECT``` statement, and use the ```DISTINCT``` modifier to ensure that the results only count each manuscript once. Remove every variable in this line other than the manuscript variable. The results for this query will be a single row and column showing the number of manuscripts that meet

[https://api.triplydb.com/s/TYTfQGf6](https://api.triplydb.com/s/TYTfQGf6)
```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX efrbroo: <http://erlangen-crm.org/efrbroo/>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX ecrm: <http://erlangen-crm.org/current/>

SELECT COUNT (DISTINCT ?manuscript) WHERE {
  VALUES ?owner { <http://ldf.fi/mmm/actor/bodley_person_73979081> }
	VALUES ?owner2 { <http://ldf.fi/mmm/actor/bibale_6119> }
  ?manuscript a efrbroo:F4_Manifestation_Singleton ;
              skos:prefLabel ?manuscript_label ;
	            ecrm:P51_has_former_or_current_owner ?owner ;
	            ecrm:P51_has_former_or_current_owner ?owner2 .
  ?owner skos:prefLabel ?owner_label.
}
```
<br>
------

**Query 2.5: Limit results to a specific MMM source dataset**

You can limit the results of your query to manuscript data derived from a particular source. Use a similar construction to the ```FILTER``` clause we applied in Query 2.1. In this case, we'll add the ```STR``` function to this clause, which will match the results of the ```?manuscript``` variable to a specific string of characters. In the example below we're matching ```?manuscript``` to the string "bibale" that appears in the uri for manuscript data that comes from the _Bibale_ database. To search for _SDBM_ data, use the string "sdbm". For _Bodleian data_, use "bodley".

This example applies to Query 2.4, but you can use a similar construction in other queries that contain manuscripts.
[https://api.triplydb.com/s/ZQpw9XTB](https://api.triplydb.com/s/ZQpw9XTB)
```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX efrbroo: <http://erlangen-crm.org/efrbroo/>
PREFIX frbroo: <http://iflastandards.info/ns/fr/frbr/frbroo/>
PREFIX ecrm: <http://erlangen-crm.org/current/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT COUNT (DISTINCT ?manuscript) WHERE {
  VALUES ?owner { <http://ldf.fi/mmm/actor/bodley_person_73979081> }
  VALUES ?owner2 { <http://ldf.fi/mmm/actor/bibale_6119> }
  ?manuscript a efrbroo:F4_Manifestation_Singleton ;
              skos:prefLabel ?manuscript_label ;
  	          ecrm:P51_has_former_or_current_owner ?owner ;
  	          ecrm:P51_has_former_or_current_owner ?owner2 .
  ?owner skos:prefLabel ?owner_label.
}
FILTER (CONTAINS (STR(?manuscript),"bibale"))
```
<br>
------
#### Manuscript production queries
Use these queries to find the dates and places where manuscripts were created.

------

**Query 3: Find manuscript production events and their timespans**

Data about the production of manuscripts is mapped to the Production class (```ecrm:E12_Production```). For a basic query that returns a random sampling of production events, the manuscripts that they resulted in, and the timespans in which they occurred, use the query below. Production events link to the manuscripts they produced via the ```ecrm:P108_has_produced``` property, and to their timespans via the ```ecrm:P4_has_time``` property.

Notice that for the following query, we don't need to define our ```?manuscript``` variable as a manifestation singleton. The predicate ```ecrm:P108_has_produced``` can only refer to manifestation singletons, so stating it again would be redundant.

[https://api.triplydb.com/s/zkXxnccn](https://api.triplydb.com/s/zkXxnccn)

```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX ecrm: <http://erlangen-crm.org/current/>

SELECT * WHERE {
  ?production_event ecrm:P108_has_produced ?manuscript ;
                    ecrm:P4_has_time-span ?timespan .
}
LIMIT 100
```
<br>
------

**Query 3.1: Find the beginning and end of manuscript production event timespans**

Timespans are complicated because we have to model an estimation. This requires many separate statements that, when taken as a whole, can accurately represent a range. The Timespan class (```ecrm:E52_Time-Span```) consists of several properties that accomplish this. In most cases in MMM, you will only need two of these properties to construct a date range: ```ecrm:P82a_begin_of_the_begin```(the very beginning of a timespan) and ```ecrm:P82b_end_of_the_end```(the very end of the timespan). The query below builds upon Query 3 by returning the values of the beginning and end of the production event timespans.

[https://api.triplydb.com/s/EZ_2fY_d](https://api.triplydb.com/s/EZ_2fY_d)

```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX ecrm: <http://erlangen-crm.org/current/>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>

SELECT * WHERE {
 ?production_event ecrm:P108_has_produced ?manuscript ;    
                   ecrm:P4_has_time-span ?timespan .
  ?timespan ecrm:P82a_begin_of_the_begin ?begin ;    
        skos:prefLabel ?begin_label .
  ?timespan ecrm:P82b_end_of_the_end ?end ;
        skos:prefLabel ?end_label .
}
LIMIT 100
```
<br>
------

**Query 3.2: Limit timespan to a specific range of years**

In Query 3.1, our results included integer strings that represented the timespans of the ```?begin``` and ```?end``` variables. To limit those results a specific range of years, we need to extract the year information from those strings, and then filter the results. This requires three additional steps, using the ```BIND``` and ```FILTER``` clause with additional support from  ```YEAR``` function. Use the ```YEAR``` function to extract the year data from the ```?begin``` variable results, nestled within the ```BIND``` clause that assigns the year data as a new variable, ```?begin_year```. Then apply the ```FILTER``` clause to that new variable. We've used ```FILTER``` in several of the prior queries, but this one will be special because it includes two statements: a value that ```?begin_year``` must be less than (```<```), and a second value that it must be greater than (```>```). The ```&&``` between the two value statements is the symbol for logical and, meaning that both the first and last statement must be true for the value to be included in the results. In the query below, the ```FILTER``` clause states that the value for ```?begin_year``` must be less than 1000 and greater than 500.

[https://api.triplydb.com/s/Lf4FWleR](https://api.triplydb.com/s/Lf4FWleR)
```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX ecrm: <http://erlangen-crm.org/current/>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>

SELECT * WHERE {
 ?production_event ecrm:P108_has_produced ?manuscript ;    
                   ecrm:P4_has_time-span ?timespan .
  ?timespan ecrm:P82a_begin_of_the_begin ?begin ;    
        skos:prefLabel ?begin_label .
   ?timespan ecrm:P82b_end_of_the_end ?end ;
        skos:prefLabel ?end_label .
  BIND (YEAR (?begin) AS ?begin_year)
  FILTER ( ?begin_year < 1000 && ?begin_year > 500 )
}
LIMIT 2000
```
<br>
------
#### Advanced queries
These queries combine strategies demonstrated in the previous sections

------
**Query 4.1: Which manuscripts from Sir Thomas Phillipps’ collection are in British libraries now?**

This question can only be answered in a modified form with the MMM data. Our Actor class only distinguishes between Persons, Groups, and Actors (the last being a catch-all designation for instances where we don’t know whether an Actor is a Person or a Group). Rather than finding manuscripts owned by libraries, the best we can do is find manuscripts owned by a Group. Another caveat is that our location data for Actors associated with Britain specifically is limited. Much of our geographic association for Actors derives from VIAF, which uses the United Kingdom as the geographic designation for their records associated with Britain. This means that the best way to find British Actors is to search for Actors associated with the United Kingdom, with the understanding that these results may also include Actors associated with Northern Ireland.

With these limitations in mind, the modified query can be understood as: Which manuscripts from Sir Thomas Phillipps’ collection are also associated with Groups who have a geographic association with the United Kingdom?

To construct our query, we start by finding the manuscripts in Phillipps’ collection. First, we write a ```VALUES``` statement to assign Sir Thomas Phillipps’ uri to our first ```?owner``` variable. We follow that with our typical statements establishing the ```?manuscript``` variable and linking that to the first owner. We then write another statement to include manuscripts that list Phillipps in their collection label, since provenance can be indicated in manifestation singletons either through the ```?owner``` or ```?collection``` class. We filter the results based on the collection label, to include the name Phillipps but not the name Halliwell (Phillipps’ son-in-law, whose name is Halliwell-Phillipps and had his own manuscript collection). Then we use the ```UNION``` function to connect the results of these two statements.

Now that we have the manuscripts in Phillipps’ collection, we can narrow these results to those manuscripts that have another type of owner, a Group associated with the United Kingdom. We first establish our ```?owner2``` variable as the ```ecrm:E74_Group``` class. We link this second owner to our ```?manuscript``` variable, then get its label and link to its formation event via the ```ecrm:P98i_was_born``` predicate. Formation events link to their associated place via the ```ecrm:P7_took_place_at``` predicate, which we can mandate for our query to include only places within the United Kingdom.

[https://api.triplydb.com/s/S55GFW_8M](https://api.triplydb.com/s/S55GFW_8M)
```
PREFIX gvp: <http://vocab.getty.edu/ontology#>
PREFIX ecrm: <http://erlangen-crm.org/current/>
PREFIX efrbroo: <http://erlangen-crm.org/efrbroo/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>

# Question: How many manuscripts formerly owned by Sir Thomas Phillipps are in British libraries?

SELECT DISTINCT ?manuscript ?owner2 ?owner2_label ?owner2_formation WHERE {
  VALUES ?owner {
    <http://ldf.fi/mmm/actor/bodley_person_73979081> # defined owner as Sir Thomas Phillipps
  }

  { ?manuscript a efrbroo:F4_Manifestation_Singleton ;
  ecrm:P51_has_former_or_current_owner ?owner .
  }
  UNION
  { ?manuscript a efrbroo:F4_Manifestation_Singleton ;
  ecrm:P46i_forms_part_of ?collection .
    ?collection skos:prefLabel ?collection_label .
    FILTER (CONTAINS (?collection_label, "Phillipps"))
    FILTER (!CONTAINS (?collection_label, "Halliwell"))
  }

  ?owner2 a ecrm:E74_Group .
  ?manuscript ecrm:P51_has_former_or_current_owner ?owner2 .
  ?owner2 skos:prefLabel ?owner2_label ;
          ecrm:P98i_was_born ?owner2_formation .
  ?owner2_formation ecrm:P7_took_place_at ?owner2_place .
  ?owner2_place gvp:broaderPreferred* <http://ldf.fi/mmm/place/tgn_7008591> . # uri for United Kingdom

}

```

<!---#### Works and all that
Querying data about the works contained within manuscripts is tricky, and there will be many nuances in the results you receive. MMM follows the [FRBRoo model](https://www.ifla.org/files/assets/cataloguing/FRBRoo/frbroo_v_2.4.pdf) for describing the relationship between manuscripts and the works they contain (see page 17 of the linked FRBRoo description for a diagram illustrating this hierarchy).

Here's a brief summary of the FRBRoo Work-Expression-Manifestation Singleton hierarchy: an intellectual or artistic conception with an author and a title (Work) exists in a specific textual form (Expression) within a unique manuscript (Manifestation Singleton). For an MMM example, think of Augustine's _City of God_. Augustine conceived of this Work, which then appeared in various linguistic expressions, starting with his autograph manuscript and followed by subsequent copies. These expressions exist within specific unique manuscripts, such as the British Library's [Royal 17 F III](https://www.bl.uk/catalogues/illuminatedmanuscripts/record.asp?MSID=5690&CollID=16&NStart=170503)).

To make matters more difficult, none of MMM's data sources map easily to the FRBR model. The SDBM has no Work concept at all, since it repeats data from other sources that do not use it. The Bibale and Bodleian datasets do have Works, though they do not strictly follow FRBR and include language and date information within their Work objects.

TODO: explain when to use Work, Expression, when to jump straight from MSingle to author, etc.--->


### Medieval Manuscripts in Oxford Libraries
 _Medieval Manuscripts in Oxford Libraries_ provides descriptions of all known Western medieval manuscripts in the Bodleian Library, and of medieval manuscripts in selected Oxford colleges (Christ Church, Jesus, and Merton). It was launched in August 2017. As of July 2019, it covers more than 9,400 manuscripts.

 Most of the descriptions are summary entries, based primarily on the Quarto and Summary Catalogues published between 1853 and 1924. Detailed modern descriptions are available for manuscripts acquired since 1916.

 The manuscript descriptions and authority files are encoded in XML according to the Guidelines of the Text Encoding Initiative (TEI). The manuscript descriptions conform to a customization used by all the Bodleian Library’s TEI catalogues: [https://github.com/bodleian/consolidated-tei-schema](https://github.com/bodleian/consolidated-tei-schema)

The XML files, together with processing software, are stored in a publicly accessible GitHub repository: [https://github.com/bodleian/medieval-mss](https://github.com/bodleian/medieval-mss)

The Web site for the catalogue was developed by staff at Bodleian Digital Library Systems and Services. The XML files are indexed using Solr and displayed using the open-source discovery platform Blacklight.


#### MMM transformation

For the Mapping Manuscript Migrations project, the XML files were transformed into RDF Linked Data. The first step was to extract a selection of the XML elements from each file and combine them into a simpler, flatter XML structure using xQuery. This simplified XML was then mapped to RDF using the 3M (Mapping Memory Manager) software: [https://github.com/isl/Mapping-Memory-Manager](https://github.com/isl/Mapping-Memory-Manager)

This initial mapping was based on the CIDOC-CRM ontology. The resulting RDF was then mapped to the Unified Data Model developed for the Mapping Manuscript Migrations project, which combines entities and properties from CIDOC-CRM and FRBROO and adds some project-specific entities and properties.  

Scripts and mapping files can be found here: [https://github.com/mapping-manuscript-migrations/bodleian-mmm](https://github.com/mapping-manuscript-migrations/bodleian-mmm)

The transformation process produces more than 80,500 RDF triples. A small sample of the RDF can be seen here: [http://yasgui.org/short/jZ6h1tSwv](http://yasgui.org/short/jZ6h1tSwv)

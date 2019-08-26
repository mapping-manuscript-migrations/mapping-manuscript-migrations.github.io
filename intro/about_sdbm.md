### Schoenberg Database of Manuscripts (SDBM)

The SDBM aggregates observations of pre-modern manuscripts drawn from over 13,500 auction and sales catalogs, inventories, catalogs from institutional and private collections, and other sources that document the sales and locations of these books from around the world. Using 36 possible fields, entries in the SDBM record data from observations made in these sources and assist researchers in locating and identifying particular manuscripts, establishing provenance, and aggregating descriptive information about specific manuscripts as they are described in various sources over time. The SDBM data is stored in a MySQL relational database, with Ruby on Rails providing the overall web framework. Blacklight provides front end display support for public searching. The SDBM is hosted by the University of Pennsylvania Libraries and is publicly available [here](https://sdbm.library.upenn.edu/).

#### SDBM Data model

![Image of SDBM Data Model](NewSDBMflow.png)

The SDBM's data model is designed to capture data about both manuscripts and the sources that describe them.

**Sources** describe manuscripts, such as auction catalogs, bookseller websites, etc.

**Entries** represent observations of manuscripts derived from a Source, such as a single lot from an auction catalog, or a single item in a library catalog.

**Manuscript Records** link together Entries that describe the same manuscript, so that you can easily compare different descriptions of the same manuscript over time.

**SDBM Name Authority files** govern fields related to persons and institutions who impacted the production or provenance history of a manuscript, including authors, scribes, artists, owners (both individual persons and institutions), and selling agents. When possible, SDBM Name Authority files include links to VIAF. The Names can also link to SDBM Place Authority files, indicating the places where a person lived or the location of an institution.

**SDBM Place Authority files** appear within an Entry to indicate where a manuscript was produced. These files link to the Getty Thesaurus of Geographic Names, GeoNames, or VIAF, when possible. SDBM Places also link to SDBM Name Authority files.


#### MMM transformation

SDBM users access the SDBM SQL database via a user interface powered by Ruby on Rails. Whenever a user makes a change to a SDBM record, that change is reflected in the SDBM user interface, which in turn changes the data stored in the SQL database. The change also triggers a messaging process overseen by RabbitMQ that communicates this change to the SDBM RDF triplestore. The data in this triplestore adheres to the SDBM data model, so it undergoes a final transformation against the MMM data model before it arrives in the MMM triplestore.

![Image of SDBM Data Transformation](SDBMtoMMMtransformationprocess.png)

#### Mapping of an SDBM Entry into MMM

![Image of SDBM MS Record mapping to MMM](MappingSDBM_MS_RecordtoMMM.png)

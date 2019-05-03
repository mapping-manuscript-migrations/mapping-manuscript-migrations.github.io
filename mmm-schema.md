| Class/Property                                                                                                            | Cardinality | Range                               |
|---------------------------------------------------------------------------------------------------------------------------|-------------|-----------------------------------  |
| Namespaces:                                                                                                               |             |                                     |
| PREFIX dct: <http://purl.org/dc/terms/>                                                                                   |             |                                     |
| PREFIX ecrm: <http://erlangen-crm.org/current/>                                                                           |             |                                     |
| PREFIX frbroo: <http://erlangen-crm.org/efrbroo/>                                                                         |             |                                     |
| PREFIX mmms: <http://ldf.fi/mmm/schema/>                                                                                  |             |                                     |
| `frbroo:F1_Work`                                                                                                          |             |                                     |
| `dct:source`                                                                                                              |             | `mmms:Database`                     |
| `mmms:data_provider_url`                                                                                                  |             | URL                                 |
| `skos:altLabel`                                                                                                           |             | string                              |
| `skos:prefLabel`                                                                                                          |             | string                              |
| `frbroo:F27_Work_Conception`                                                                                              |             |                                     |
| `ecrm:P4_has_time_span`                                                                                                   |             | `ecrm:E52_Time-Span`                |
| `ecrm:P7_took_place_at`                                                                                                   |             | `ecrm:E53_Place`                    |
| `dct:source`                                                                                                              |             | `mmms:Database`                     |
| `skos:prefLabel`                                                                                                          |             | string                              |
| `frbroo:R16_initiated`                                                                                                    |             | `frbroo:F1_Work`                    |
| `mmms:carried_out_by_as_author`                                                                                           |             | `ecrm:E21_Person`                   |
| `mmms:carried_out_by_as_possible_author`                                                                                  |             | `ecrm:E21_Person`                   |
| `mmms:carried_out_by_as_commissioner`                                                                                     |             | `ecrm:E21_Person`                   |
| `mmms:carried_out_by_as_editor`                                                                                           |             | `ecrm:E21_Person`                   |
| `frbroo:F2_Expression`, `ecrm:E33_Linguistic_Object`.                                                                     |             |                                     |
| `ecrm:P72 has language`                                                                                                   |             | string                              |
| `dct:source`                                                                                                              |             | `mmms:Database`                     |
| `mmms:data_provider_url`                                                                                                  |             | URL                                 |
| `skos:altLabel`                                                                                                           |             | string                              |
| `skos:prefLabel`                                                                                                          |             | string                              |
| `frbroo:F28_Expression_Creation`                                                                                          |             |                                     |
| `ecrm:E12_Production`                                                                                                     |             |                                     |
| `ecrm:P4_has_time_span`                                                                                                   |             | ecrm:E52_Time-Span                  |
| `ecrm:P7_took_place_at`                                                                                                   | 1..n        | `ecrm:E53_Place`                    |
| `ecrm:P108_has_produced`                                                                                                  |             | `frbroo:F4_Manifestation_Singleton` |
| `dct:source`                                                                                                              |             | `mmms:Database`                     |
| `skos:prefLabel`                                                                                                          |             | string                              |
| `mmms:carried_out_by_as_commissioner`                                                                                     |             | `ecrm:E21_Person`                   |
| `mmms:carried_out_by_as_illuminator`                                                                                      |             | `ecrm:E21_Person`                   |
| `mmms:carried_out_by_as_printer`                                                                                          |             | `ecrm:E21_Person`                   |
| `mmms:carried_out_by_as_scribe`                                                                                           |             | `ecrm:E21_Person`                   |
| `frbroo:F4_Manifestation_Singleton`                                                                                       |             |                                     |
| `ecrm:P128_carries`                                                                                                       |             | F2 Expression                       |
| `ecrm:P3_has_note`                                                                                                        |             | string                              |
| `ecrm:P43_has_dimension`                                                                                                  |             | "mmms:Width, ..."                   |
| `ecrm:P45_consists_of`                                                                                                    |             | `mmms:Material`                     |
| `ecrm:P46_is_composed_of`                                                                                                 |             | `frbroo:F4_Manifestation_Singleton` |
| `ecrm:P46i_forms_part_of`                                                                                                 |             | `ecrm:E78_Collection`               |
| `ecrm:P51_has_former_or_current_owner`                                                                                    |             | `ecrm:E21_Person`                   |
| `ecrm:P52_has_current_owner`                                                                                              |             | `ecrm:E21_Person`                   |
| `ecrm:P70i_is_documented_in`                                                                                              |             | `mmms:Source`                       |
| `dct:source`                                                                                                              |             | `mmms:Database`                     |
| `mmms:catalog_or_lot_number`                                                                                              |             | string                              |
| `mmms:data_provider_url`                                                                                                  |             | URL                                 |
| `mmms:entry`                                                                                                              |             | URL                                 |
| `mmms:manuscript_author`                                                                                                  |             | `ecrm:E21_Person`                   |
| `mmms:manuscript_record`                                                                                                  |             | URL                                 |
| `mmms:manuscript_work`                                                                                                    |             | `frbroo:F1_Work`                    |
| `mmms:phillipps_number`                                                                                                   |             | string                              |
| `mmms:shelfmark_arsenal`                                                                                                  |             | string                              |
| `mmms:shelfmark_barocci`                                                                                                  |             | string                              |
| `mmms:shelfmark_bnf_hebreu`                                                                                               |             | string                              |
| `mmms:shelfmark_bnf_latin`                                                                                                |             | string                              |
| `mmms:shelfmark_bnf_nal`                                                                                                  |             | string                              |
| `mmms:shelfmark_buchanan`                                                                                                 |             | string                              |
| `mmms:shelfmark_christ_church`                                                                                            |             | string                              |
| `owl:sameAs`                                                                                                              |             | URI                                 |
| `skos:altLabel`                                                                                                           |             | string                              |
| `skos:prefLabel`                                                                                                          |             | string                              |
| ecrm:E52_Time-Span                                                                                                        |             |                                     |
| `ecrm:P81a_end_of_the_begin`                                                                                              |             | datetime                            |
| `ecrm:P81b_begin_of_the_end`                                                                                              |             | datetime                            |
| `ecrm:P82a_begin_of_the_begin`                                                                                            |             | datetime                            |
| `ecrm:P82b_end_of_the_end`                                                                                                |             | datetime                            |
| `skos:altLabel`                                                                                                           |             | string                              |
| `skos:prefLabel`                                                                                                          |             | string                              |
| E10 Transfer of Custody / E7 Activity                                                                                     |             |                                     |
| `skos:prefLabel`                                                                                                          |             | string                              |
| `ecrm:P11_had_participant`                                                                                                |             | `ecrm:E21_Person`                   |
| `ecrm:P28_custody_surrendered_by`                                                                                         |             | `ecrm:E21_Person`                   |
| `ecrm:P29_custody_received_by`                                                                                            |             | `ecrm:E21_Person`                   |
| `ecrm:P30_transferred_custody_of`                                                                                         |             | `ecrm:E21_Person`                   |
| `ecrm:P3_has_note`                                                                                                        |             | string                              |
| ecrm:P4_has_time-span                                                                                                     |             | ecrm:E52_Time-Span                  |
| `ecrm:P70i_is_documented_in`                                                                                              |             | Source                              |
| `ecrm:P7_took_place_at`                                                                                                   |             | `ecrm:E53_Place`                    |
| `mmms:data_provider_url`                                                                                                  |             | URL                                 |
| `mmms:observed_manuscript`                                                                                                |             | `frbroo:F4_Manifestation_Singleton` |
| `mmms:ownership_attributed_to`                                                                                            |             | `ecrm:E21_Person`                   |
| `dct:source`                                                                                                              |             | `mmms:Database`                     |
| `skos:prefLabel`                                                                                                          |             | string                              |
| `ecrm:E78_Collection`                                                                                                     |             |                                     |
| `ecrm:P51_has_former_or_current_owner`                                                                                    |             |                                     |
| `ecrm:P92i_was_brought_into_existence_by`                                                                                 |             |                                     |
| `ecrm:P93i_was_taken_out_of_existence_by`                                                                                 |             |                                     |
| `mmms:collection_location`                                                                                                |             |                                     |
| `mmms:collection_type`                                                                                                    |             |                                     |
| `mmms:data_provider_url`                                                                                                  |             |                                     |
| `mmms:external_url`                                                                                                       |             |                                     |
| `mmms:institution_literal`                                                                                                |             |                                     |
| `mmms:location_literal`                                                                                                   |             |                                     |
| `mmms:source_agent`                                                                                                       |             |                                     |
| `mmms:source_date`                                                                                                        |             |                                     |
| `mmms:source_type`                                                                                                        |             |                                     |
| `dct:source`                                                                                                              |             |                                     |
| `skos:altLabel`                                                                                                           |             |                                     |
| `skos:prefLabel`                                                                                                          |             |                                     |
| `mmms:Source`                                                                                                             |             |                                     |
| `ecrm:E53_Place`                                                                                                          |             |                                     |
| `mmms:Actor` / `mmms:Person` / `mmms:Organization`                                                                        |             |                                     |
| ecrm:E67_Birth / `ecrm:E69_Death`                                                                                         |             |                                     |
| ecrm:E63_Beginning_of_Existence / `ecrm:E64_End_of_Existence`                                                             |             |                                     |
| ecrm:E66_Formation / `ecrm:E68_Dissolution`                                                                               |             |                                     |
| `ecrm:E57_Material`                                                                                                       |             |                                     |
| `mmms:Height` / `mmms:Width`                                                                                              |             |                                     |
| `ecrm:P90_has_value`                                                                                                      |             |                                     |
| `ecrm:P91_has_unit`                                                                                                       |             | `mmms:Millimetre`                   |
| `mmms:Folios` / `mmms:Columns` / `mmms:Lines` / `mmms:DecoratedInitials` / `mmms:HistoriatedInitials` / `mmms:Miniatures` |             |                                     |
| `ecrm:P90_has_value`                                                                                                      |             | integer                             |
| `mmms:Database`                                                                                                           |             |                                     |



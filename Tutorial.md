# Querying CIDOC-CRM

This document is intended to provide some examples of how to access resources defined using the CIDOC-CRM ontology.

The SPARQL endpoint used is accessible at the following link https://researchportal-public.gta.arch.ethz.ch/resource/sparql


# Architecture of a sparql query

## Declare prefix shortcuts

  * Designates a shorthand to denote your data's namespace

  ```
PREFIX  org:  <http://example.com/org/>
PREFIX  rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX  wdt:  <http://www.wikidata.org/prop/direct/>
PREFIX  skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX  rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX  wd:   <http://www.wikidata.org/entity/>
PREFIX  crm:  <http://www.cidoc-crm.org/cidoc-crm/>
```

## Define the dataset

  * Appoints variables for the data your query will retrieve and display to the screen and specifies the data to pull out of your dataset

  ```
  SELECT

  FROM <...>

  FROM NAMED <...>

  WHERE {

    query pattern 

  }

  ```

## Query modifiers (optional)

  * Applied to create another output sequence

  ```
  GROUP BY

  HAVING

  ORDER BY

  LIMIT

  OFFSET

  BINDINGS

  ```

## Type of SPARQL queries

  * SELECT (Select specific variables and expressions)

  ```

  PREFIX  org:  <http://example.com/org/>
  PREFIX  rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
  PREFIX  wdt:  <http://www.wikidata.org/prop/direct/>
  PREFIX  skos: <http://www.w3.org/2004/02/skos/core#>
  PREFIX  rdfs: <http://www.w3.org/2000/01/rdf-schema#>
  PREFIX  wd:   <http://www.wikidata.org/entity/>
  PREFIX  crm:  <http://www.cidoc-crm.org/cidoc-crm/>

    SELECT ?s

    WHERE {

      ?s a crm:E35_Title

    }
```

* CONSTRUCT (Construct RDF triples/graphs)

```
 CONSTRUCT 
   WHERE { 
         ?s   rdf:type              crm:E53_Place ;
              crm:P1_is_identified_by  ?place_name .
          ?place_name
              rdf:type crm:E41_Appellation ;
              rdfs:label            ?label
    FILTER ( ?label = "Zollikon" )
   }

```

* ASK (return TRUE or FALSE)

```
PREFIX  org:  <http://example.com/org/>
PREFIX  rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX  wdt:  <http://www.wikidata.org/prop/direct/>
PREFIX  skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX  rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX  wd:   <http://www.wikidata.org/entity/>
PREFIX  crm:  <http://www.cidoc-crm.org/cidoc-crm/>


ASK {<https://resource.swissartresearch.net/archivalobject/22453> ?o ?p}


```

* DESCRIBE (Describe the resources matched by the given variables)

```
PREFIX  org:  <http://example.com/org/>
PREFIX  rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX  wdt:  <http://www.wikidata.org/prop/direct/>
PREFIX  skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX  rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX  wd:   <http://www.wikidata.org/entity/>
PREFIX  crm:  <http://www.cidoc-crm.org/cidoc-crm/>


DESCRIBE <https://resource.swissartresearch.net/archivalobject/22453> 
```
# Overview of the triplestore

Following queries are meant to provide the end user with a general overview of how data are structured in a triple store if you do not know how resources are structured.

## Show me all the graphs
```
SELECT DISTINCT ?g 

WHERE {

  GRAPH ?g { ?s ?p ?o }
}
```
## Number of statement in each named graph
```
SELECT ?graphName
       ( COUNT ( * ) AS ?count )
WHERE
  {
    GRAPH ?graphName
      {
        ?s ?p ?o
      }
  }
GROUP BY ?graphName
```

## Obtain a list of classes and the number of entity per each class

```

PREFIX  org:  <http://example.com/org/>
PREFIX  rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX  wdt:  <http://www.wikidata.org/prop/direct/>
PREFIX  skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX  rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX  wd:   <http://www.wikidata.org/entity/>
PREFIX  crm:  <http://www.cidoc-crm.org/cidoc-crm/>

SELECT ?c
       ( COUNT ( * ) AS ?count )
WHERE
  {
    GRAPH <file://E78_rp_2.0.rdf-26-01-2022-03-31-30>
        {[] a ?c .}
  }
GROUP BY ?c
```



## All the E78_Collection

```
PREFIX  org:  <http://example.com/org/>
PREFIX  rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX  wdt:  <http://www.wikidata.org/prop/direct/>
PREFIX  skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX  rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX  wd:   <http://www.wikidata.org/entity/>
PREFIX  crm:  <http://www.cidoc-crm.org/cidoc-crm/>

SELECT ?s
      
WHERE
  {
  ?s a crm:E78_Collection
  }


```
## Number of statement per record
```
PREFIX  org:  <http://example.com/org/>
PREFIX  rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX  wdt:  <http://www.wikidata.org/prop/direct/>
PREFIX  skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX  rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX  wd:   <http://www.wikidata.org/entity/>
PREFIX  crm:  <http://www.cidoc-crm.org/cidoc-crm/>

select ?e78 (count(?o) AS ?no) where {
  ?e78 a crm:E78_Collection; ?o ?p .
} 
group by ?e78
ORDER BY DESC (?no)
```
# Description queries 

With the following queries, the user can see how the information is structured using the CIDOC-CRM ontology

## Retrieve the full graph of a specific resource


```

PREFIX  org:  <http://example.com/org/>
PREFIX  rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX  wdt:  <http://www.wikidata.org/prop/direct/>
PREFIX  skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX  rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX  wd:   <http://www.wikidata.org/entity/>
PREFIX  crm:  <http://www.cidoc-crm.org/cidoc-crm/>

SELECT DISTINCT ?p ?o
      
WHERE
  {
<https://resource.swissartresearch.net/archivalobject/21731> ?p ?o
  }
```

OR

```
PREFIX  org:  <http://example.com/org/>
PREFIX  rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX  wdt:  <http://www.wikidata.org/prop/direct/>
PREFIX  skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX  rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX  wd:   <http://www.wikidata.org/entity/>
PREFIX  crm:  <http://www.cidoc-crm.org/cidoc-crm/>


DESCRIBE <https://resource.swissartresearch.net/archivalobject/21731> 
```

# Specific queries (E78_Collection)

## Retrieve the name of a collection and the name of the collection creator

```
PREFIX  org:  <http://example.com/org/>
PREFIX  rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX  wdt:  <http://www.wikidata.org/prop/direct/>
PREFIX  skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX  rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX  wd:   <http://www.wikidata.org/entity/>
PREFIX  crm:  <http://www.cidoc-crm.org/cidoc-crm/>

SELECT DISTINCT  ?appellation ?label
WHERE
  { ?subject  rdf:type              crm:E78_Collection ;
              crm:P1_is_identified_by  ?name_identifier ;
              crm:P108i_was_produced_by  ?production .
    ?name_identifier
              rdf:type              crm:E41_Appellation ;
              rdfs:label            ?appellation .
    ?production  rdf:type           crm:E12_Production ;
              crm:P14_carried_out_by  ?e39 .
    ?e39      rdf:type              crm:E39_Actor ;
              crm:P2_has_type       <https://resource.swissartresearch.net/actor%20creator/actor%20creator> ;
              crm:P1_is_identified_by  ?actor_appellation .
    ?actor_appellation
              rdf:type              crm:E41_Appellation ;
              crm:P2_has_type       <https://resource.swissartresearch.net/types/Preferred%20Name> ;
              rdfs:label            ?label
  }

```

## Retrive the production place of a collection 

```

PREFIX  org:  <http://example.com/org/>
PREFIX  rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX  wdt:  <http://www.wikidata.org/prop/direct/>
PREFIX  skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX  rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX  wd:   <http://www.wikidata.org/entity/>
PREFIX  crm:  <http://www.cidoc-crm.org/cidoc-crm/>

SELECT  ?appellation ?label
WHERE
  { ?subject  rdf:type              crm:E78_Collection ;
              crm:P1_is_identified_by  ?name_identifier ;
              crm:P108i_was_produced_by  ?production .
    ?name_identifier
              rdf:type              crm:E41_Appellation ;
              rdfs:label            ?appellation .
    ?production  rdf:type           crm:E12_Production ;
              crm:P7_took_place_at  ?value .
    ?value    rdf:type              crm:E53_Place ;
              crm:P1_is_identified_by  ?e41 .
    ?e41      rdf:type              crm:E41_Appellation ;
              rdfs:label            ?label
  }

```

## Retrieve the collections produced in a specific place 

```
PREFIX  org:  <http://example.com/org/>
PREFIX  rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX  wdt:  <http://www.wikidata.org/prop/direct/>
PREFIX  skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX  rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX  wd:   <http://www.wikidata.org/entity/>
PREFIX  crm:  <http://www.cidoc-crm.org/cidoc-crm/>

SELECT DISTINCT  ?appellation ?label
WHERE
  { ?subject  rdf:type              crm:E78_Collection ;
              crm:P1_is_identified_by  ?name_identifier ;
              crm:P108i_was_produced_by  ?production .
    ?name_identifier
              rdf:type              crm:E41_Appellation ;
              rdfs:label            ?appellation .
    ?production  rdf:type           crm:E12_Production ;
              crm:P7_took_place_at  ?value .
    ?value    rdf:type              crm:E53_Place ;
              crm:P1_is_identified_by  ?place_name .
    ?place_name
              rdf:type              crm:E41_Appellation ;
              rdfs:label            ?label
    FILTER ( ?label = "Zollikon" )
  }
  ```

  ## Retrieve the collections produced beween a specific time span

  ```
PREFIX  xsd:  <http://www.w3.org/2001/XMLSchema#>
PREFIX  rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX  rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX  crm:  <http://www.cidoc-crm.org/cidoc-crm/>

SELECT DISTINCT  ?subject ?label_preferred_identifier ?label_preferred_name
WHERE
  { ?subject  rdf:type  crm:E78_Collection .
    ?subject crm:P108i_was_produced_by/crm:P4_has_time-span ?date .
    ?date  crm:P82a_begin_of_the_begin  ?begin ;
           crm:P82b_end_of_the_end  ?end
    FILTER ( strdt(str(?begin), xsd:date) <= "2003-01-01"^^xsd:date )
    FILTER ( strdt(str(?end), xsd:date) >= "1994-01-01"^^xsd:date )
    ?subject  rdf:type              crm:E78_Collection ;
              crm:P1_is_identified_by  ?preferred_name .
    ?preferred_name
              rdf:type              crm:E41_Appellation ;
              rdfs:label            ?label_preferred_name
  }
  ```

  # Specific queries (E53_Place)

  ## Retrieve all the person born in a place (inverse)

  ```
PREFIX  org:  <http://example.com/org/>
PREFIX  rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX  wdt:  <http://www.wikidata.org/prop/direct/>
PREFIX  skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX  rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX  wd:   <http://www.wikidata.org/entity/>
PREFIX  crm:  <http://www.cidoc-crm.org/cidoc-crm/>

SELECT DISTINCT  ?Place ?Person
WHERE
  { ?subject  rdf:type  crm:E53_Place ;
             ^crm:P7_took_place_at ?birth ;
             crm:P1_is_identified_by  ?name_identifier .
    ?name_identifier
              rdf:type              crm:E41_Appellation ;
              rdfs:label            ?Place .
    ?birth    rdf:type              crm:E67_Birth .
    ?birth ^crm:P98i_was_born ?value .
    ?value    rdf:type              crm:E21_Person ;
              crm:P1_is_identified_by  ?appellation .
    ?appellation  rdf:type          crm:E41_Appellation ;
              rdfs:label            ?Person
  }
  ```

  ## Retrieve the production place of E22_Man-Made_Object

  ```
PREFIX  org:  <http://example.com/org/>
PREFIX  rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX  wdt:  <http://www.wikidata.org/prop/direct/>
PREFIX  skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX  rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX  wd:   <http://www.wikidata.org/entity/>
PREFIX  crm:  <http://www.cidoc-crm.org/cidoc-crm/>

SELECT DISTINCT  ?Appellation ?Place_Appellation
WHERE
  { ?subject  rdf:type              crm:E22_Man-Made_Object ;
              crm:P1_is_identified_by  ?name_identifier ;
              crm:P108i_was_produced_by  ?production .
    ?name_identifier
              rdf:type              crm:E41_Appellation ;
              rdfs:label            ?Appellation .
    ?production  rdf:type           crm:E12_Production ;
              crm:P7_took_place_at  ?value .
    ?value    rdf:type              crm:E53_Place ;
              crm:P1_is_identified_by  ?place_appellation .
    ?place_appellation
              rdf:type              crm:E41_Appellation ;
              rdfs:label            ?Place_Appellation
  }

  ```


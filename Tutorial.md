# Tutorial

### https://researchportal-public.gta.arch.ethz.ch/resource/sparql


# SPARQL query architecture

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
# Gneric Overview about the triplestore

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

# Specific query 

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


## If you don't know the mapping structure of E78 use describe 


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

## Collection name and Collection creator

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
              crm:P2_has_type       <https://resource.swissartresearch.net/types/Preferred%20Name>;
              rdfs:label ?label
  }

```

## Collection produced in Zollikon ask, prepare, process, analyze, share, and act.

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

  ## Collection produced beween time span

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

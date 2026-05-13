---
ontology: http://dw-oml-3.github.io/bundle
---
# Components

This is a read-only table so that we can select all specialized instances of
a common base type

```table
---
columns: { iri: { label: "element"} }
---
PREFIX base: <http://dw-oml-3.github.io/foundation/base/base#> 
PREFIX sys:  <http://dw-oml-3.github.io/foundation/system/system#> 

SELECT ?iri (GROUP_CONCAT(DISTINCT(?name)) AS ?Name) (GROUP_CONCAT(DISTINCT(?id)) AS ?Id) (GROUP_CONCAT(DISTINCT(?subs)) AS ?Sub) (GROUP_CONCAT(DISTINCT(?if)) AS ?Ports)

WHERE {
    ?iri a sys:Component ;
        base:hasCanonicalName ?name ;
        sys:componentId ?id  .

       ?subs sys:composes ?iri .

    OPTIONAL {
        ?iri sys:presents ?if .  
    }    
} 
GROUP BY ?subs ?iri
ORDER BY ?subs ?iri

```


---
ontology: http://dw-oml-3.github.io/components
---
# Components

This is a read-only table so that we can select all specialized instances of
a common base type

```table
---
columns: { iri: { label: "element"} }
---
PREFIX base: <http://dw-oml-3.github.io/foundation/base/base#> 
PREFIX sys: <http://dw-oml-3.github.io/foundation/system/system#> 

SELECT ?iri ?name ?id ?if ?subs

WHERE {
    ?iri a sys:Component ;
        base:hasCanonicalName ?name ;
        sys:componentId ?id  .

       ?subs sys:composes ?iri .

    OPTIONAL {
        ?iri sys:presents ?if .  
    }
              
}

```


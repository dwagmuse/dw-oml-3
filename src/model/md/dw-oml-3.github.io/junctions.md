---
ontology: http://dw-oml-3.github.io/junctions
---
# Junctions

This is a read-only table so that we can select all specialized instances of
a common base type

```table
---
columns: { iri: { label: "element"} }
---
PREFIX base: <http://dw-oml-3.github.io/foundation/base/base#> 
PREFIX sys: <http://dw-oml-3.github.io/foundation/system/system#> 

SELECT ?iri ?id ?ss ?cmp ?if

WHERE {
    ?iri a sys:Junction ;
        sys:junctionId ?id  .

    OPTIONAL {
        ?iri sys:connectsSubsystem ?ss .
    }
    OPTIONAL {
        ?iri sys:connectsComponent ?cmp .
    }
    OPTIONAL {
        ?iri sys:joins ?if .  
    }    
} 


```


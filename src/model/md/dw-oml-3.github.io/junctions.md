---
ontology: http://dw-oml-3.github.io/junctions
---
# Junctions

This is a read-only table so that we can select all specialized instances of
a common base type.

FIXME: Why do I get an error when I add ?Jid to the select list?

```table
---
columns: { iri: { label: "Element"} }
---
PREFIX base: <http://dw-oml-3.github.io/foundation/base/base#> 
PREFIX sys: <http://dw-oml-3.github.io/foundation/system/system#> 

SELECT $iri (GROUP_CONCAT(DISTINCT(?if)) as ?Interfaces) (GROUP_CONCAT(DISTINCT(?ss)) as ?Subsystem) (GROUP_CONCAT(DISTINCT(?cmp)) as ?Component) 

WHERE {
    ?iri a sys:Junction ;
        sys:junctionId ?Jid  .

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
GROUP BY ?iri

```


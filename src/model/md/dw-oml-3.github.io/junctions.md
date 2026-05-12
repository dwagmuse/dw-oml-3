---
ontology: http://dw-oml-3.github.io/junctions
---
# Junctions

This is a read-only table so that we can select all specialized instances of
a common base type.

FIXME: need a way to collapse multi-valued columns for distinct junction
so that we have only one row per junction instance.

```table
---
columns: { iri: { label: "element"} }
---
PREFIX base: <http://dw-oml-3.github.io/foundation/base/base#> 
PREFIX sys: <http://dw-oml-3.github.io/foundation/system/system#> 

SELECT $iri (GROUP_CONCAT(DISTINCT(?if)) as ?interfaces) (GROUP_CONCAT(DISTINCT(?ss)) as ?subsystem) (GROUP_CONCAT(DISTINCT(?cmp)) as ?component) 

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
GROUP BY ?iri

```


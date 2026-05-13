---
ontology: http://dw-oml-3.github.io/bundle
---
# Components

This is a read-only table so that we can select all specialized instances of
a common base type

```table
---
columns: { Iri: { label: "Component"} }
stylesheet:
  - selector: header[name === "g"], column[name === "g"]
    style:
      display: "none"
---
PREFIX base: <http://dw-oml-3.github.io/foundation/base/base#> 
PREFIX sys:  <http://dw-oml-3.github.io/foundation/system/system#> 

SELECT ?g (GROUP_CONCAT(DISTINCT(?subs)) AS ?Subsystem) (GROUP_CONCAT(DISTINCT(?iri)) AS ?Iri)  (GROUP_CONCAT(DISTINCT(?name)) AS ?Name) (GROUP_CONCAT(DISTINCT(?id)) AS ?Id) ?Type (GROUP_CONCAT(DISTINCT(?if)) AS ?Ports)

WHERE {
    ?iri a sys:Component ;
        base:hasCanonicalName ?name ;
        sys:componentId ?id  .

    GRAPH ?g {
        ?iri a ?Type .
    }
    # This filters out entailed types
    FILTER (!contains(str(?g), "entailments"))
    ?subs sys:composes ?iri .

    OPTIONAL {
        ?iri sys:presents ?if .  
    }    
} 
GROUP BY ?subs ?iri ?Type ?g
ORDER BY ?Subsystem ?Iri

```


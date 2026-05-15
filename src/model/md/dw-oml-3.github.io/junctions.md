---
ontology: http://dw-oml-3.github.io/junctions
---
# Junctions

This is a read-only table so that we can select all specialized instances of
a common base type.


```table
---
columns: { iri: { label: "Junction"} }
stylesheet:
  - selector: header[name === "g"], column[name === "g"]
    style:
      display: "none"
---
PREFIX base: <http://dw-oml-3.github.io/foundation/base/base#> 
PREFIX sys: <http://dw-oml-3.github.io/foundation/system/system#> 

SELECT ?g ?iri ?Type (GROUP_CONCAT(DISTINCT(?if)) as ?Interfaces) (GROUP_CONCAT(DISTINCT(?ss)) as ?Subsystem) (GROUP_CONCAT(DISTINCT(?cmp)) as ?Component) 

WHERE {
    ?iri a sys:Junction ;
        sys:junctionId ?Jid  .
    GRAPH ?g {
        ?iri a ?Type .
    }
    # This filters out entailed types
    FILTER (!contains(str(?g), "entailments"))

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
GROUP BY ?g ?iri ?Type

```

## Breakdown by Type


  </div>
  <div style="flex:1; min-width:280px;">

```chart
---
type: pie
data:
  labels: Type
  datasets:
    - label: Requirements
      data: count
options:
  plugins:
    title:
      display: true
      text: Junctions by Type
    legend:
      position: right
---
PREFIX base: <http://dw-oml-3.github.io/foundation/base/base#> 
PREFIX sys: <http://dw-oml-3.github.io/foundation/system/system#> 

SELECT (STRAFTER(str(?t), "#") AS ?Type) (COUNT(?t) AS ?count)
WHERE {
    ?j a sys:Junction .
    GRAPH ?g {
        ?j a ?t .
    }
    FILTER (!contains(str(?g), "entailments"))
}
GROUP BY ?t
```

  </div>
</div>



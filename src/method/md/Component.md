---
template:
  id: http://dw-oml-3.github.io/foundation/system/system/Component
  name: "Component"
  rank: 0
  expose:
    - kind: navigation
      match:
        anyTypeOf: 
          - http://dw-oml-3.github.io/foundation/system/system#Component
  params:
    - id: member
      type: iri
      defaultValue: ${context.member}
      required: true
    - id: ontology
      type: iri
      defaultValue: ${context.ontology}
      required: true
---
# [[${member}]]

## Properties

```table
---
stylesheet:
  - selector: header[name === "g"], column[name === "g"]
    style:
      display: "none"
  - selector: cell[col === "Values" && row.get("g").endsWith("__entailments")]
    target: value
    style:
      padding: 4px 12px
      border-radius: 999px
      font-size: 12px
      background-color: pink
---
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX oml: <http://opencaesar.io/oml#>
PREFIX schema: <http://dw-oml-3.github.io/foundation/system/system#>

SELECT ?g ?Property (GROUP_CONCAT(?Value; separator=", ") AS ?Values)
WHERE {
    GRAPH ?g {
        <${member}> ?Property ?Value .
        FILTER(?Value != owl:NamedIndividual)
        FILTER(?Property != oml:type)
    }
}
GROUP BY ?g ?Property
ORDER BY STRAFTER(STR(?Property), "#")
```
## Interface Map

This table should list all of the interfaces presented by this component. 

Why is it not showing joined Junctions?

```table
---
stylesheet:
  - selector: header[name === "g"], column[name === "g"]
    style:
      display: "none"
  - selector: cell[col === "Values" && row.get("g").endsWith("__entailments")]
    target: value
    style:
      padding: 4px 12px
      border-radius: 999px
      font-size: 12px
      background-color: pink
---
PREFIX base: <http://dw-oml-3.github.io/foundation/base/base#>
PREFIX sys:  <http://dw-oml-3.github.io/foundation/system/system#>

SELECT ?If ?Id ?Name ?Junction

WHERE {

  <${member}> sys:presents ?If .

  OPTIONAL {
    ?If sys:interfaceId ?Id .
  }
  
  OPTIONAL {
    ?If base:hasCanonicalName ?Name .
  }

  OPTIONAL {
    ?Junction sys:joins ?If .
  }

}
ORDER BY ?Id

```

## Relations

```graph
---
stylesheet:
  - selector: node[value === "${member}"]
    style:
      fill: white
  - selector: node[outgoing.some(e => e.target.value.endsWith("__entailments"))]
    style:
      stroke: pink
      fill: pink
  - selector: node[incoming.some(e => e.value.endsWith("#graph"))]
    style:
      radius: 0
      opacity: 0
      color: none
  - selector: edge[value.endsWith("#graph")]
    style:
      stroke-width: 0
      opacity: 0
      color: none
---
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX oml: <http://opencaesar.io/oml#>

CONSTRUCT {
    <${member}> ?Property ?Value .
    ?Value oml:graph ?g .
}
WHERE {
    GRAPH ?g {
        <${member}> ?Property ?Value .
        FILTER (!isLiteral(?Value))
        FILTER(?Value != owl:NamedIndividual)
        FILTER(?Property != oml:type)
    }
}
```
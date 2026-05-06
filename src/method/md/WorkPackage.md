---
template:
  id: http://dw-oml-3.github.io/foundation/project/project/WorkPackage
  name: "WorkPackage"
  rank: 0
  expose:
    - kind: navigation
      match:
        anyTypeOf: 
          - http://dw-oml-3.github.io/foundation/project/project#WorkPackage
  params:
    - id: member
      type: iri
      from: context.member
      required: true
    - id: ontology
      type: iri
      from: context.ontology
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
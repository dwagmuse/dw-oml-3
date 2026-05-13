---
template:
  id: http://dw-oml-3.github.io/foundation/system/system/Junction
  name: "Junction"
  rank: 0
  expose:
    - kind: navigation
      match:
        anyTypeOf: 
          - http://dw-oml-3.github.io/foundation/system/system#Junction
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

## Description

```text
PREFIX base: <https://dw-oml-3.github.io/foundation/base/base#>

SELECT ?description
WHERE {
    GRAPH ?g {
        <${member}> base:hasDescription ?description .
    }
}
```

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

## Missing Connects Subsystem Relations

Find any connected components not in a subsystem listed in connected subsystems

```table

PREFIX base: <http://dw-oml-3.github.io/foundation/base/base#>
PREFIX sys:  <http://dw-oml-3.github.io/foundation/system/system#>

SELECT ?Subsystem ?Component
WHERE {
    <${member}> sys:connectsComponent ??Component .
    ?Subsystem sys:composes ?Component .
    FILTER NOT EXISTS { <${member}> sys:connectsSubsystem ?Subsystem }
}
```

## Missing Connects Component Relations

Report any instances where the junction joins to
an interface whose presenting component is not
related by connectsComponent. This should not happen if the UI only allows joins to select interfaces presented by related components.

```table

PREFIX base: <http://dw-oml-3.github.io/foundation/base/base#>
PREFIX sys:  <http://dw-oml-3.github.io/foundation/system/system#>

SELECT ?Subsystem ?Component ?Interface 
WHERE {
    <${member}> sys:joins ?Interface .
    ?Component sys:presents ?Interface .
    ?Subsystem sys:composes ?Component .
    FILTER NOT EXISTS { <${member}> sys:connectsComponent ?Component }
}
```
## Related Interfaces

```table

PREFIX base: <http://dw-oml-3.github.io/foundation/base/base#>
PREFIX sys:  <http://dw-oml-3.github.io/foundation/system/system#>

SELECT ?Subsystem ?Component ?Interface
WHERE {
    <${member}> sys:joins ?Interface .
    ?Component sys:presents ?Interface .
    ?Subsystem sys:composes ?Component .
}
ORDER BY ?Subsystem ?Component
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
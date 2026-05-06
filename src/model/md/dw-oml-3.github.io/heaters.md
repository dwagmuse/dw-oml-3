---
ontology: http://dw-oml-3.github.io/components
---
# Electrical Heaters


```table-editor
---
columns: { focus: { label: "CMP", prefix: "CMP_" } }
---
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix dash: <http://datashapes.org/dash#> .
@prefix base: <http://dw-oml-3.github.io/foundation/base/base#> .

@prefix ss: <http://dw-oml-3.github.io/foundation/system/system#> .
@prefix ee: <http://dw-oml-3.github.io/library/electrical/components#> .

ss:CShape
    a sh:NodeShape ;
    sh:targetClass ee:Heater ;
    sh:property [
        sh:path base:hasCanonicalName ;
        sh:name "Name" ;
        sh:maxcount 1 ;
    ] ;
    sh:property [
        sh:path ss:componentId ;
        sh:name "ID" ;
        sh:maxcount 1 ;
    ] 
    .
```


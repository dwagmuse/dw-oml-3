---
ontology: http://dw-oml-3.github.io/ss
---
# Subsystems

List of Subsystems in the model

```table-editor
---
columns: { focus: { label: "WP", prefix: "WP_" } }
---
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix dash: <http://datashapes.org/dash#> .
@prefix base: <http://dw-oml-3.github.io/foundation/base/base#> .

@prefix ss: <http://dw-oml-3.github.io/foundation/system/system#> .

ss:SSShape
    a sh:NodeShape ;
    sh:targetClass ss:Subsystem ;
    sh:property [
        sh:path base:hasCanonicalName ;
        sh:name "Name" ;
        sh:maxcount 1 ;
    ] ;
    sh:property [
        sh:path ss:hasSubsystemId ;
        sh:name "ID" ;
        sh:maxcount 1 ;
    ] 
    .
```


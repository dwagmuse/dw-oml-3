---
ontology: http://dw-oml-3.github.io/workpackages
---
# Work Packages

List of Work Packages in the model

```table-editor
---
columns: { focus: { label: "WP", prefix: "WP_" } }
---
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix dash: <http://datashapes.org/dash#> .
@prefix base: <http://dw-oml-3.github.io/foundation/base/base#> .
@prefix pp: <http://dw-oml-3.github.io/foundation/project/project#> .

pp:WPShape
    a sh:NodeShape ;
    sh:targetClass pp:WorkPackage ;
    sh:property [
        sh:path base:hasCanonicalName ;
        sh:name "Name" ;
        sh:maxcount 1 ;
    ] ;
    sh:property [
        sh:path pp:hasWPID ;
        sh:name "ID" ;
        sh:maxcount 1 ;
    ] 
    .
```


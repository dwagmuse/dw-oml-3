---
ontology: http://dw-oml-3.github.io/bundle
---

## Components using R

Here we generate the components table using R scripting

FIXME:
* how to make links work in omlcode views?
* collapse rows in table?
* styling


```r
webr::install("dplyr")
webr::install("htmlTable")
library(htmlTable)
library(dplyr)

frag <- function(iri) { parts <- strsplit(iri, "[#/]")[[1]]; tail(parts[nchar(parts) > 0], 1) }

# This isn't quite right since the
# generated links don't work the same
# as the ones in native tables
tolink <- function(iri) {
  paste0("<a href='",iri,"'>", frag(iri), "</a>")
}

tolinks <- function(iris) {
  lapply(iris, tolink)
}

result <- query("
PREFIX base: <http://dw-oml-3.github.io/foundation/base/base#> 
PREFIX sys:  <http://dw-oml-3.github.io/foundation/system/system#> 

SELECT DISTINCT ?subs ?iri ?name ?id ?t ?p

WHERE {
    ?iri a sys:Component ;
        base:hasCanonicalName ?name ;
        sys:componentId ?id  .

    GRAPH ?g {
        ?iri a ?t .
    }
    # This filters out entailed types
    FILTER (!contains(str(?g), 'entailments'))
    ?subs sys:composes ?iri .

    OPTIONAL {
        ?iri sys:presents ?p .  
    }    
} 
ORDER BY ?subs ?iri
")

# TODO: remove column g
# convert Iri column to URL
# convert Subsystem column to URL

of <- result %>% transmute(
  Subsystem=tolinks(subs),
  Element=tolinks(iri),
  Name=name, 
  Id=id, 
  Type=frag(t),
  Port=tolinks(p)
)

table <- htmlTable(of, caption="Components",
          align="l",
          align.header="l",
          rnames=FALSE) 

display(table)
```


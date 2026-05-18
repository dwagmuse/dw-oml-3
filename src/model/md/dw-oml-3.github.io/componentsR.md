---
ontology: http://dw-oml-3.github.io/bundle
---

## Components using R

Here we generate the components table using R scripting

FIXME:
* collapse rows in table?
* styling

The old htmlTable library is rather limited in its
formatting options.

```r
webr::install("dplyr")
webr::install("htmlTable")
webr::install("huxtable")
library(htmlTable)
library(huxtable)
library(dplyr)

frag <- function(iri) { parts <- strsplit(iri, "[#/]")[[1]]; tail(parts[nchar(parts) > 0], 1) }

# This isn't quite right since the
# generated links don't work the same
# as the ones in native tables
tolink <- function(iri) {
  paste0("<a class='wikilink' iri='",iri,"' href='#' title='",iri,"' >", frag(iri), "</a>")
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

table1 <- htmlTable(of, caption="Components",
          #rgroup=c('Subsystem','Element','Name','Id','Type'),
          align="l",
          align.header="l",
          rnames=FALSE) 


#display(table1)

ht <- hux(of)
escape_contents(ht) <- FALSE 
# seems like it should be easy to add rowspans here
# but the examples for hux just don't work
display(to_html(ht))
```


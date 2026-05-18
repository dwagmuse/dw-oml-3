---
ontology: http://dw-oml-3.github.io/bundle
---

# Components List 

Generated using python

TODO:
* styling

```python
import micropip
await micropip.install(['matplotlib', 'pandas'])
import pandas as pd

frag = lambda iri: (iri or '').split('#')[-1].split('/')[-1]

toLink = lambda iri: f"<a class='wikilink' iri='{iri}' href='#' title='{iri}'> {frag(iri)} </a>"


data = await query("""
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
ORDER BY ?subs ?iri ?p
""")

df = pd.DataFrame(data['rows'])
df = df[['subs','iri','id','name','t','p']]
df.rename(columns={'subs': 'Subsystem', 'iri':'Element', 't':'Type', 'p':'Port'}, inplace=True)

df['Subsystem'] = df['Subsystem'].apply(lambda x: toLink(x))
df['Element'] = df['Element'].apply(lambda x: toLink(x))
df['Type'] = df['Type'].apply(lambda x: frag(x))
df['Port'] = df['Port'].apply(lambda x: toLink(x))

df.set_index(['Subsystem', 'Element','id', 'name', 'Type'], inplace=True)

html_table = df.to_html(escape=False) 

#display(data['rows'])
display(html_table)
```

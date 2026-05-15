---
ontology: http://dw-oml-3.github.io/bundle
---
# Components

This table is generated using javascript so that we have more control over customization.

```javascript

//await load('https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.min.js');

const frag = iri => iri.split(/[#\/]/).pop();
const toId = iri => frag(iri).replace(/\W/g, '_');

function toWikiLink(iri) {
  const a = document.createElement('a');
  a.className = 'wikilink';
  a.setAttribute('iri', iri)
  a.href = "#"
  a.title = iri
  a.text = frag(iri)
  return a
}

const result = await query(`
PREFIX base: <http://dw-oml-3.github.io/foundation/base/base#> 
PREFIX sys:  <http://dw-oml-3.github.io/foundation/system/system#> 

SELECT DISTINCT ?Subsystem ?Element ?Name ?Id ?Type ?Port

WHERE {
    ?Element a sys:Component ;
        base:hasCanonicalName ?Name ;
        sys:componentId ?Id  .

    GRAPH ?g {
        ?Element a ?Type .
    }
    # This filters out entailed types
    FILTER (!contains(str(?g), 'entailments'))
    ?Subsystem sys:composes ?Element .

    OPTIONAL {
        ?Element sys:presents ?Port .  
    }    
} 
ORDER BY ?Subsystem ?Element ?Port
`);


function generateTable(data) {
  const table = document.createElement('table');
  const thead = table.createTHead();
  const tbody = table.createTBody();

  // Create Headers
  const headerRow = thead.insertRow();
  const keys = Object.keys(data[0]);
  keys.forEach(key => {
    const th = document.createElement('th');
    th.textContent = key;
    headerRow.appendChild(th);
  });

  // Create Rows
  data.forEach(item => {
    const row = tbody.insertRow();
    keys.forEach(key => {
      const cell = row.insertCell();
      const bd = item[key];
      if (bd.startsWith('http')) {
        cell.textContent = toWikiLink(item[key]);
      } else {
        cell.textContent = frag(item[key]);
      }
      
      // if item[key] starts with http we interpret 
      // as an iri and generate a link
    });
  });

  return table
}

const t = generateTable(result['rows']);
const c = document.createElement('div');
c.appendChild(t);
display(c.getHTML());

```

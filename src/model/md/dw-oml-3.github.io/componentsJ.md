---
ontology: http://dw-oml-3.github.io/bundle
---
# Components

This table is generated using javascript so that we have more control over customization.

```javascript

//await load('https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.min.js');

const frag = iri => iri.split(/[#\/]/).pop();
const toId = iri => frag(iri).replace(/\W/g, '_');

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
      cell.textContent = frag(item[key]);
    });
  });

  return table
}

display(generateTable(result['rows']).getHTML());

```

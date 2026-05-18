---
ontology: http://dw-oml-3.github.io/bundle
---
# Components

This table is generated using javascript so that we have more control over customization.

LIBRARIES
* [Danfo](https://danfo.jsdata.org/) no toHTML support
* 

TODO:
* styling
* how to limit collapse of type column to current Element


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
        sys:composedIn ?Subsystem ;
        sys:componentId ?Id  .

    GRAPH ?g {
        ?Element a ?Type .
    }
    # This filters out entailed types
    FILTER (!contains(str(?g), 'entailments'))

    OPTIONAL {
        ?Element sys:presents ?Port .  
    }    
} 
ORDER BY ?Subsystem ?Id ?Port
`);

// modify to take a column array that we can use
// to map data into ordered columns?
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
        cell.appendChild(toWikiLink(item[key]));
      } else {
        cell.textContent = frag(item[key]);
      }
    });
  });

  return table
}

function mergeRows(table, colIndex) {
  let lastValue = null;
  let firstCell = null;
  let spanCount = 1;

  // Iterate through all rows in the table body
  for (let i = 0; i < table.rows.length; i++) {
    const currentCell = table.rows[i].cells[colIndex];
    const currentValue = currentCell.innerText;

    if (currentValue === lastValue) {
      // If same value, hide current cell and increase span of the first cell
      currentCell.style.display = 'none';
      spanCount++;
      firstCell.rowSpan = spanCount;
    } else {
      // If different value, reset tracking to current cell
      lastValue = currentValue;
      firstCell = currentCell;
      spanCount = 1;
    }
  }
}

// each row is a list of tuples: key:value
// we need to reorganize this into a map with
// keys and lists of column values 

const rows = result['rows']

// for some reason the columns come out in different
// order from that specified in the SELECT

const t = generateTable(rows);
const c = document.createElement('div');
c.appendChild(t);

//display(JSON.stringify(rows[0]) )
mergeRows(t, 0)
mergeRows(t, 1)
mergeRows(t, 2)
//mergeRows(t, 3)
display(c.getHTML());

```

# shadow-cljs + mermaid bug reproduction repository

## Reproduction steps

``` shell
npm install
cp node_modules/mermaid/dist/mermaid.esm.min.mjs resources/mermaid.esm.min.js
npx shadow-cljs watch app
```

Then navigate to  in your browser.

- http://localhost:8080/expected.html
- http://localhost:8080/actual.html

Even though all we're doing is requiring the file, the SVG that gets rendered to the DOM seems malformed when you inspect it:

``` shell
document.querySelector("svg > style").innerHTML
```

### Expected

``` javascript
"#mermaid-1668207996628 {font-family:\"trebuchet ms\",verdana,arial,sans-serif;font-size:16px;fill:#333;}#mermaid-1668207996628 .error-icon{fill:#552222;}#mermaid-1668207996628 .error-text{fill:#552222;stroke:#552222;}#mermaid-1668207996628 .edge-thickness-normal{stroke-width:2px;}#mermaid-1668207996628 .edge-thickness-thick{stroke-width:3.5px;}#mermaid-1668207996628 .edge-pattern-solid{stroke-dasharray:0;}#mermaid-1668207996628 .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-1668207996628 .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-1668207996628 .marker{fill:#333333;stroke:#333333;}#mermaid-1668207996628 .marker.cross{stroke:#333333;}#mermaid-1668207996628 svg{font-family:\"trebuchet ms\",verdana,arial,sans-serif;font-size:16px;}#mermaid-1668207996628 .label{font-family:\"trebuchet ms\",verdana,arial,sans-serif;color:#333;}#mermaid-1668207996628 .cluster-label text{fill:#333;}#mermaid-1668207996628 .cluster-label span{color:#333;}#mermaid-1668207996628 .label text,#mermaid-1668207996628 span{fill:#333;color:#333;}#mermaid-1668207996628 .node rect,#mermaid-1668207996628 .node circle,#mermaid-1668207996628 .node ellipse,#mermaid-1668207996628 .node polygon,#mermaid-1668207996628 .node path{fill:#ECECFF;stroke:#9370DB;stroke-width:1px;}#mermaid-1668207996628 .node .label{text-align:center;}#mermaid-1668207996628 .node.clickable{cursor:pointer;}#mermaid-1668207996628 .arrowheadPath{fill:#333333;}#mermaid-1668207996628 .edgePath .path{stroke:#333333;stroke-width:2.0px;}#mermaid-1668207996628 .flowchart-link{stroke:#333333;fill:none;}#mermaid-1668207996628 .edgeLabel{background-color:#e8e8e8;text-align:center;}#mermaid-1668207996628 .edgeLabel rect{opacity:0.5;background-color:#e8e8e8;fill:#e8e8e8;}#mermaid-1668207996628 .cluster rect{fill:#ffffde;stroke:#aaaa33;stroke-width:1px;}#mermaid-1668207996628 .cluster text{fill:#333;}#mermaid-1668207996628 .cluster span{color:#333;}#mermaid-1668207996628 div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:\"trebuchet ms\",verdana,arial,sans-serif;font-size:12px;background:hsl(80, 100%, 96.2745098039%);border:1px solid #aaaa33;border-radius:2px;pointer-events:none;z-index:100;}#mermaid-1668207996628 :root{--mermaid-font-family:\"trebuchet ms\",verdana,arial,sans-serif;}"
```

### Actual

``` javascript
"#mermaid-1668207998698 {font-family:\",verdana,arial,sans-serif;font-size:16px;fill:#333;}{fill:#552222;}{fill:#552222;stroke:#552222;}{stroke-width:2px;}{stroke-width:3.5px;}{stroke-dasharray:0;}{stroke-dasharray:3;}{stroke-dasharray:2;}{fill:#333333;stroke:#333333;}{stroke:#333333;}{font-family:\",verdana,arial,sans-serif;font-size:16px;}{font-family:\",verdana,arial,sans-serif;color:#333;}{fill:#333;}{color:#333;}{fill:#333;color:#333;}{fill:#ECECFF;stroke:#9370DB;stroke-width:1px;}{text-align:center;}{cursor:pointer;}{fill:#333333;}{stroke:#333333;stroke-width:2.0px;}{stroke:#333333;fill:none;}{background-color:#e8e8e8;text-align:center;}{opacity:0.5;background-color:#e8e8e8;fill:#e8e8e8;}{fill:#ffffde;stroke:#aaaa33;stroke-width:1px;}{fill:#333;}{color:#333;}{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:\",verdana,arial,sans-serif;font-size:12px;background:hsl);border:1px solid #aaaa33;border-radius:2px;pointer-events:none;z-index:100;}{--mermaid-font-family:\",verdana,arial,sans-serif;}"
```

It seems like perhaps there's some kind of escaping problem. The actual generated script tag seems to be missing text that is expected to be enclosed in quotes.

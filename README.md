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

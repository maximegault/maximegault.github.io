---
title: Postman
date: 2022-06-23 09:00:00 HH:MM:SS +0200
categories: [Development, Postman]
tags: [development, postman]
---

### Retrieve a collection variables

With the `pm` object and the `collectionVariables` property:

```javascript
pm.collectionVariables.get("myVariableName");
```

### Log something

With the `pm` object and the `collectionVariables` property:

```javascript
console.log("My console log message");
```

### Perform a loop request

#### Context

Example with:

* a query on the URI `http://myUri` that returns paged results and a link to the next page in its response payload (example with 100 results)
* an `after` parameter that enables to skip results, like: `http://myUri?after=100`

So:

* first query is `http://myUri`, in its response payload the next link is `http://myUri?after=100`
* second query is `http://myUri?after=100`, in its response payload the next link is `http://myUri?after=200`
* it continues until there is no more result and so no more next link in the response payload

#### How to?

* We have a collection variable called `queryKeyAfter` which value is `after`
* We will dynamically set a variable called `after` that will contain the results to skip. It's this parameter that will enable to launch automatically a new query or not
* In the collection variables, we also have a variable named ie. `count` and set it to `0` before running the loop (only used as a queries performed counter)
* The query with pre-request and tests scripts will automatically loop
* In the query:

Pre-request script:

```javascript
// We first check if the after parameter already exist
let after = pm.collectionVariables.get(pm.collectionVariables.get("queryKeyAfter"));
// If so, we use its value and add it to the request
if (after && after.length > 0) {
    //console.log("Pre-request Script - After parameter retrieved from collection variables: " + after);
    pm.request.url.query.add(pm.collectionVariables.get("queryKeyAfter"));
    pm.request.url.query.idx(1).value = after;
}
else {
    // Display purposes only
    console.log("{");
}

// Counter
let count = pm.collectionVariables.get("count");
if(!count || count.length == 0) {
    pm.collectionVariables.set("count",  0);
}
```

Tests script:

```javascript
// Regexes for links
const regexNextLink = /^(?:\<(.*)\>; rel=\"next")$/gm;
const regexAfter = /^.*\?(?:.*&)?after=(.*)(?:&.*)$/gm;
// Keys
const headerKeyLink = "link";
const queryName = "List Users LOOP";

// gets all given header key values
function getAll(key) {
    let result = [];
    key = key.toLowerCase();
    pm.response.headers.each((header) => {
        if (String(header.key).toLowerCase() === key) {
            result.push(header.valueOf());
        }
    });
    return result;
}

function main() {
    // Display purposes
    console.log("\"req_" + pm.collectionVariables.get("count") + "\"");
    console.log(pm.response.json());
    // Get the response content
    let allLinks = getAll(headerKeyLink);    
    let nextLink = "";
    allLinks.each((currentNextLink) => {
        let currentMatchNextLink;
        while ((currentMatchNextLink = regexNextLink.exec(currentNextLink)) !== null) {
            // This is necessary to avoid infinite loops with zero-width matches
            if (currentMatchNextLink.index === regexNextLink.lastIndex) {
                regexNextLink.lastIndex++;
            }
            nextLink = currentMatchNextLink[1]; 
            
            let currentMatchAfter;
            while ((currentMatchAfter = regexAfter.exec(nextLink)) !== null) {
                // This is necessary to avoid infinite loops with zero-width matches
                if (currentMatchAfter.index === regexAfter.lastIndex) {
                    regexAfter.lastIndex++;
                }
                //console.log("Tests Script - Retrieved next after is " + currentMatchAfter[1]);
                // Next linkk is found, we set the next "after" parameter value and update the counter
                pm.collectionVariables.set(pm.collectionVariables.get("queryKeyAfter"),  currentMatchAfter[1]);
                pm.collectionVariables.set("count", pm.collectionVariables.get("count") + 1);
            }
        }
    });
    // There is still a next link
    if (nextLink !== "") {
        console.log(",");
        // Next query is launched
        postman.setNextRequest(queryName);
    }
    else {
        // The end: we remove the collection variable
        pm.collectionVariables.unset(pm.collectionVariables.get("queryKeyAfter"));
        // Display purposes only
        console.log("}");
    }
}

main();
```

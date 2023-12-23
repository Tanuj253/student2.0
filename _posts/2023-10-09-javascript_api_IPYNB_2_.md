---
title: JS API Deprecated
description: A Tech Talk on the usage of an API. This is about creating a page that is well organized and updates based on the data from the api.  Suggest lesson to use this ... https://performance-partners.apple.com/search-api
categories: ['C5.0', 'C7.0', 'C7.6']
type: ccc
---

The code below holds the info that is being <mark>generated into an HTML table</mark>.

Key things to know:
- < table > creates a TABLE 
- < tr > creates a ROW
- < th > makes the text a column HEADER
- < tbody  id = "results" > defines an element id, to be used within JavaScript



```python
%%html

<!-- HTML table fragment for page -->
<table>
  <thead>
  <tr>
    <th>Joke</th>
    <th>HaHa</th>
    <th>Boohoo</th>
  </tr>
  </thead>
  <tbody id="result">
    <!-- javascript generated data -->
  </tbody>
</table>
```

Constant variables are declared here with keyword const

Key things to know:
- The document object "result" represents table body in the HTML above.
- If you want to access any element in an HTML page in JavaScript, you always start by accessing the document object.  In this case, we are accessing "result" and defining a "resultContainer"
- In the code, in following cells, document elements are created and organized for each Joke, each is added to the "resultContainer" as a row in the table body.
- Accessing the api is done using the variables url and options, this is setup to fetch the Jokes from the backend


```python
%%js

// prepare HTML defined "result" container for new output
const resultContainer = document.getElementById("result");

// keys for joke reactions
const HAHA = "haha";
const BOOHOO = "boohoo";

// prepare fetch urls
const url = "https://flask.nighthawkcodingsociety.com/api/jokes";
const like_url = url + "/like/";  // haha reaction
const jeer_url = url + "/jeer/";  // boohoo reaction

// prepare fetch GET options
const options = {
  method: 'GET', // *GET, POST, PUT, DELETE, etc.
  mode: 'cors', // no-cors, *cors, same-origin
  cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
  credentials: 'omit', // include, *same-origin, omit
  headers: {
    'Content-Type': 'application/json'
    // 'Content-Type': 'application/x-www-form-urlencoded',
  },
};

// prepare fetch PUT options, clones with JS Spread Operator (...)
const put_options = {...options, method: 'PUT'}; // clones and replaces method
```

The below code uses a function called fetch to gather the data from the backend.   

Key things to understand:
- The "url" "response" is checked in case the site is down and returns an error
- On successful fetch, the code places each Joke in the HTML table body using a "for" loop and creating document elements from each "row" of the fetched "data".
- The creation of each Haha and Boohoo "onclick" "button" is also done in the same loop.
- Updates to backend are setup to occur with each onclick, each click calls the "reaction" function


```python
// fetch the API
fetch(url, options)
  // response is a RESTful "promise" on any successful fetch
  .then(response => {
    // check for response errors
    if (response.status !== 200) {
        error('GET API response failure: ' + response.status);
        return;
    }
    // valid response will have JSON data
    response.json().then(data => {
        console.log(data);
        for (const row of data) {
          // make "tr element" for each "row of data"
          const tr = document.createElement("tr");
          
          // td for joke cell
          const joke = document.createElement("td");
            joke.innerHTML = row.id + ". " + row.joke;  // add fetched data to innerHTML

          // td for haha cell with onclick actions
          const haha = document.createElement("td");
            const haha_but = document.createElement('button');
            haha_but.id = HAHA+row.id   // establishes a HAHA JS id for cell
            haha_but.innerHTML = row.haha;  // add fetched "haha count" to innerHTML
            haha_but.onclick = function () {
              // onclick function call with "like parameters"
              reaction(HAHA, like_url+row.id, haha_but.id);  
            };
            haha.appendChild(haha_but);  // add "haha button" to haha cell

          // td for boohoo cell with onclick actions
          const boohoo = document.createElement("td");
            const boohoo_but = document.createElement('button');
            boohoo_but.id = BOOHOO+row.id  // establishes a BOOHOO JS id for cell
            boohoo_but.innerHTML = row.boohoo;  // add fetched "boohoo count" to innerHTML
            boohoo_but.onclick = function () {
              // onclick function call with "jeer parameters"
              reaction(BOOHOO, jeer_url+row.id, boohoo_but.id);  
            };
            boohoo.appendChild(boohoo_but);  // add "boohoo button" to boohoo cell
            
          // this builds ALL td's (cells) into tr (row) element
          tr.appendChild(joke);
          tr.appendChild(haha);
          tr.appendChild(boohoo);

          // this adds all the tr (row) work above to the HTML "result" container
          resultContainer.appendChild(tr);
        }
    })
})

// catch fetch errors (ie Nginx ACCESS to server blocked)
.catch(err => {
  error(err + " " + url);
});
```

The below code uses fetch to update backend data using "put_options". The purpose is to update Hahaa and Bohoo counters in backend.

Key things to understand:
- The "url" "response" is checked to verify update occurred
- The element id of button clicked is updated with the data returned from the API.  
- Note, the elemID is received as parameter.  This data was setup when the button was created in former cell.


```python
// Reaction function to likes or jeers user actions
function reaction(type, put_url, elemID) {

  // fetch the API
  fetch(put_url, put_options)
  // response is a RESTful "promise" on any successful fetch
  .then(response => {
    // check for response errors
    if (response.status !== 200) {
        error("PUT API response failure: " + response.status)
        return;  // api failure
    }
    // valid response will have JSON data
    response.json().then(data => {
        console.log(data);
        // Likes or Jeers updated/incremented
        if (type === HAHA) // like data element
          document.getElementById(elemID).innerHTML = data.haha;  // fetched haha data assigned to haha Document Object Model (DOM)
        else if (type === BOOHOO) // jeer data element
          document.getElementById(elemID).innerHTML = data.boohoo;  // fetched boohoo data assigned to boohoo Document Object Model (DOM)
        else
          error("unknown type: " + type);  // should never occur
    })
  })
  // catch fetch errors (ie Nginx ACCESS to server blocked)
  .catch(err => {
    error(err + " " + put_url);
  });
  
}
  
// Something went wrong with actions or responses
function error(err) {
  // log as Error in console
  console.error(err);
  // append error to resultContainer
  const tr = document.createElement("tr");
  const td = document.createElement("td");
  td.innerHTML = err;
  tr.appendChild(td);
  resultContainer.appendChild(tr);
}
```

# Hacks
> The code below relates to the rapidapi you worked with last week. 
- What are some similarities you see with the javascript for the jokes api? 
- In a blogpost, break up the code in cells like done above and try to describe what this code is doing. 


```python
<!-- HTML table fragment for page -->
<table>
  <thead>
  <tr>
    <th>Time</th>
    <th>All-time Cases</th>
    <th>Recorded Deaths</th>
    <th>Active Cases</th>
  </tr>
  </thead>
  <tbody>
    <td id="time"></td>
    <td id="total_cases"></td>
    <td id="total_deaths"></td>
    <td id="active_cases"></td>
  </tbody>
</table>

<table>
  <thead>
  <tr>
    <th>Country</th>
    <th>All-time Cases</th>
    <th>Recorded Deaths</th>
    <th>Active Cases</th>
  </tr>
  </thead>
  <tbody id="result">
    <!-- generated rows -->
  </tbody>
</table>

<!-- Script is layed out in a sequence (no function) and will execute when page is loaded -->
<script>
  // prepare HTML result container for new output
  const resultContainer = document.getElementById("result");

  // prepare fetch options
  const url = "https://flask.nighthawkcodingsociety.com/api/covid/";
  const headers = {
    method: 'GET', // *GET, POST, PUT, DELETE, etc.
    mode: 'cors', // no-cors, *cors, same-origin
    cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
    credentials: 'omit', // include, *same-origin, omit
    headers: {
      'Content-Type': 'application/json'
      // 'Content-Type': 'application/x-www-form-urlencoded',
    },
  };

  // fetch the API
  fetch(url, headers)
    // response is a RESTful "promise" on any successful fetch
    .then(response => {
      // check for response errors
      if (response.status !== 200) {
          const errorMsg = 'Database response error: ' + response.status;
          console.log(errorMsg);
          const tr = document.createElement("tr");
          const td = document.createElement("td");
          td.innerHTML = errorMsg;
          tr.appendChild(td);
          resultContainer.appendChild(tr);
          return;
      }
      // valid response will have json data
      response.json().then(data => {
          console.log(data);
          console.log(data.world_total)

          // World Data
          document.getElementById("time").innerHTML = data.world_total.statistic_taken_at;
          document.getElementById("total_cases").innerHTML = data.world_total.total_cases;
          document.getElementById("total_deaths").innerHTML = data.world_total.total_deaths;
          document.getElementById("active_cases").innerHTML = data.world_total.active_cases;

          // Country data
          for (const row of data.countries_stat) {
            console.log(row);

            // tr for each row
            const tr = document.createElement("tr");
            // td for each column
            const name = document.createElement("td");
            const cases = document.createElement("td");
            const deaths = document.createElement("td");
            const active = document.createElement("td");

            // data is specific to the API
            name.innerHTML = row.country_name;
            cases.innerHTML = row.cases; 
            deaths.innerHTML = row.deaths; 
            active.innerHTML = row.active_cases; 

            // this builds td's into tr
            tr.appendChild(name);
            tr.appendChild(cases);
            tr.appendChild(deaths);
            tr.appendChild(active);

            // add HTML to container
            resultContainer.appendChild(tr);
          }
      })
  })
  // catch fetch errors (ie ACCESS to server blocked)
  .catch(err => {
    console.error(err);
    const tr = document.createElement("tr");
    const td = document.createElement("td");
    td.innerHTML = err;
    tr.appendChild(td);
    resultContainer.appendChild(tr);
  });
</script>

```

# Fetch API
 Fetch API allows you to make http request faster.
 
 ````
 fetch('http://api.example.com')
  .then(res=> {
    return res.json();
  })
  .then(result=> {
    console.log(result);
  })
 ````
 
 fetch() accept second param for configuration:
 ````
 function postData(url, data) {
  // Default options are marked with *
  return fetch(url, {
    body: JSON.stringify(data), // must match 'Content-Type' header
    cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
    credentials: 'same-origin', // include, same-origin, *omit
    headers: {
      'user-agent': 'Mozilla/4.0 MDN Example',
      'content-type': 'application/json'
    },
    method: 'POST', // *GET, POST, PUT, DELETE, etc.
    mode: 'cors', // no-cors, cors, *same-origin
    redirect: 'follow', // manual, *follow, error
    referrer: 'no-referrer', // *client, no-referrer
  })
  .then(response => response.json()) // parses response to JSON
}
 ````
 but it needs server to make cors configuration too.
 You can config the request with Request object and then fetch(yourRequest):
 ````
 const myRequest = new Request('http://localhost/flowers.jpg');
 fetch(myRequest)
  .then(response => response.blob())
  .then(blob => {
    myImage.src = URL.createObjectURL(blob);
  });
 ````

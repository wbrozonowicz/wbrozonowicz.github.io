---
title: "Long polling example in Node and React"
excerpt: "Long polling example in Node and React"
last_modified_at: 2022-08-22T10:27:01-05:00
categories:
  - JavaScript
  - React
  - Node
tags: 
  - fetch
---

<!-- short introduction -->
## Long polling example in Node and React

First server - App.js:

{% include code-header.html %}
```js
const express = require('express')
const cors = require("cors");

const app = express()
const port = 3000

let subscribers = Object.create(null);

app.use(cors({
    origin: '*'
}))

app.get('/endpoint', (req, res) => {
    res.send("mam 9")
})

app.get('/subscribe', (req, res) => {
    onSubscribe(req, res)
})

app.get('/closesubs', (req, res) => {
    close()
    res.send('ok')
})

app.post('/publish', (req, res) => {
    let message = '';
    req.on('data', function (chunk) {
        message += chunk;
    }).on('end', function () {
        publish(message); // publish it to everyone
        res.end("ok");
    });
});


app.listen(port, () => {
    console.log(`Example app listening on port ${port}`)
})


function onSubscribe(req, res) {
    let id = Math.floor(Math.random()*10000) * Date.now();
    subscribers[id] = res;
    req.on('close', function () {
        delete subscribers[id];
        console.log('deleteing subscriber ' + id)
    });
}


function publish(message) {
    for (let id in subscribers) {
        console.log('publish to subscriber ' + id)
        let res = subscribers[id];
        // res.end(message);
        res.send(message)
    }
    subscribers = Object.create(null);
}

function close() {
    for (let id in subscribers) {
        let resToClose = subscribers[id];
        console.log('close ' + id)
    }
     subscribers =  Object.create(null);
}

process.on('uncoughtException', (err) => {
    console.log(`crash...`, err.stack);
    process.exit(1);
});

```

Now client in React:


{% include code-header.html %}
```js
import './App.css';
import React, { useState } from 'react';
import API from './API'

function App() {

  const [mytext, setMytext] = useState({"name":"John", "age":30, "car":20});


  // Receiving messages with long polling
  const getSubscription = () => {
    const url =
      'subscribe';
    API.get(url).then((res) => {
      if (res.status === 502) {
        // Connection timeout (connection was pending for too long)
        // let's reconnect
        getSubscription();
      } else if (res.status !== 200) {
        // Show Error
        console.log(res.statusText);
        // Reconnect in one second
        setTimeout(getSubscription, 1000);
      } else {
        // Got message
        // console.log(res)
        // let myObject = JSON.stringify(res.data);
        // console.log(myObject)
        setMytext(res.data)
        // subcsribe next time
        getSubscription()
      }

    });
  };

  // simulate API update
  const sendData = () => {
    const out = mytext
    out.name = out.name + '1'
    setMytext(out)
    API.post('publish', out).then(
      (res) => {
        console.log('send to publish')
      },
      (error) => {
        console.log('fuck')
      }
    );
  }

  const closeSubscription = () => {
    const url =
      'closesubs';
    API.get(url).then((res) => {
      console.log('cancelled')
    })
  }


  return (
    <div className="App">
      <header className="App-header">
        <p>
          {mytext.name}
        </p>
        <button onClick={sendData}>PUBLISH data</button>
        <br />
        <button onClick={getSubscription}>SUBSCRIBE DUDE</button>
        <br />
        <button onClick={closeSubscription}>CANCEL SUBSCRIPTIONS</button>

      </header>
    </div>
  );
}

export default App;

```


Finally API file:

{% include code-header.html %}
```js
import axios from 'axios';

export default axios.create({
	baseURL: 'http://localhost:3000/',
	responseType: 'json',
});
```

That's all!







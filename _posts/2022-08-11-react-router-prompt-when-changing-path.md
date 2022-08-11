---
title: "Show prompt in REACT when user change path"
excerpt: "Show prompt in REACT when user change path"
last_modified_at: 2022-08-11T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - basics
---

<!-- short introduction -->
## Show prompt when user change path 

In some situation, when user wants to change path (go to another route in our app), We want to show him promt (if He is sure). Typical case is when form is filled but not submited - changing route in this scenerio can be by accident, so We want to prevent this with special promt (alert).
React router has special component for that - "Prompt".

Below our component with form:

{% include code-header.html %}
```js
import React from 'react';
import '../styles/ContactPageComponent.css';
import { Prompt } from 'react-router-dom';

class ContactPageComponent extends React.Component {
    state = {
        text: "",
    }

    handleSubmit = (e) => {
        e.preventDefault(); // prevent from default page reloading
        // send text to api...
        this.setState({ 
            text: "",  //reset form
        })
    }

    handleChange = (e) => {}   
            this.setState({
                text: e.target.value,
            })
    }

    render() {
        return (
            <div className="contact_form">
                <form onSubmit={this.handleSubmit}>
                    <h2>Contact:</h2>
                    <textarea value={this.state.text} onChange={this.handleChange} placeholder="Write something..."></textarea>
                    <button>Send</button>
                </form>
                <Prompt
                    when={this.state.text}
                    message="Doy You want to left page before sending message?"
                />
            </div>

        );
    }
}

export default ContactPageComponent;
```
Notes:
- Prompt component has two parameters:
1. "when" - to specify condition to show it when routes are changing. In our case, when text is not empty, prompt will be display
2. "message: - info to show on prompt

Now some simple style to our form ("ContactPageComponent.css"):
{% include code-header.html %}
```js
 .contact_form textarea {
    font-size: 14;
    width: 85%;
    min-height: 250px;
    margin: 20px 0;
    line-height: 1.5;
}

.contact button {
    display: block;
    margin: 0 15% 0 auto;
    border: 3px solid black;
    background-color: #fff;
    padding: 8 16;
    cursor: pointer;
    font-size: 18;
    transition: .3s;
}

.contact button:hover {
    color: red;
    background-color: #000;
}
```
One note:
- if We set textarea width 85%, than to align button to right edge of it, We have to set margin of button to 15% from rigth (0 15% 0 auto)


That's all!




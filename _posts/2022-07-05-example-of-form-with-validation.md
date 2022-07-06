---
title: "Form in React - example with validation"
excerpt: "Controlled component for simple React Form with validation - example"
last_modified_at: 2022-07-05T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - basics
---

<!-- short introduction -->
## Form in React - example with validation

Below simple example of Form in React with validation. 
Some notes:
- noValidate in "<form onSubmit={this.handleSubmit} noValidate>" turns off default html5 validation (We will use our own function for this)  
- componentDidUpdate() will be executed after every render (except the first one), We will use this to hide message after 5 seconds
- setTimeout() take 2 arguments: function to execute and time of delay
- if We specify htmlFor tag with the same name as input id (id="name_input"), when user click label it will activate input
- to render something only on error===true we use f.e. for passwd "{this.state.errors.passwd && something...}" - it will render something only when this.state.errors.passwd will be equal to true (error === true)


{% include code-header.html %}
```js
  import React, { Component } from 'react';
import './App.css';

class App extends Component {

  state = {
    username: '',
    email: '',
    passwd: '',
    checkbox_accepted: false,
    succes_message: '',  // text to show when validation is OK [after click submit button]

    errors: {
      username: false,
      email: false,
      passwd: false,
      checkbox_accepted: false,
    }
  }

  // messages will not be chenged, so there is no need to define them in state
  messages = {
    username_not_ok: 'Name is too short',
    email_not_ok: 'Missing @ in email',
    password_not_ok: 'Password should have 6 characters',
    checkbox_not_ok: 'Checkbox should be checked'
  }

  handleChange = (e) => {
    const name = e.target.name;
    const type = e.target.type;
    // check type of input and set state accordingly
    if (type === "text" || type === "password" || type === "email") {
      const value = e.target.value;
      this.setState({
        [name]: value  
      })
    } else if (type === "checkbox") {
      const checked = e.target.checked;
      this.setState({
        [name]: checked
      })
    }
  }

  handleSubmit = (e) => {
    e.preventDefault()  // prevent form from default submit

    const validation = this.formValidation()  // run validation function

    if (validation.form_ok) {
      // clear inputs when validation is ok (reset form)
      this.setState({
        username: '',
        email: '',
        passwd: '',
        checkbox_accepted: false,
        message: 'Form is OK',

        errors: {
          username: false,
          email: false,
          passwd: false,
          checkbox_accepted: false,
        }
      })
    } else {
      this.setState({
        // setup errors object, We have validation.username === true when it is OK, and error === true 
        // when there is error, so We have to use ! to reverse value
        errors: {
          username: !validation.username,
          email: !validation.email,
          passwd: !validation.password,
          checkbox_accepted: !validation.checkbox_accepted
        }
      })
    }
  }

  formValidation() { 
    // function that validate all inputs and return object with total state of form (form_ok = true when validation is ok) 
    // and state of each input (true or false, true if input is ok)
 
    let username = false;
    let email = false;
    let password = false;
    let checkbox_accepted = false;
    let form_ok = false;

    // if username should have more then 5 characters and not contain space
    // string.idexOf('argument') will return -1 if not find argument in string, or index of argument in string
    if (this.state.username.length > 5 && this.state.username.indexOf(' ') === -1) {
      username = true;
    }

    if (this.state.email.indexOf('@') !== -1) {
      email = true;
    }

    // password should have exactly 6 characters
    if (this.state.passwd.length === 6) {
      password = true;
    }

    // checkbox should be checked
    if (this.state.checkbox_accepted) {
      checkbox_accepted = true
    }
    
    // if all inputs are ok
    if (username && email && password && checkbox_accepted) {
      form_ok = true
    }

    return ({
      form_ok,
      username,
      email,
      password,
      checkbox_accepted
    })
  }

  // if succes message is not empty string, We will reset it after 5 seconds 
  componentDidUpdate() {
    if (this.state.succes_message !== '') {
      setTimeout(() => this.setState({
        message: ''
      }), 5000)
    }
  }

  render() {
    return (
      <div className="App">
        <form onSubmit={this.handleSubmit} noValidate>
          <label htmlFor="name_input">Name:
          <input type="text" id="name_input" name="username" value={this.state.username} onChange={this.handleChange} />
            {this.state.errors.username && <span>{this.messages.username_not_ok}</span>}
          </label>

          <label htmlFor="email_input">Email:
          <input type="email" id="email_input" name="email" value={this.state.email} onChange={this.handleChange} />
            {this.state.errors.email && <span>{this.messages.email_not_ok}</span>}
          </label>

          <label htmlFor="password_input">Password:
          <input type="password" id="password_input" name="passwd" value={this.state.passwd} onChange={this.handleChange} />
            {this.state.errors.passwd && <span>{this.messages.password_not_ok}</span>}
          </label>
          <label htmlFor="accept_checkbox">
            <input type="checkbox" id="accept_checkbox" name="checkbox_accepted" checked={this.state.checkbox_accepted} onChange={this.handleChange} /> I accept
          </label>
          {this.state.errors.checkbox_accepted && <span>{this.messages.checkbox_not_ok}</span>}
          <button>Submit</button>
        </form>
        {this.state.succes_message && <h3>{this.state.succes_message}</h3>}
      </div>
    );
  }
}

export default App;
```



That's all!




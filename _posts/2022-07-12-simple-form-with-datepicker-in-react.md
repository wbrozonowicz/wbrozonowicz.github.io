---
title: "Simple form with datepicker in REACT"
excerpt: "Simple form in React - date picker & date format"
last_modified_at: 2022-07-12T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - basics
---

<!-- short introduction -->
## Simple form in React - date picker & date format

To create very simple, basic form with datepicker, let's use below example.
We will use minimal date and maximal date (limits for user) and default date , that will be current date.
We will also format date to format "rrrr-mm-dd".
We will also have one text input and one checkbox input. Let's go!

{% include code-header.html %}
```js
import React, { Component } from 'react';
import './DatePickerFormExample.css';
class DatePickerFormExample extends Component {

  minimalDate = new Date().toISOString().slice(0, 10); // current date in "rrrr-mm-dd"
  state = {
    text: '',
    isChecked: false,
    date: this.minimalDate
  }

  handleTextChange = (e) => {
    this.setState({
      text: e.target.value
    })
  }

  handleCheckboxChange = (e) => {
    this.setState({
      isChecked: e.target.checked
    })
  }

  handleDateChange = (e) => {
    this.setState({
      date: e.target.value
    })
  }

  handleClick = () => {
    const { text, isChecked, date } = this.state;
    // do something when form is submited..
  }

  render() {
    let maximalDate = this.minimalDate.slice(0, 4) * 1 + 1; // get year from minimalDate, change to number type and add 1
    maximalDate = maximalDate + "-12-31"; // concatenation to get date in string format "rrrr-mm-dd"

    return (
      <div className="formStyle">
        <input type="text" placeholder="add text" value={this.state.text} onChange={this.handleTextChange} />
        <input type="checkbox" checked={this.state.isChecked} id="priority" onChange={this.handleCheckboxChange} />
        <label htmlFor="priority">Is priority?</label><br />
        <label htmlFor="date">Deadline</label>
        <input type="date" value={this.state.date} min={this.minimalDate} max={maximalDate} onChange={this.handleDateChange} />
        <br />
        <button onClick={this.handleClick}>Submit</button>
      </div>
    );
  }
}

export default DatePickerFormExample;
```

That's all!




---
title: "React Redux store example"
excerpt: "React Redux store example"
last_modified_at: 2022-08-28T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - hooks
  - redux
---

<!-- short introduction -->
## React Redux store example

To use Redux first: 
- npm install @reduxjs/toolkit 
- npm install react-redux

Redux will need:
- store: this is where We keep state
- actions: defined actions for manipulation of state
- reducers: functions to take state and actions and return new state

So let's create directories:
- store
- actions
- reducers

In store directory create file "store.js". We will have in uor store "tasks" array.

{% include code-header.html %}
```js
import { configureStore } from '@reduxjs/toolkit';
import { taskReducer } from '../reducers/taskReducer';

// get all reducers into one (combine to root reducer automatically by configureStore())
const reducer = {
  tasks: taskReducer,
}

const store = configureStore({
  reducer
});

export default store;
```

In actions directory create file taskActions.js:
{% include code-header.html %}
```js
export const ADD_TASK = 'ADD_TASK';
export const DELETE_TASK = 'DELETE_TASK';
export const EDIT_TASK = 'EDIT_TASK';

export const addTask = ({name, timeStamp, status}) => ({
  type: ADD_TASK,
  payload: {
    name,
    timeStamp,
    id: Math.floor(Math.random() * 1234), // generate id
    status,
  }
});

export const deleteTask = id => ({
  type: DELETE_TASK,
  // for deleting only id is needed
  payload: {
    id,
  }
});

export const editTask = ({name, id, status, timeStamp}) => ({
  type: EDIT_TASK,
  payload: {
    name,
    timeStamp,
    id,
    status,
  }
});

```

In reducers directory create file "taskReducer.js":
{% include code-header.html %}
```js
import {
  ADD_TASK, DELETE_TASK, EDIT_TASK
} from '../actions/taskActions';

// default state: empty array
export const taskReducer = (state = [], action) => {

  switch (action.type) {

    // add new -> get state and add action.payload
    case ADD_TASK:
      return [...state, action.payload];

    case EDIT_TASK:
      return state.map(currentStateElement => {
        // if id is not matched return existing element
        if (currentStateElement.id !== action.payload.id) {
          return currentStateElement;
        }
        // else take props from payload and return element with new props
        const { name, timeStamp, status } = action.payload;
        return ({
          name,
          timeStamp,
          id: currentStateElement.id,
          status,
        });
      });
    // delete task -> iterate in state (tasks) and filter matched id:
    case DELETE_TASK:
      return state.filter(currentStateElement => currentStateElement.id !== action.payload.id);

    // nt defined action type -> show warning and retirn existing state
    default:
      console.warn(`Action type not defined: ${action.type}`);
      return state;
  }
}
```

Now, how to use it:
- in file index.js add imports and store provider:
{% include code-header.html %}
```js
import React from 'react';
import { createRoot } from 'react-dom/client';
import { Provider } from 'react-redux';
import  store  from './store/store';
import App from './App';
import './index.css';

const container = document.getElementById('root');
const root = createRoot(container);

root.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);
```

In our prjects We will have:
- App.js: main cmponent
- TaskList: here We will get all tasks from store
- TaskForm: here We will have button to add/edit task 
- Task: for rendering task

So let's take a look at our components:
- App.js file:
{% include code-header.html %}
```js
import React from 'react';
import './App.css';
import TaskList from './components/TaskList';
import TaskForm from './components/TaskForm';
import { useState, useMemo } from 'react';


function App() {
  const [currentTask, setCurrentTask] = useState({
    name : '',
    timeStamp : 0,
    status :'',
    id: undefined,
});

const handleSetCurrentTask = (task) => {
  setCurrentTask(task) 
}

  return (
    <div className="App">
      Redux example
      <div>
        <TaskList handler={handleSetCurrentTask}/>
        <TaskForm {...currentTask} handler={handleSetCurrentTask}/>
      </div>
    </div>
  );
}

export default App;

```

In directory "components" files:

- TaskList.js:
{% include code-header.html %}
```js
import React from 'react';
import { useSelector } from 'react-redux';

import Task from './Task';

const TaskList = ({handler}) => {

    // get tasks from store
    const tasks = useSelector(store => store.tasks);

    const listOfTasks = tasks.map(task => (
        (<li key={task.id}><Task  {...task} handler={handler}></Task></li>)
    ))

    return (
        <div>
            <ul>
                {listOfTasks}
            </ul>
        </div>
    );
}

export default TaskList;
```

- TaskForm.js:
{% include code-header.html %}
```js
import React from 'react';
import { useDispatch } from 'react-redux';

import { addTask, editTask } from '../actions/taskActions'; // import actions to manipulate state in store

const TaskForm = (
    {
        name,
        timeStamp,
        status,
        id,
        handler
    }
) => {

    const dispatch = useDispatch();

    const resetForm = () => {
        handler({ id: undefined, name: '', timeStamp: 0, status: '' })
    }

    const handleChangeTaskName = e => {
        handler({ id, name: e.target.value, timeStamp, status })
    }

    const handleStatusChange = e => {
        handler({ id, name, timeStamp, status: e.target.value })
    }

    const handleOnSubmit = e => {
        e.preventDefault();

        const taskObject = {
            name,
            status,
            id,
            timeStamp: timeStamp > 0 ? timeStamp : new Date().getTime(), // if editing dont't change timestamp, else new stamp with current time (new task)
        };

        id
            ? dispatch(editTask(taskObject)) // edit tasks in store.tasks
            : dispatch(addTask(taskObject)); // add new task to store.tasks

        resetForm()
    }

    return (<div>
        <form onSubmit={handleOnSubmit}>
            <div>
                <label>
                    Task name:
                    <input
                        onChange={handleChangeTaskName}
                        type="text"
                        value={name}
                    />
                </label>
            </div>
            <div>
                <label>
                    Status:
                    <input
                        onChange={handleStatusChange}
                        type="text"
                        value={status}
                    />
                </label>
            </div>
            <button type="submit">
                {id ? 'Edit task' : 'Add task'}
            </button>
        </form>
    </div>);
}

export default TaskForm;
```

- Task.js:
{% include code-header.html %}
```js
import React from 'react';
import { useDispatch } from 'react-redux';
import { deleteTask } from '../actions/taskActions';


const Task = ({ id, name, timeStamp, status, handler }) => {
    const dispatch = useDispatch();

    const handleDelete = () => {
        dispatch(deleteTask(id))
    }

    return (
        <div>
            Task {id} name = {name} , status =  {status}, timestamp = {timeStamp}
            <button onClick={() => handler({ id, name, timeStamp, status })}>edit</button>
            <button onClick={handleDelete}>delete</button>
        </div>
    );
}

export default Task;
```

Summary:
- to get something frm store:  
```js
import { useSelector } from 'react-redux';
// ....
const tasks = useSelector(store => store.tasks);
```

- to manipulate state in components (f.e. delete):
```js
import { useDispatch } from 'react-redux';
import { deleteTask } from '../actions/taskActions';
// ...
const dispatch = useDispatch();
    const handleDelete = () => {
        dispatch(deleteTask(id))
    }
// ...
 <button onClick={handleDelete}>delete</button>
// ...
```

That's all!







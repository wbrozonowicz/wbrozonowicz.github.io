---
title: "React MOBX store example"
excerpt: "React MOBX store example"
last_modified_at: 2022-08-28T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - hooks
  - mobx
---

<!-- short introduction -->
## React MOBX store example

MobX is alternative library to Redux. To use it, first: 
- npm install mobx
- npm install mobx-react

MobX will need:
- Root store: this is where We will keep all stores and combine them into one
- Store Provider: wrapper that will provide store for children elements
- custom hooks to use store
- store: in this file We will keep state and actions to modify it

So let's create directories:
- stores
- components

In "stores" directory create:
- file "RootStore.js". 

{% include code-header.html %}
```js
import TasksStore from './TasksStore';

export default class RootStore {
  constructor() {
    this.tasksStore = new TasksStore();
    // other stores here...
  }
}
```

- file "StoreProvider.js"
{% include code-header.html %}
```js
import { createContext } from 'react';

import RootStore from './RootStore';

export const StoreContext = createContext()

const StoreProvider = ({children}) => (
  <StoreContext.Provider value={new RootStore()}>
    {children}
  </StoreContext.Provider>
);

export default StoreProvider;
```

- file "StoreHooks.js"
{% include code-header.html %}
```js
import { useContext } from 'react';

import { StoreContext } from './StoreProvider';

export function useTasksStore() {
  const rootStore = useContext(StoreContext);

  if (!rootStore) {
    throw new Error('No root store found');
  }

  return rootStore.tasksStore;
}
```

- file "TasksStore.js" - our store with tasks []:
{% include code-header.html %}
```js
import { observable, action, makeObservable } from 'mobx';

export default class TasksStore {
  tasks = [{
    id: 1,
    taskName: 'Learn MOBX dude!',
  }];

  constructor() {
    makeObservable(this, {
      tasks: observable,
      addTask: action,
      removeTask: action
    });
  }

  addTask = task => this.tasks.push(task);

  removeTask = id => {
    this.tasks = this.tasks.filter(
      task => task.id !== id
    );
  }
  
}
```

Now in folder components We will have Tasks.js file to show list of tasks and TaskForm,js file to add new Task:
- Tasks.js file:
{% include code-header.html %}
```js
import { observer } from 'mobx-react';

import { useTasksStore } from '../stores/StoreHooks';

const Tasks = () => {
  const { tasks, removeTask } = useTasksStore();

  const handleClick = e => {
    const id = Number(e.target.dataset.id);
    removeTask(id);
  }

  const taskList = tasks.map(task => (
    <li key={task.id}>
      <p>{task.taskName}</p>
      <button
        data-id={task.id}
        onClick={handleClick}
      >
        Delete Task
      </button>
    </li>
  ));

  return (
    <ul>
      {taskList}
    </ul>
  );
};

export default observer(Tasks);
```

- TaskForm.js:
{% include code-header.html %}
```js
import { useState } from 'react';

import { useTasksStore } from '../stores/StoreHooks';

const TaskForm = () => {
  const [inputData, setInputData] = useState('');
  const { addTask } = useTasksStore(); 

  const handleOnChange = e => setInputData(e.target.value);

  const handleOnSubmit = e => {
    e.preventDefault();

    const newTask = {
      id: Date.now(),
      taskName: inputData,
    };

    addTask(newTask);
    setInputData('');
  }

  return (
    <form onSubmit={handleOnSubmit}>
      <label>
       Add new task:
        <input
          onChange={handleOnChange}
          type="text"
          value={inputData}
        />
      </label>
    </form>
  );
};

export default TaskForm;
```

OK, now We can use it in our App.js like below:

{% include code-header.html %}
```js
import './App.css';
import Tasks from './components/Tasks';
import TaskForm from './components/TaskForm';
import StoreProvider from './stores/StoreProvider';


function App() {
  return (
    <StoreProvider>
      <div>
        <h1>Mobx EXAMPLE</h1>
        <Tasks />
        <TaskForm />
      </div>
    </StoreProvider>
  );
}

export default App;
```

Summary:
- StoreProvider is wrapper that provides RootStore for all children components
- RootStore combine every store into one Root Store
- Each store is separate file like TasksStore.js in our case
- StoreHooks.js is file with custom hooks. Each hook give us store that We want to use (from RootStore)

And to use it:
- In component We will get state from store with hook f.e. "const { tasks, removeTask } = useTasksStore();"
- To update component with state change, be sure to add observer on the end of file, f.e. in Tasks.js: "export default observer(Tasks);"

That's all!







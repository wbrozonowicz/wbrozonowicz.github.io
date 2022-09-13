---
title: "React Redux store example with slice"
excerpt: "React Redux store example with slice"
last_modified_at: 2022-08-28T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - hooks
  - Redux
---

<!-- short introduction -->
## React Redux store example with slice

Older way of using Redux: 
- separate file for reducer
- separate file for actions

Newer way:
- In one file called "slice" there are both: actions and reducers

So let's see simple and easy example:

Project organization:
- Folder: "app" -> file "store.ts" and "hooks.ts"

file "store.js":

{% include code-header.html %}
```ts
import { configureStore, ThunkAction, Action } from '@reduxjs/toolkit';
import counterReducer from '../features/counter/counterSlice';
import taskReducer from '../features/tasks/taskSlice';

export const store = configureStore({
  reducer: {
    tasks: taskReducer,
  },
});

export type AppDispatch = typeof store.dispatch;
export type RootState = ReturnType<typeof store.getState>;
export type AppThunk<ReturnType = void> = ThunkAction<
  ReturnType,
  RootState,
  unknown,
  Action<string>
>;

```

- in reducer object We can define many stores (reducers), each from separate slice

file "hooks.ts":

{% include code-header.html %}
```ts
import { TypedUseSelectorHook, useDispatch, useSelector } from 'react-redux';
import type { RootState, AppDispatch } from './store';

// Use throughout your app instead of plain `useDispatch` and `useSelector`
export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

OK, now each store will be in folder "features", in subfolder, for exampe in "tasks: will be "taskSlice":

- file "taskSlice.ts"
{% include code-header.html %}
```ts
import { createSlice, PayloadAction } from '@reduxjs/toolkit';
import { RootState } from '../../app/store';

export interface TaskItem {
  id: number,
  name: string,
}

const initialState: TaskItem[] = [{id:1,name:"Wojtek"}]

export const taskSlice = createSlice({
  name: 'tasks',
  initialState,
  reducers: {
    addTask: (state, action: PayloadAction<{id:number,name:string}>) => {
      const newTask: TaskItem = {id:action.payload.id, name: action.payload.name}
      state.push(newTask)
    },
    deleteTask: (state, action: PayloadAction<{id:number}>) => {
      return state = state.filter(task => task.id!==action.payload.id)
    },
    editTask: (state, action: PayloadAction<{id:number,name:string}>) => {
      state.forEach(task=>{
        if (task.id===action.payload.id){
          task.name=action.payload.name
        }
      })
    },
  },

});

export const { addTask, deleteTask, editTask } = taskSlice.actions;

export const selectTasks = (state: RootState) => state.tasks;

export default taskSlice.reducer;
```

OK, now We can use it in Tasks.tsx component (rendering tasks):

- file "Tasks.tsx":
{% include code-header.html %}
```ts
import React, { useState } from 'react';

import { useAppSelector, useAppDispatch } from '../../app/hooks';
import {
  addTask,
  editTask,
  deleteTask,
  selectTasks,
  TaskItem,
} from './taskSlice';
import styles from './Tasks.module.css';

export function Tasks() {
  const tasks = useAppSelector(selectTasks);

  const [selectedTaskId, setSelectedTaskId] = useState(0);
  const [taskName, setTaskName] = useState('');

  const dispatch = useAppDispatch();

  const tasksList = tasks.map(task =>
    (<li key={task.id} onClick={() => taskClickHandler(task)}>{task.id} : {task.name}</li>)
  )

   const getMaxId = () => {
    if (tasks.length>0){
     return Math.max(...tasks.map(task => task.id));
    } else 
    {
      return 0
    }
  }

  const taskClickHandler = (task: TaskItem) => {
    setSelectedTaskId(task.id)
    setTaskName(task.name)
  }

  const addClickHandler = (taskName: string) => {
    let currentMaxId: number = getMaxId()
    dispatch(addTask({ id:currentMaxId+1, name: taskName }))
  }

  return (
    <div>
      <div className={styles.row}>
        <input className={styles.textbox}
          aria-label="Task id"
          value={selectedTaskId}
          onChange={() => { }} />
        <input className={styles.textbox_long}
          aria-label="Task name"
          value={taskName}
          onChange={(e) => setTaskName(e.target.value)} />
        <button
          className={styles.button}
          aria-label="Add task"
          onClick={() => addClickHandler(taskName)}
        >
          Add
        </button>

        <button
          className={styles.button}
          aria-label="Edit task"
          onClick={() => dispatch(editTask({ id: selectedTaskId, name: taskName }))}
        >
          Edit
        </button>
        <button
          className={styles.button}
          aria-label="Delete task"
          onClick={() => dispatch(deleteTask({ id: selectedTaskId }))}
        >
          Delete
        </button>
        <ul >{tasksList}</ul>
      </div>

    </div>
  );
}
```

Notes:
We have to also add store provider, the best place is index.tsx:

- index.tsx file:
{% include code-header.html %}
```js
import React from 'react';
import { createRoot } from 'react-dom/client';
import { Provider } from 'react-redux';
import { store } from './app/store';
import App from './App';
import './index.css';
import 'bootstrap/dist/css/bootstrap.min.css';

const container = document.getElementById('root')!;
const root = createRoot(container);

root.render(
   <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
   </React.StrictMode>
);
```

That's all!







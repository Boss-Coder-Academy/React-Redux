# React(Redux)

### Agenda

1. **Introduction to React-Redux**
2. **Core Concepts**
+ Redux
+ React-Redux
3. **Setting Up React-Redux**
+ Installation
+ Configuration
4. Key Components and Terminology
+ Store
+ Actions
+ Reducers
+ Dispatch
+ mapStateToProps and mapDispatchToProps
+ Provider
4. Workflow and Usage
+ Creating Actions
+ Implementing Reducers
+ Connecting React Components
+ Dispatching Actions
5. Asynchronous Actions
+ Redux Thunk
+ Redux Saga
6. Assignment

---



### 1. Introduction to React-Redux

Redux is a predictable state container for JavaScript applications, particularly useful in managing complex data and state changes in large-scale applications. It's based on three fundamental principles:

+ **Single Source of Truth**: The entire state of the application is stored in a single JavaScript object called the store.

+ **State is Read-Only and Immutable**: The state cannot be modified directly. Instead, you dispatch actions to describe the changes you want to make.

+ **Changes are Made with Pure Functions**: To specify how the state changes in response to actions, you use reducers, which are pure functions.
.

### 2. Core Concepts

+ **Redux**

Redux revolves around the concept of a single immutable state managed by a store. Actions are dispatched to update the state using pure functions called reducers. This ensures predictability and traceability of state changes throughout the application.

+ **React-Redux**

React-Redux is the official React binding for Redux. It offers a set of APIs and components that simplify the integration of Redux with React components. It enables React components to interact with the Redux store and subscribe to updates efficiently.


### 3. Setting Up React-Redux

+ **Installation**

Install React-Redux via npm or yarn:

```bash
npm install react-redux
```

+ **Configuration**

To integrate React-Redux into a React application, follow these steps:  
         a.) Create a Redux store.  
         b.) Wrap the root component with the Provider component provided by React-Redux.

**Example**:

```javascript
Copy code
import { Provider } from 'react-redux';
import { createStore } from 'redux';
import rootReducer from './reducers'; // Combine all reducers into one root reducer

const store = createStore(rootReducer);

ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>,
    document.getElementById('root')
);
```

### 4. Key Components and Terminology

Each key component and terminology in Redux and React-Redux with detailed explanations and examples:

+ **Store**

The store is the heart of Redux. It holds the entire state tree of the application. It is created using the `createStore()` function from Redux and manages the application's state.

**Example**:

```javascript
import { createStore } from 'redux';
import rootReducer from './reducers'; // Assume rootReducer combines all reducers

const store = createStore(rootReducer);
```

+ **Actions**

Actions are payloads of information that send data from the application to the store. They are plain JavaScript objects with a `type` property that describes the action.

**Example**:

```javascript
// Action Types
export const ADD_TODO = 'ADD_TODO';

// Action Creator
export const addTodo = (text) => ({
    type: ADD_TODO,
    payload: {
        text
    }
});
```

+ **Reducers**

Reducers specify how the application's state changes in response to actions. They are pure functions that take the previous state and an action, then return the new state.

**Example**:

```javascript
// Initial state
const initialState = {
    todos: []
};

// Reducer
const todoReducer = (state = initialState, action) => {
    switch (action.type) {
        case ADD_TODO:
            return {
                ...state,
                todos: [...state.todos, action.payload]
            };
        default:
            return state;
    }
};
```

+ **Dispatch**

`dispatch()` is a method available on the store that is used to dispatch actions to the Redux store. It sends the action to the reducer, triggering a state change.

**Example**:

```javascript
store.dispatch(addTodo('Buy groceries'));
```

+ **mapStateToProps and mapDispatchToProps**

These are functions used to connect Redux state and actions to React components.

1. `mapStateToProps`: Maps parts of the Redux state to the component's props.
2. `mapDispatchToProps`: Maps action creators to the component's props.

**Example**:

```javascript
const mapStateToProps = (state) => ({
    todos: state.todoReducer.todos
});

const mapDispatchToProps = (dispatch) => ({
    addTodo: (text) => dispatch(addTodo(text))
});
```



+ **Provider**

The `<Provider>` component from React-Redux makes the Redux store available to the entire application by wrapping the root component.

**Example**:

```javascript
import { Provider } from 'react-redux';
import store from './store'; // Assume the Redux store

ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>,
    document.getElementById('root')
);
```

These key components and terminologies form the backbone of Redux and React-Redux, facilitating the management and interaction of application state in a predictable and efficient manner.


### 5. Workflow and Usage

+ **Creating Actions**

Actions are typically defined as functions that return an object with a `type` property and optional payload data. These actions are dispatched to the reducers, initiating state changes.

**Example**:

```javascript
// Action Types
export const INCREMENT = 'INCREMENT';
export const DECREMENT = 'DECREMENT';

// Action Creators
export const increment = () => ({ type: INCREMENT });
export const decrement = () => ({ type: DECREMENT });
```
+ **Implementing Reducers**

Reducers are functions that specify how the application's state changes in response to actions. Each reducer handles a specific slice of the state based on the action type.

**Example**:

```javascript
const initialState = {
    count: 0
};

const counterReducer = (state = initialState, action) => {
    switch (action.type) {
        case INCREMENT:
            return { ...state, count: state.count + 1 };
        case DECREMENT:
            return { ...state, count: state.count - 1 };
        default:
            return state;
    }
};
```

+ **Connecting React Components**

Use the `connect()` function from React-Redux to connect React components to the Redux store. `connect()` returns a higher-order component that wraps the original component, providing access to Redux state and actions as props.

**Example**:

```javascript
import { connect } from 'react-redux';

const Counter = ({ count, increment, decrement }) => {
    return (
        <div>
            <p>Count: {count}</p>
            <button onClick={increment}>Increment</button>
            <button onClick={decrement}>Decrement</button>
        </div>
    );
};

const mapStateToProps = (state) => ({
    count: state.counter.count
});

const mapDispatchToProps = {
    increment,
    decrement
};

export default connect(mapStateToProps, mapDispatchToProps)(Counter);
```

+ **Dispatching Actions**

To dispatch actions from React components, access the action creators via props and use them to dispatch actions to the Redux store.

**Example**:

```javascript
import { increment, decrement } from './actions/counterActions';

const App = ({ increment, decrement }) => {
    return (
        <div>
            <button onClick={increment}>Increment</button>
            <button onClick={decrement}>Decrement</button>
        </div>
    );
};

export default connect(null, { increment, decrement })(App);
```







### 6. Asynchronous Actions

In some scenarios, actions in Redux need to perform asynchronous tasks, such as making API requests or executing async operations before dispatching the actual action to update the store.

+ **Redux Thunk**

Redux Thunk is a middleware for Redux that allows action creators to return functions instead of plain objects. These functions have access to the `dispatch` method, enabling asynchronous logic before dispatching actions.

**Example**:

Assume an action that fetches user data from an API:

```javascript
// Action creator using Redux Thunk
export const fetchUser = (userId) => {
    return async (dispatch) => {
        dispatch({ type: 'FETCH_USER_REQUEST' });

        try {
            const response = await fetch(`https://api.example.com/users/${userId}`);
            const userData = await response.json();
            dispatch({ type: 'FETCH_USER_SUCCESS', payload: userData });
        } catch (error) {
            dispatch({ type: 'FETCH_USER_FAILURE', payload: error.message });
        }
    };
};
```

+ **Redux Saga**

Redux Saga is a middleware library that allows you to manage side effects like asynchronous operations, such as data fetching and complex control flows, in a more centralized and structured way using generator functions.

**Example**:

A Redux Saga to handle the user data fetching:

```javascript
import { takeEvery, call, put } from 'redux-saga/effects';

function* fetchUser(action) {
    try {
        const response = yield call(fetch, `https://api.example.com/users/${action.payload}`);
        const userData = yield response.json();
        yield put({ type: 'FETCH_USER_SUCCESS', payload: userData });
    } catch (error) {
        yield put({ type: 'FETCH_USER_FAILURE', payload: error.message });
    }
}

function* userSaga() {
    yield takeEvery('FETCH_USER_REQUEST', fetchUser);
}

export default userSaga;
```

**Usage**:

You need to run the saga middleware in your application:

```javascript
import { createStore, applyMiddleware } from 'redux';
import createSagaMiddleware from 'redux-saga';
import rootReducer from './reducers';
import userSaga from './sagas/userSaga'; // Assume the saga file

const sagaMiddleware = createSagaMiddleware();

const store = createStore(
    rootReducer,
    applyMiddleware(sagaMiddleware)
);

sagaMiddleware.run(userSaga);
```

+ **Redux Thunk vs. Redux Saga**

**Redux Thunk** is simpler and more straightforward for simpler async flows, especially for beginners.
**Redux Saga** is more powerful, allowing complex control flows and easier testing due to its generator-based approach.

Both middleware options offer solutions for handling asynchronous operations in Redux, catering to different complexities and preferences in managing application side effects.



### 7. Assignment

**Task 1**: Create a modal as shown on clicking the more info button, which should pop up, on clicking more info, and close on clicking the same button, as shown.

**Desired Output**:

![AltImage](https://i.postimg.cc/pLMhJCBD/image.png)


**Code**: 

+ Make a ‘components’ folder inside the ‘src’ and create a file named header.js inside this components folder as shown:

![AltImage](https://i.postimg.cc/vB4ZsxzY/install.jpg)


+ Also, create a ‘utils’ folder inside ‘src’ and a subfolder called ‘images’ inside utils and add all the required images in this folder.


![AltImage](https://i.postimg.cc/3JnVgfhq/image.png)


+ Also, add two other files to the utils folder and name them `appSlice.js` and `store.js`.
+ Make sure that Tailwind is installed in this project. 
+ Now write the following code in different files as follows:


**videoCard.js**

```javascript
import React, { useState, useEffect } from 'react';
import cardImage from '../utils/images/card.jpeg';
import playButton from '../utils/images/playbutton.png';
import likeButton from '../utils/images/white-thumbs-up-icon-26.jpg';
import DislikeButton from '../utils/images/white-thumbs-down-icon-26.jpg';
import drop from '../utils/images/dropdown_logo.png';
import {toggleDescription} from '../utils/appSlice';
import { useDispatch, useSelector } from 'react-redux';



const VideoCard = () => {

const isDescriptionVisible = useSelector((store) => store.app.isDescriptionVisible);
const dispatch = useDispatch();


const toggleDescriptionHandler = () => {
  dispatch(toggleDescription()); // Dispatch action to toggle description visibility
};



  const [likeCount, setLikeCount] = useState(0);
 


  useEffect(() => {
    // Monitor changes in likeCount if needed
    // console.log('Like count changed:', likeCount);
  }, [likeCount]);


  const handleLikeClick = () => {
    setLikeCount(likeCount + 1);
  };


  return (
    <div className="max-w-xs rounded overflow-hidden shadow-lg bg-gray-900 m-4 relative">
      {/* Video Thumbnail */}
      <img
        className="w-full h-40 object-cover object-center"
        src={cardImage}
        alt="Video Thumbnail"
      />
     
      {/* Video Information */}
      <div className="px-4 py-2 relative">
        <div className='flex justify-between'>
          <div className="font-bold text-xl mb-2 text-white">
            <button><img className='w-12' src={playButton} alt="Play Button" /></button>
            <button onClick={handleLikeClick}>
              <img className='w-7 mr-2 mb-4' src={likeButton} alt="Like Button" />
            </button>
            <button><img className='w-7 mb-4' src={DislikeButton} alt="Dislike Button" /></button>
          </div>
          <button
            onMouseEnter={toggleDescriptionHandler}
            onMouseLeave={toggleDescriptionHandler}
            onFocus={toggleDescriptionHandler}
            onBlur={toggleDescriptionHandler}
            onClick={toggleDescriptionHandler}

        >
            <img className='w-5 mb-4' src={drop} alt="Drop Button" />
            {/* Floating Description */}
            {isDescriptionVisible ? (
          <div className="absolute bg-gray-700 text-white p-1 rounded-lg shadow-md top-8 left-6 z-10 text-sm">
            {/* Description content goes here */}
            <p>Chandramukhi 2 is a 2023 Indian Tamil-language comedy horror film written and
               directed by P. Vasu.</p>
          </div>
        ) : null}
          </button>
        </div>
        <p className="text-gray-300">{likeCount} likes</p>
        <div className='flex'>
          <p className="mr-2 text-green-500">97% Match</p>
          <p className="text-gray-300 text-base">2h 35m</p>
        </div>
        <div>
          <ol className='flex'>
            <li className="text-gray-300 text-base mr-2">Horror</li>
            <li className="text-gray-300 text-base mr-2">Adventure</li>
            <li className="text-gray-300 text-base">Mystery</li>
          </ol>
        </div>
      </div>
    </div>
  );
};


export default VideoCard;

```


**App.js**


```javascript
import { Provider } from 'react-redux';
import './App.css';
import Header from './components/header';
import MainContainer from './components/mainContainer';
import store from "./utils/store";




function App() {
  return (
    <Provider store={store}>
    <div>
      <Header/>
      <MainContainer />
    </div>
    </Provider>
  );
}


export default App;
```



**appSlice.js**


```javascript
// AppSlice.js

import { createSlice } from "@reduxjs/toolkit";

const appSlice = createSlice({
    name: "app",
    initialState : {
       isDescriptionVisible: false, // Add a new state for description visibility
    }, 
    reducers : {
        
        toggleDescription: (state) => { // Toggle description visibility
             state.isDescriptionVisible = !state.isDescriptionVisible;
        }
        
    }
});

export const { toggleDescription} = appSlice.actions;
export default appSlice.reducer;


```


**Store.js**

```javascript
import { configureStore } from "@reduxjs/toolkit";
import appSlice from "./appSlice";


const store = configureStore({
    reducer: {
        app: appSlice,
    }
})

export default store
```



**Task 2**: Use react-redux in order to implement the like functionality.

**Desired Output**:

![AltImage](https://i.postimg.cc/kGHzKB35/dasc.jpg)













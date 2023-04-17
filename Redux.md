
# Redux

js
naming convenctions
design patterns

- Predictable state container for js apps
- can be used for any framework or venilla js
- stores the state of the ui
- state represents the state of all individual components of the app

## Terms
- Store: holds the state of the app
- Action: Describes what to happen
- Reducer: Ties the Store and actions together

## Three Principles
- The State of whole app is stored in an object tree within a single store
- Only way to change the state is to emit an action, an object descrbing what happened
- to specify how the state tree is transformed by actions, you write pure reducers
Reducer - (previous state, action) => new State

## Actions
- ways to interact with the store
- plain js objects with 'type' property
- have a 'type' property that indicates the type of action performed
```
const BUY_CAKE  = 'BUY_CAKE'

{
    type: BUY_CAKE;
    info: 'first redux action'
}

// action creator
function buyCake(){
    return {
        type: BUY_CAKE,
        info: 'first redux action'
    }
}
```

## Reducers
- Specify how the app's state changes in response to the actions sent to the store
- function that accepts state and action as args and return next state of app
```
// state
const initialState = {
    numOfCakes: 10
}

// reducer
const reducer = (state = initialState, action) => {
    switch(action.type){
        case BUY_CAKE: return {
            ...state, // copy all the other states
            numOfCakes: state.numOfCakes - 1 // and just update this state
        }
        default: return state
    }
}
```

## Redux Store
- holds app state
- allows access to state via getState()
- allows state to update via dispatch(action)
- registers listeners via subscribe(listener)
- unregisters listeners via function returned by subscribe()

```
const redux = require('redux')
const createStore = redux.createStore


const store = createStore(reducer)
console.log('Initial State', store.getState())
const unsubscribe = store.subscribe(() => console.log('updated state', store.getState()))
store.dispatch(buyCake())
store.dispatch(buyCake())
store.dispatch(buyCake())
unsubscribe()
```

lendleasegroup.atlassian.net/wiki/spaces/PDP/pages/1205535587

## Multiple reducers
- Define multiple actions and states for icecream and cake
```
const combineReducers = redux.combineReducers

// action creators
function buyCake(){}
function buyIcecream(){}


// individul reducers
const cakeReducer = () => {}
const icecreamReducer = () => {}

const rootReducer = combineReducers({
    cake: cakeReducer,
    iceCream: iceCreamReducer
})
const store = createStore(rootReducer)
store.getState()
store.subscribe(func)
store.dispatch(buyCake())
store.dispatch(buyIcecream())

NOTE: if number of cakes needs to be accessed, need to use state.<key>.prop like state.cake.numOfCakes where <key> is one specified in combineReducers
```

## Middleware
- provides third party extension point b/w dispatching an action and moment it reaches the reducer
- used for logging, crash reporting, performing async tasks etc

```
# using logger middleware
const reduxLogger = require('redux-logger')

const applyMiddleware = redux.applyMiddleware
const logger = reduxLogger.createLogger()

# in store, pass 2nd arg
const store = createStore(rootReducer, applyMiddleware(logger))
```

## async fetch
- using middleware: redux-thunk, axios
`fetchasync.js`



# Redux in React
```
npm install redux react-redux
```
- create redux folder in src for redux code and individual folders for each features

### cakeTypes
```
- /components/redux/cake/cakeTypes.js
# define and export types 

export const BUY_CAKE = 'BUY_CAKE'
```

experience apps
redux dynamic module

### cakeActions

```
- /components/redux/cake/cakeActions.js
import { BUY_CAKE } from "./cakeTypes"

// action creator
export const buyCake = () => {
    return {
        type: BUY_CAKE
    }
}
```
### cakeReducer

```
- /components/redux/cake/cakeReducer.js
import { BUY_CAKE } from "./cakeTypes"

const initialState = {
    numOfCakes : 10
}

const cakeReducer = (state= initialState, action) => {
    switch(action.type){
        case BUY_CAKE: return {
            ...state,
            numOfCakes: state.numOfCakes - 1
        }
        default: return state
    }
}

export default cakeReducer;
```

### createStore()
```
- redux/store.js
import { createStore } from 'redux'
import cakeReducer from './cake/cakeReducer'

const store = createStore(cakeReducer)
export default store
```

- in app.js include the following
```
import { Provider } from 'react-redux';
import store from './redux/store';

...

    <Provider store={store}>
    <div className="App">
      <CakeContainer/>
    </div>
    </Provider>
```

### Component code
cakeContainer.js component
```
import React from "react";
import { connect } from "react-redux";
import {buyCake} from '../redux'

function CakeContainer (props){
    return (
        <div>
            <h2>Number of cakes = {props.numOfCakes}</h2>
            <button onClick={props.buyCake}>buy cake</button> 
        </div>
    )
}

const mapStateToProps = state => {
    return {
        numOfCakes: state.numOfCakes
    }
}

const mapDispatchToProps = dispatch => {
    return {
        buyCake: () => dispatch(buyCake())
    }
}

export default connect (
    mapStateToProps,
    mapDispatchToProps
)(CakeContainer)
```

# ReactRedux + Hooks

## useSelector()
- hook which is equivalent to mapstateToProps function
- used to access any state that's maintained in the redux store

```
import {useSelector} from 'react-redux'

function Component(){
    const numOfCakes = useSelector(state => state.numOfCakes)
    return (
        <div>
            h1 numOfCakes = {numOfCakes} </h1>
        </div>
    )
}
```
## useDispatch()
- returns a reference to the dispatch function from the redux store
```
function comp(){
    ...useSelector...
    const dispatch = useDispatch()

    return (
        <button onClick= {() => dispatch(buyCake())}> BuyCake </button>
    )
}
```

# Redux Logger Middleware

```
# install the package
npm i redux-logger

# import in store.js file
import logger from 'redux-logger'

# apply middleware
import {applyMiddleware} from 'redux'

const store = createStore(rootReducer, applyMiddleware(logger))

# logs are printed to the console
```

# Payload
- pass additional data

```
# in the component
const [number, setNumber] = useState(1)
return
    <input type='text' value={number} onChange={e => setNumber(e.target.value)} />
    <button onClick={() => props.buyCake(number)}> Buy Cake </button>

mapDispatchToProps:
    return{
        buyCake: number => dispatch(buyCake(number))
    }

# cakeActions.js
export const buyCake = (number=1) => {
    return {
        type: BUY_CAKE,
        payload: number
    }
}

# cakeReducer.js
case BUY_CAKE:
    numOfCakes: state.numOfCakes - action.payload
```

# Multiple Reducers
(todo)

# Redux-Dynamic-Modules
- Group redux artifacts

```
- component-module.js
function getMyModule(){
    return {
        // unique id of the module
        id: "my-module",

        // one or more state keys and reducers
        reducerMap: {
            myState: myReducer
        },

        // middlewares/sagas
        sagas: [mySaga]
    };
}
```

- create a Store
```
configureStore (
    initialState,

    // use sagas in the store
    [getSagaExtension()],

    //Register the module
    getMyModule
);
```

- inject additional modules at runtime
When the Higher order Component is mounted, the modules are added and removed dynamically
```
const DynamicWidget = () => {
    return {
        <DynamicModuleLoader
            modules={[getWidgetModule()]}
        >
            <ConnectedFooContent>
        </DynamicModuleLoader>
    }
}
```

# Redux dev tools
```
- using npm package
npm install --save @redux-devtools/extension

- in store.js
import { composeWithDevTools } from '@redux-devtools/extension';
const store = createStore(
    Reducer,
    composeWithDevTools()
)
```

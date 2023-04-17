
# Hooks

## rules for hooks
- Only call hooks at the top level
Dont call hooks inside loops, conditions or nested functions

- Only call hooks from the react functions
call them from within react Functional components and not from any regular js function


## useState()
- accepts args which is the initial value of state property
- returns the current value of state property and a method thats capable of updating that state property
- updates the state and rerenders the ui when the state is updated

```
import {useState} from 'react'

function HookCounter(){
    const [count, setCount] = useState(0)

    return(
        <button onClick={() => setCount(count+1)}> Count {count} </button>
    )
}
```

## using previous state
- the previous code of accessing the previous state is not right for the following
```
# this wouldnt work
const incrementFive = () => {
    for(int i=0; i<5; i++){
        setCount(count + 1)
    }
}

```

```
# instead of new state, a function is passed to setCount whose arg is previous state
const incrementFive = () => {
    for(int i=0; i<5; i++){
        setCount(prevCount => prevCount + 1)
    }
}
```

### useState with object
```
const [name, setName] = useState({firstName: '', lastName: ''})

# required to use spread operator
setName({...name, firstName: 'value'})
setName({...name, lastName: 'value'})
```

### useState with array
```
const [items, setItems] = useState([])
const addItems = () => {
    setItems([...items, {
        id: items.length,
        value: 5
    }])
}

<ul>
    {items.map(item => {
        <li key={item.id}>{item.value}</li>
    })}
</ul>
```

# useEffect() hook
- performs sideeffects in functional components
- close replacement for componentDidMount, componentDidUpdate, componentWillUnmount

```
# useEffect calls the passed function eachtime the ui is re-rendered

import {useState, useEffect} from 'react'

    const [count, setcount] = useState(0)

    useEffect(() => {
        document.title = `you clicked ${count} times`
    })

    return(
        <div>
            <button onClick={() => setCount(count+1)}>Click {count}</button>
        </div>
    )
```

### conditionally run useEffect
```
# in class component,
componentDidUpdate(prevProps, prevState){
    if(prevState.count !== this.state.count){
        update ui only if the required variable changes
    }
}
```

```
# the effect is run only if the variables in the list change

useEffect(() => {
    console.log('updating ui')
}, [<list of variables that are watched to update the ui>])

```

### running only once(like componentDidMount)
- useEffect accepts second arg as a dependency list. useEffect will run whenever any var in dependency list changes.
- Here, Empty array as second arg specifies to run the effect only once after initial render like componentDidMount

```
useEffect(() => {
    window.addEventListener('mousemove', logMouseMove)
}, [])

```

### useEffect like componentWillUnmount()
- used to write cleanup code to remove any listeners attached to an unmounted component
- The useEffect returns a function which is executed when component is unmounted

```
useEffect(() => {
    window.addEventListener('mousemove', callbackFunc)

    return ()=> {
        # unmounting code
        window.removeEventListener('mousemove', callbackFunc)
    }
}, [])
```

# Context API
- Context: provides a way to pass data through the component tree without having to pass props down manually at every level

# useContext() hook
- does the same as context api

```
# creating and providing a context at the parent component(just like context api)

export const UserContext = React.createContext()
export const ChannelContext = React.createContext()

function App(){
    return(
        <div className = 'App'>
            <UserContext.Provider value={'Alice'}>
                <ChannelContext.Provider value={'channel'}>
                    <Mycomponent/>
                </ChannelContext.Provider>
            </UserContext.Provider>
        </div>
    )
}
```

```
# consuming value in child component

import React, {useContext} from 'react'

# get our contexts defined in parent
import { UserContext, ChannelContext } from '../App'

function Mycomponent() {
    const user = useContext(UserContext)
    const channel = useContext(ChannelContext)
}
```

# useReducer() hook
- used to manage state like useState()
- useReducer(reducer, initialState)
- newState = reducer(currentState, action)
- useReducer returns a pair of values(newState, dispatch)

- example
```
import React, {useReducer} from 'react'

const initState = 0
const reducer = (state, action) => {
    switch(action) {
        case 'increment':
            return state+1
        case 'decrement':
            return state-1
        case 'reset':
            return initState
        default:
            return state
    }
}

function Component(){
    const [count, dispatch] = useReducer(reducer, initState)
    return(
        <div>
            <button onClick={() => dispatch('increment')}>increment</button>
            <button onClick={() => dispatch('decrement')}>decrement</button>
            <button onClick={() => dispatch('reset')}>reset</button>
        </div>
    )
}
```

## useReducer() with useContext()
- required when there are many nested components that share the same prop
```
- Define initialState and reducer in app.js

- provide the dispatch functon to the components via the the context api
export const CountCount = React.createContext()

function App(){
    const {count, dispatch} = useReducer(reducer, initialState)
    return(
        <CountContext.Provider value={{countState: count, countDispatch: dispatch}}>
            ...
        </CountContext.Provider>
    )
}

- use the context
inport {useContext} from 'react'
function Comp(){
    ...
    const countContext = useContext(CountContext)
}
```
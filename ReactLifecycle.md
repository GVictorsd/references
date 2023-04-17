
# Lifecycle Methods

- Mounting: when an instance of a component is being created and inserted into the Dom
- Updating: When rerendered as a result if changes to either props or state
- Unmounting: When a component is being removed from the Dom
- Error Handling: when an Error encountered during rendering, in a lifecycle method, or in the constructor of any child components

## Methods
- Mounting
```
- constructor
- static getDerivedStateFromProps
- render 
- componentDidMount
```

- Updating
```
static getDerivedStateFromProps
shouldComponentUpdate
render
getSnapshotBeforeUpdate
componentDidUpdate
```

- Unmounting
```
componentWillUnmount
```

- ErrorHandling
```
static getDerivedStateFromError
componenttDidCatch
```

## Mounting

### Constructor
- initializes state, binding event handlers
- do not cause side effects(http requests etc)
- super(this)
- directly overwrite this.state

### static getDerivedStateFromProps(props, state)
- When state of the comp depends on the changes in props over time
- set the state
- cant access this since the method is static
- Do not cause sideeffects

### render
- only required method
- returns jsx 
- dont change state or interact with DOM or make ajax calls
- children components lifecycle methods are also executed

### ComponentDidMount()
- called when the component and all its children comps are rendered
- can cause sideeffects

## children Components
- assuming the component (compA) has a child component(compB) the order is as follows
```
compA constructor
compA getDerivedStateFromProps
compA render
compB const
compB GDSFP
compB render
compB compDidmount
compA CompDidMount
```
- Example
```
class LifecycleA extends Component {
    constructor(props){
        super(props)

        this.state = {
            name: 'Alice'
        }
        console.log('lifeCycleA Constructor')
    }

    static getDerivedStateFromProps(props, state){
        console.log('lifecycleA getDerivedStateFromProps')
        return null
    }

    componentDidMount(){
        console.log('lifecycleA componentDidMount')
    }

    render(){
        console.log('lifecycleA render method')
        return(
            jsx
        )
    }
}
```

## Update Lifecycle
comp being rerendered due to change in state

### static getDerivedStateFromProps(state, props)
### shouldComponentUpdate(nextProps, nextState)
- dictates if the component should re-render or not by returning true or false
- used for perf optimization
- dont cause side effects or call setstate()
### render
### getSnapshotBeforeUpdate(prevProps, prevState)
- called right before changes from virtual DOM are to be reflected in the DOM
- capture some info from the dom
- return either null, or a value. return value will be passed as a third parameter to next method
### componentDidUpdate(prevProps, prevState, snapshot)
- called when rerender is finished
- can cause sideeffects

## Unmounting phase
### componentWillUnmount()
- Invoked immediatly before a component is unmounted and destroyed
- used to cancelling any network requests, removing event handlers, cancelling subscriptions, invalidating timers
- do not call setstate since the component will never rerender again

## Error Handling phase
### static getDerivedStateFromError(error)
### componentDidCatch(error, info)

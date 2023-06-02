
# React Router

## Paths
/ - home.js
/contact - contact.js
/about - about.js

## Installing
```
npm install react-router-dom@6.4
```

## Using in older versions(before v6.4)
```
import {BrowserRouter, Routes, Route, Link, NavLink} from 'react-router-dom'

function app(){
    return (
        <BrowserRouter>
            <header>
                <nav>
                    <Link to = "/">Home</Link>
                    <NavLink to="about">About </NavLink>
                </nav>
            </header>

            <main>
                <Routes>
                    <Route path='/' element={<Home />} />
                    <Route path='/about' element={<About/>} />
                </Routes>
            </main>
        </BrowserRouter>

    )
}

# import each of the <Home/> and <About /> components
```
- NavLink automativally changes the class of the element to `active` when the navlink is active

```
header nav{
    display: flex;
    gap: 16px;
    justify-content: end;
    max-width: 1200px;
    margin: 0 auto;
}
header nav h1 {
    margin-right: auto;
    border-bottom: 3px solid var(--primary);
}

header nav a{...}
header nav a:active{
    background: var(--primary)
}
```

## Passing props via link and navlink
```
# for Link
<Link to={{
    pathname: '/home',
    state: {name: 'from home page'}
}}> abc </Link>

# navLink
<NavLink to={{
    pathname: '/home',
    state: {title: 'from home page'}
}}> abc </NavLink>

# accessing in the component using hooks
import {useLocation} from 'react-router-dom'
function App(){
    let location = useLocation();
    console.log(location)
    return...
}

# for class comp
console.log(this.props.location)
```

## New way
```
import {
    createBrowserRouter,
    Router,
    Link,
    NavLink
} from 'react-router-dom'

const router = createBrowserRouter(
    createRoutersFromElements(
        <Routes>
            <Route ..>
            <Route ..>
        </Routes>
    )
)
```
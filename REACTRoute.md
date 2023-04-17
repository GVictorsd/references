
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
                    <Link to = "/">Home<Link>
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

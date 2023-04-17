
# Storybook app

## Installing
```
npx create-react-app
npx sb init
npm run storybook

# in case of error: Module not found: Can't resolve '@testing-library/dom'
npm install --save-dev @testing-library/dom
```

## Creating a component
```
#Component.js

import React from "react"
import './Button.css'

function Button(props
    const {variant = 
    return (
        <button class
            {children
        </button>
    );
}

export default Button
```

```
# component.css
.button {
    color: white;
    padding: 15px 12px;
    text-align: center;
    cursor: pointer;
    font-size: 16px;
    display: inline-block;
}

.primary {background-color: blue;}
.secondary {background-color: white; color: black;}
.success {background-color: green;}
.danger {background-color: red;}
```

```
# Component.stories.js

import React from "react";
import Button from './Button'

export default {
    title: 'Button2',
    component: Button,

    // sets default args for all components if not overwritten by individuals
    args :{
        children: "default child"
    }
}

export const Primary = () => <Button variant='primary'> Primary </Button>
export const Secondary = () => <Button variant='secondary'> secondary </Button>
export const Success = () => <Button variant='success'> success</Button>
export const Danger = () => <Button variant='danger'> danger</Button>

// changes the name in storybook without changing the export name
// Primary.storyName = 'primary button'


// using args 
const Template = args => <Button {...args}/>

export const PrimaryA = Template.bind({})
PrimaryA.args = {
    variant: 'primary',
    children: 'Primary Args'
}

export const SecondaryA = Template.bind({})
SecondaryA.args = {
    variant: 'secondary',
    children: 'secondary Args'
}

// extending from other args
export const extendedPrimaryA = Template.bind({})
extendedPrimaryA.args = {
    // get args of PrimaryA
    ...PrimaryA.args,   
    // overwrite the children arg
    children: 'extended args'
}
```
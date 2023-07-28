# Angular

## Installing 
```
npm i @angular/cli -g
ng --version

# create workspace with default app
ng new <workspace-name>

# create empty workspace
ng new <workspace-name> --createApplication=false
# add application to it
# ng generate
ng g app <appName>
```

## files
```
tsconfig.json: ts configurations
tsconfig.spec.json: used by unit test files
tsconfig.app.json: used by app files
angular.json: info about workspace
.browserlistrc: supported browsers

src/
test.ts: used for testing
style.scss: global styles
polyfills.ts: imports required to support different browsers
main.ts: start point of app
index.html
environment/ : enviromnent variables
app/ : app files
```

## running locally
```
ng serve
ng serve -o
```

### notes
- every angular app has atleast one module(root module)
- each module is made of components and services
- every module has atleast one component (RootComponent)
- each component has a html template for view and class for logic
- Services is a class that holds business logic of the app

### execution flow
```
ng serve => main.ts => app.module.ts => AppComponent
```

### Component
- Template - view, html
- class - code ts, data and methods
- Metadata - information decorator
The component decorator is used before a component holding metadata telling that the class is a component

```
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {
  title = 'myapp';
}
```
- selector - custom html tag that can be used to represent the component
- templateURL - represents the file that holds the template for the class
- styleUrls - path to the style file

### creating new component
```
# using angular cli
# ng generate component <name>
ng g c <name>
ng g c test
```
### using component as class and attribute
```
# start selector with a dot to use as class
@Component({
  selector: '.app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
# in import html
<div class="app-root"></div>


# Enclose in [] for attribute
@Component({
  selector: '[app-root]',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
# in import html
<div app-root ></div>
```

### inline template
declare template inside the component.ts file
```
@Component({
  selector: '[app-root]',
  template: '<div>inline template</div>',
  # for multiple lines
  template: `
        <div>
        </div>
    `,


  styleUrls: ['./app.component.scss']
  # inline styles:
  styles: [`
        div(
            color: 'red'
        )
    `]
})
```

### Interpolation
- using class property inside template using double curly braces
```
template{
    <h1>hello {{name}}</h1>
}
class ...{
    public name = 'hello'
}
```
js expressions can be used inside double curly braces
```
{{2+2}}
{{name.length}}
{{name.toUpperCase()}}

{{mymethod()}} 
class ...{
    ...
    mymethod(){
        return "hello";
    }
}
```

Following CAN"T be used in {{}}:
```
# assignments
{{a = 2 + 2}}

# accessing global variables like document, window etc
{{window.location.href}}

These needs to be done inside the class
```

## Property binding
Attributes vs Properties
- attr are defined by html
- properties are defined by dom
- attr initilize dom properties and done(they are not changed once initialized)
- property values can change
eg;
```
for 
<input type='text' value="initValue">
# if this element is inspected in console
> $0.getAttribute('value')
>> initValue
> $0.value
>> initValue

# on changing value in input text field in ui to "apple"
> $0.getAttribute('value')
>> initValue
> $0.value
>> apple
```

with property binding, we are binding to the property of the dom element
property binding is setting properties of the html elements to class properties
```
curly braces for myId not required
<input [id]="myId">
class{
    public myId = "testid";
}

# simple interpolation won't work for the above for boolean properties like disabled
<input disabled="false"> will be disabled even when false is set
<input [disabled]="isDisabled"> will undisable it
```
- alternate syntax
```
instead of [property]="" used for binding we can use
bind-property=""
```
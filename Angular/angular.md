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

## class Binding
```
<h2 class="text-success">

# class binding
public successClass = "text-success"
<h2 [class]="successClass" >

# when using literal class name and class binding, literal class is ignored
# here only successClass is applied
<h2 class="text-error" [class]="successClass" >

# conditionally applying class names
# here text-danger is css class name and hasError is a property of the class
public hasError = true/false
<h2 [class.text-danger]="hasError">

# conditionally applying multiple classnames (ngClass)
public hasError = false;
public isSpecial = true;
public messageClasses = {
  "text-success": !this.hasError,
  "text-danger": this.hasError,
  "text-special": this.isSpecial
}
<h2 [ngClass]="messageClasses"></h2>
```

## Style Binding
```
<h2 [style.color]="orange">

# conditional styling
<h2 [style.color]="hasError ? 'red' : 'orange'">

public highlightColor = "orange";
<h2 [style.color]="highlightColor">

# multiple styles(ngStyle)
public titleStyles = {
  color: "blue",
  fontStyle: "italic"
}
<h2 [ngStyle]="titleStyles">
```

## Event Binding
class --[Data Binding]--> Template
Template --[Event Binding]--> Class

```
<element (event)="eventHandler()">...</element>

<button (click)="onClick()">
class method:
onClick(){
  ...
}

# getting info about the event($event)
<button (click)="onClick($event)">

# without separate event handler method
<button (click)="<js to be executed>">
<button (click)="greeting = 'hello'">
for public greeting = ''
```

## Template reference variables
Reference variable is used to refer to an html element from another html element
variable specified by `#variableName`
```
# log the content of input to console when button clicked
<input #myInput type="text>
<button (click)="logMessage(myInput.value)">Log</button>

logMessage(value) {
  console.log(value)
}
```

## Two way binding
keeps view and model in sync
update the dom whenever a property changes and viceversa

```
<input [(ngModel)]="name" type="text">
{{name}}

public name = "";

# here value "name" flows from view (input) to model and then to view ({{name}})

note: forms module needs to be imported in app.module.ts file
import {formsModule} from '@angular/forms'
# in imports array
imports: [
  ...,
  FormsModule
]
```

## Structural Directives
## ngIf
- ngIf
- ngSwitch
- ngFor

```
<h2 *ngIf="true/false">

<h2 *ngIf="displayName">
public displayName = true/false;

# if-else
<h2 *ngIf="displayName; else elseBlock">

<ng-template #elseBlock>
  <h2> show if displayName is false
</ng-template>

# alternate syntax
<div *ngIf="displayName; then thenBlock; else elseBlock"></div>

<ng-template #thenBlock>
  <h2>...
</ng-template>

<ng-template #elseBlock>
  <h2>...
</ng-template>
```

## ngSwitch
```
<div [ngSwitch]="color">
  <div *ngSwitchCase="'red'">you picked red</div>
  <div *ngSwitchCase="'blue'">you picked blue</div>
  <div *ngSwitchCase="'green'">you picked green</div>
  <div *ngSwitchDefault>Default</div>
</div>

public color = 'red';
```

## ngFor

```
<div *ngFor="let color of colors">
  <h2>{{color}}</h2>
</div>

public colors = ['red', 'blue', "green", "yellow"];

# get indexes
<div *ngFor="let color of colors; index as i">

# indicates if an element is first element or not
<div *ngFor="let color of colors; first as f">
<div *ngFor="let color of colors; last as l">
<div *ngFor="let color of colors; odd as o">
<div *ngFor="let color of colors; even as e">
```

## Component Interaction
Interaction between child and parent components
- Input and output decorators: @Input[], @Output[]

```
#### from parent to child
## app.component
<app-test [parentData]="name">

## test.component
import {Input} from '@angular/core'
# use same name as parent
@Input() public parentData;
# use a different name
@Input('parentData') public name;


#### from child to parent
- using events
### test component
import {EventEmitter, Output} from '@angular/core'
<button (click)="fireEvent()">

@Output() public childEvent = new EventEmitter();

fireEvent(){
  this.childEvent.emit('Message to parent')
}

### app.component
<app-test (childEvent)="message=$event">
public message = ""
```

## Pipes
- allow transforming data before displaying them in view
```
## FOR STRING TYPE
# convert to lowercase
<h2>{{ name | lowercase }}</h2>
<h2>{{ name | uppercase }}</h2>
# capitalize every first letter of the word
<h2>{{ name | titlecase }}</h2>
# string starting from index 3
<h2>{{ name | slice:3}}</h2>
# starting at 3 not including 5
<h2>{{ name | slice:3:5}}</h2>
# display json
<h2>{{ person | json}}</h2>

## FOR NUMBERS
# <minimum no of integer digits>.<min no of decimal digits>-<max no of decimal digits>
<h2>{{5.678 | number: '1.2-3'}}

<h2>{{0.25 | percent}}</h2>
<h2>{{0.25 | currency}}</h2>
# currency = great Britin Pounds
<h2>{{0.25 | currency:'GBP'}}</h2>

## FOR DATES
<h2>{{date}}</date>
<h2>{{date | date: 'short'}}</date>
<h2>{{date | date: 'shortDate'}}</date>
<h2>{{date | date: 'shortTime'}}</date>

public date = new Date();
public name = "name";
```

## Services
A class with a specific purpose

Used when need to:
- Share date
- Implement app logic without view
- External interaction
naming convention: .service.ts

## Dependency Injection
DI is a coding pattern in which a class gets its dependencies from external sources rather than creating them itself

```
# this method is not flexiable because is we change engine class to accept a arg, we need to change the car constructor as well
class Engine()
class Wheels()
class Car(){
  constructor(){
    engine = new Engine()
    wheel = new Wheel()
  }
}

# with dependency injection
var engine = new Engine()
var wheel = new Wheel()
var car = new Car(engine, wheel)
# above approch fails when there are many dependencies which inturn can have more dependencies
# angular dependency framework is used to manage them
```
## angular dependency framework
- Injector: Register all the dependencies
`Injector[ Engine, tires, depA, depB, ...] => Car`

### FOR EMPLOYEE SERVICE
- Define the EmployeeService class
- Register with Injector
- Declare as dependency in required classes that need the service

## Using a Service
```
# generate service template
ng <generate(g)> <service(s)> <name>
ng g s employeeService
```
- add a method to employeeService class that returns the required data
```
## employee.service.ts
getEmployeeService{
  return [...]
}
```
- register the service to angular injector
Angular injector is hearchial, ie, all the modules below the component  in which the service is registered will be able to use the service

```
## app.module.ts
import EmployeeService;
@NgModule({
  ...
  providers: [EmployeeService]
})
```
- Mention the dependency in the required module
```
class{
  constructor(private _employeeService: EmployeeService){}
  ngOnInit(){
    this.employees = this._employeeService.getEmployees();
  }
}
```
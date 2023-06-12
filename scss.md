
# SCSS

## Setting up
```
npm i node-sass

# in package.json(add the script)
{
  "dependencies": {
    "node-sass": "^9.0.0"
  },
  "scripts": {
    "scss": "node-sass --watch scss -o css"
  }
}
```

## Comments
- `//` : single line comments, silent comments, not included in css
- `/* ... */` : multiline comments, loud comments, included in css, may contain interpolation which will be evaluated before the comment is compiled
- `/*! ... */` : Multiline comments are removed in when compiled in compressed mode. This ensures that they stay nevertless.

- `///` : Documentation comments, placed above the thing thats being documented

## Variables

```
$var1 : <value1>
```
- note: underscores and hyphens are same in any SCSS identifiers and variable names 
```
$my_variable == $my-variable
```

- CSS variables can have different values for different elements
- CSS variables are imperative which means when the value of a variable changes, it's previous used instances also change unlike SCSS variables

```
$var1 : val1
.rule1{
    value: $var1    //val1
}

$var1 : val2
.rule1{
    value: $var1    //val2
}
```

## Interpolation
`#{}`

```
@mixin corner-icon($name, $top-or-bottom, $left-or-right) {
  .icon-#{$name} {
    background-image: url("/icons/#{$name}.svg");
    position: absolute;
    #{$top-or-bottom}: 0;
    #{$left-or-right}: 0;
  }
}

@include corner-icon("mail", top, left);
```

## at-rules

- `@use` : used to load mixins, functions and variables from other sass stylesheets

- NOTE: A stylesheet’s @use rules must come before any rules other than @forward, including style rules. However, you can declare variables before @use rules to use when configuring modules.

```
# load css styles from other file using url
@use '<url>'
```

- css styles are replaced at @use but in case of mixins or functions, namespaces need to be used.(namespace.variable, namespace.function, @include namespace.mixin())
- namespace is the last component of the modules url

```
// src/_corners.scss
$radius: 3px;

@mixin rounded {
  border-radius: $radius;
}


// style.scss
@use "src/corners";

.button {
  @include corners.rounded;
  padding: 5px + corners.$radius;
}
```

- choosing a namespace
```
@use "src/corners" as cor;

# without namespace
@use "src/corners" as *;
```

- Private variables
start the name with `-` or `_`
```
// src/_corners.scss
$-radius: 3px;

@mixin rounded {
  border-radius: $-radius;
}

// style.scss
@use "src/corners";

.button {
  @include corners.rounded;

  // This is an error! $-radius isn't visible outside of `_corners.scss`.
  padding: 5px + corners.$-radius;
}
```

## Maps
```
// defining
$mapvar: (
    <keys>: <values>
)

// accessing
map-get(<mapName>, <key>);
```

```
$font-weights: (
    "regular": 400,
    "medium": 500,
    "bold": 700
);

.comp-class {
    font-weight: map-get($font-weights, bold);
}
```

## Nesting
```
# in normal css:
.container child{...}

.container{
    ...

    .child {
        ...
    }
}

# '&' refers to the parent in which the child is being nested

.container{
    // for .container_paragraph class
    #{&}_paragraph {...}

    &:hover{...}
}
```

## Partials
- css file won't be generated for these
-scss file with a leading underscore

```
// _partial.scss
css code

// main.scss
@import "./partial"
```

## Functions
- should compute and return values
```
@function mul($a, $b) {
    $res: 0;
    @for $i from 1 through $b{
        $res : $res + $a;
    }
    @return $res;
}

.val {
    mul: mul(4, 2);
}
```

## Mixins
- similar to functions but should return style

```
@mixin <mixin-name>{
    ...
}
@include <mixin-name>

# arguments
@mixin <mixin-name>($arg){
    ...
}
@include <mixin-name>($arg1)

# @content
@mixin mobile {
    @media (max-width: 800px){
        @content;
    }
}

@include mobile{
    // gets replaced at @content tag
    flex-direction: column;
}
```

## Conditions
```
@if <condition> {
    ...
}
```

## extend
- Use the existing styles and extend with new ones
```
.parent {...}

.child{
    @extend .parent
    // new styles
}
```
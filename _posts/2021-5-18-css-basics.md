---
layout: post
title: git merge, by example
---

Css basics

## create basic html structure in vscode

Press ! + tab key

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>

</body>
</html>
```

## writting html fast

- div>h1 then press tab
- div then tab and enter
- div * 10 then tab and enter
- h1.heading

## styling methods

- in line Css

```html
<h1 style="background-color: aqua;">Inline style</h1>
```

- in head tag

```html
<style>
  h2 {
    background-color: green;
    color: black;
  }
</style>
```

- external file (best one)

```html
<link rel="stylesheet" href="style.css">
```

## css selectors

- child parent exaple:

```css
  ul li {
    color:blue;
  }
```

## in css more specific wins, and if the same the last wins

id > class > rest

## select by class and id

- by class

```css
.item {
  color: red;
}
```

- by id

```css
#list-item {
  color: red;
}
```

id > class > rest

## css combinators

- descendants (all p tags in section tag)

```css
section p {
  color: red;
}
```

- child (imediate childs of section tag)

```css
section > p {
  color: red;
}
```

- adjacent sibling (the tag p after h1 will be red)

```css
h1 + p {
  color: red;
}
```

- general sibling (all p's after h1 will be red)

```css
h1 ~ p {
  color: red;
}
```

## text properties

```css
p {
  font-size: 16px;
  text-align: left;
  display: inline;
  font-family: Helvetica, Arial, sans-serif;
  font-style: italic;
}

```

## box model

┌─────────────────────────────────┐
│             Margin              │
│  ┌───────────────────────────┐  │
│  │          Border           │  │
│  │   ┌───────────────────┐   │  │
│  │   │     Padding       │   │  │
│  │   │    ┌─────────┐    │   │  │
│  │   │    │Content  │    │   │  │
│  │   │    └─────────┘    │   │  │
│  │   │                   │   │  │
│  │   └───────────────────┘   │  │
│  │                           │  │
│  └───────────────────────────┘  │
│                                 │
└─────────────────────────────────┘

## pseudo classes (define special state of an element)

```css
a:link {
  properties...
}

a:visited {
  color:...
}

a:hover {
  color:...
}

a:active {
  color: ...
}
```

```css
ul {
  font-size: 40px;
}

ul li:first-child {
  color: red;
}

ul li:last-child {
  color: blue;
}

ul li:nth-child(3) {
  color: blue;
}
```

## pseudo elements define style for specific part of an elemnet

```css
h1::first-letter {
  font-size: 70px;
  ...
}

h1::before {
  content: "This is"
  ...
}

h1::after {
  content: "after h122"
  ...
}
```

## Measurement Units

### Absolute

- pixel
- centimeter
- millimeter
- inch
- points
- pica

### Relative

- em
- rem
- vw
- vh
- %
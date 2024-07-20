---
authors:
  - Anton Petrov
status:
  - Draft
tags:
  - svg
  - html
---
> Hey, Chat! Let's talk about svg use tag and related topics?

Sure! The `<use>` tag in SVG (Scalable Vector Graphics) is a powerful feature that allows you to reuse existing SVG elements. This can help to keep your SVG code DRY (Don't Repeat Yourself) and can be very efficient. Here’s an overview of how it works and related topics:

### The `<use>` Tag
The `<use>` element takes nodes from within the SVG document, and duplicates them somewhere else. Here is a basic example:

```xml
<svg width="200" height="200">
  <!-- Define a circle with an id -->
  <circle id="myCircle" cx="50" cy="50" r="40" fill="blue" />
  
  <!-- Use the defined circle -->
  <use href="#myCircle" x="100" y="100" />
</svg>
```

In this example, a circle is defined once and then reused with different coordinates.

### Key Attributes
- `href`: References the element to be reused. This can be a URL or a fragment identifier starting with `#`.
- `x` and `y`: Specify the position where the reused element should be placed.
- `width` and `height`: Override the dimensions of the reused element (optional).

### Benefits of Using `<use>`
1. **Code Reusability**: You can define an element once and reuse it multiple times, which makes your SVG code cleaner and easier to maintain.
2. **Performance**: Reduces the size of your SVG file, as the same element does not need to be defined multiple times.
3. **Consistency**: Ensures uniformity, as reused elements are exact copies of the original.

### Related Topics

#### Symbols and `<symbol>` Tag
The `<symbol>` element is used to define reusable graphical objects inside SVG documents. Unlike other graphical elements, a symbol is not rendered directly. Instead, it must be referenced by a `<use>` element.

```xml
<svg width="200" height="200">
  <!-- Define a symbol -->
  <symbol id="icon" viewBox="0 0 100 100">
    <circle cx="50" cy="50" r="40" fill="red" />
  </symbol>
  
  <!-- Use the symbol -->
  <use href="#icon" x="10" y="10" width="50" height="50" />
  <use href="#icon" x="100" y="100" width="50" height="50" />
</svg>
```

#### External SVG Use
You can reference elements from external SVG files:

```xml
<svg width="200" height="200">
  <use href="external.svg#myCircle" x="50" y="50" />
</svg>
```

#### `<defs>` Element
The `<defs>` element is used to store graphical objects that will be used at a later time. Objects inside `<defs>` are not rendered directly.

```xml
<svg width="200" height="200">
  <defs>
    <circle id="myCircle" cx="50" cy="50" r="40" fill="green" />
  </defs>
  
  <use href="#myCircle" x="0" y="0" />
  <use href="#myCircle" x="100" y="0" />
</svg>
```

### Animations and Interactivity
The `<use>` element can be animated or styled just like any other SVG element. You can apply CSS styles or SMIL animations to reused elements.

```xml
<svg width="200" height="200">
  <defs>
    <circle id="myCircle" cx="50" cy="50" r="40" fill="purple" />
  </defs>
  
  <use href="#myCircle" x="0" y="0" />
  <use href="#myCircle" x="100" y="0">
    <animate attributeName="x" from="100" to="0" dur="2s" repeatCount="indefinite" />
  </use>
</svg>
```

Would you like to dive deeper into any specific aspect of SVG and the `<use>` tag?

> Now, let's imagine: I have my own svg icons somewhere like assets directory, and I want to add them to my project main page and then use them somewhere in the project, how I can achieve that? Is there some known best practices for it?

Certainly! Integrating SVG icons into a project and reusing them efficiently involves a few best practices. Here’s a step-by-step guide to achieve this:

### Step 1: Organize Your SVG Icons
Store your SVG icons in a dedicated directory within your project's assets. For instance:

```
/assets
  /icons
    icon1.svg
    icon2.svg
    icon3.svg
```

### Step 2: Inline SVG vs. External SVG
You have two main options for including SVGs in your HTML: inlining them or linking to them externally.

#### Inline SVG
Inlining SVGs allows you to manipulate and style them directly with CSS and JavaScript. This method is best for when you need to modify the SVGs dynamically.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SVG Icons Example</title>
  <style>
    .icon {
      width: 50px;
      height: 50px;
    }
  </style>
</head>
<body>

  <svg class="icon">
    <use href="assets/icons/icon1.svg#icon1"></use>
  </svg>

  <svg class="icon">
    <use href="assets/icons/icon2.svg#icon2"></use>
  </svg>

</body>
</html>
```

#### External SVG
Linking to external SVGs keeps your HTML cleaner, but you can't manipulate the SVGs as easily with CSS/JS. This method is best for static icons that don’t need to change dynamically.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SVG Icons Example</title>
  <style>
    .icon {
      width: 50px;
      height: 50px;
    }
  </style>
</head>
<body>

  <object type="image/svg+xml" data="assets/icons/icon1.svg" class="icon"></object>
  <object type="image/svg+xml" data="assets/icons/icon2.svg" class="icon"></object>

</body>
</html>
```

### Step 3: Use `<symbol>` and `<use>` for Reusability
To enhance reusability and maintain clean HTML, you can define your SVG symbols in a separate file and reference them throughout your project.

#### Defining SVG Symbols
Create a file, e.g., `icons.svg`, where you define all your SVG symbols.

```xml
<!-- assets/icons.svg -->
<svg xmlns="http://www.w3.org/2000/svg" style="display:none;">
  <symbol id="icon1" viewBox="0 0 64 64">
    <circle cx="32" cy="32" r="32" fill="red"/>
  </symbol>
  <symbol id="icon2" viewBox="0 0 64 64">
    <rect x="0" y="0" width="64" height="64" fill="blue"/>
  </symbol>
</svg>
```

#### Including SVG Symbols in HTML
Include the SVG symbols file at the beginning of your HTML document.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SVG Icons Example</title>
  <style>
    .icon {
      width: 50px;
      height: 50px;
    }
  </style>
</head>
<body>

  <!-- Including the SVG symbols -->
  <div style="display:none;">
    <object type="image/svg+xml" data="assets/icons.svg"></object>
  </div>

  <!-- Using the SVG symbols -->
  <svg class="icon">
    <use href="#icon1"></use>
  </svg>
  
  <svg class="icon">
    <use href="#icon2"></use>
  </svg>

</body>
</html>
```

### Step 4: Best Practices
1. **Optimize SVGs**: Use tools like [SVGO](https://github.com/svg/svgo) to optimize your SVG files and reduce file size.
2. **Use IDs Wisely**: Ensure each SVG symbol has a unique ID to prevent conflicts.
3. **Accessibility**: Add appropriate `aria` attributes to make SVGs accessible.
4. **Caching**: Use caching strategies for your SVG assets to improve load times.
5. **Styling**: Use CSS classes to style your SVGs, keeping your SVG files clean.

By following these steps and best practices, you can effectively integrate and reuse SVG icons in your project, ensuring maintainability and performance.

## \<use\>

Элемент `<use>` считывает содержимое SVG документа и вставляет его в других местах страницы. Этот процесс представляет собой дублирование (deep clone) элемента в буферный (non-exposed) DOM и затем размещение его в местах использования элемента `<use>`,  по аналогии с тем как работает элемент `<template>`

> Подробнее об элементе `<template>` можно узнать здесь: [[Shadow DOM]]. 

## Список источников

- [\<use\> (developer.mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/use)
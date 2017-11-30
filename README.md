<p align="center">
  <a href="http://ivomarsan.com/" target="_blank">
    <img width="128" src="http://ivomarsan.com/favicon.png">
  </a>
</p>

<p align="center">i-dropdown</p>

<p align="center">

  <a href="https://www.npmjs.com/package/i-dropdown">
    <img src="https://img.shields.io/npm/dt/i-dropdown.svg" alt="Downloads">
  </a>

  <a href="https://www.npmjs.com/package/i-dropdown">
    <img src="https://img.shields.io/npm/v/i-dropdown.svg" alt="Version">
  </a>

  <a href="https://www.npmjs.com/package/i-dropdown">
    <img src="https://img.shields.io/npm/l/i-dropdown.svg" alt="License">
  </a>
</p>

---

<a href="https://www.npmjs.com/package/i-dropdown">`i-dropdown`</a> is a
simplistic button forked from
<a href="https://www.npmjs.com/package/vue-material">`vue-material`</a> inspired
in <a href="http://material.google.com" target="_blank">Material Design</a>
specs.

## Installation

Install via `npm`

```bash
npm install i-dropdown --save
```

Import or require in your code:

```javascript
import Vue from 'vue';
import iButton from 'i-dropdown';

// OR

var Vue = require('vue');
var iButton = require('i-dropdown');
```

## Installation

### Module

```javascript
import iButton from 'i-dropdown';

// ...

export default {
  // ...
  components: {
    'my-awesome-button': iButton,
  },
  // ...
};
```

### Browser

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script src="dist/i-dropdown.min.js"></script>
<script>
Vue.use(iButton);

new Vue({
  el: '#app'
});
</script>
```

## Usage

It's very useful to use `i-dropdown` you only need to register then :smile:
seems like with

```html
<i-dropdown class="is-background-red">
  🗑
</i-dropdown>
```

You also can use some properties like

#### A Link _(href & target & rel)_

```html
<i-dropdown class="is-background-red"
:href="'http://ivomarsan.com'"
:target="'_blank'"
:rel="'noopener'">
  🗑
</i-dropdown>
```

#### Button _(disabled & type)_

```html
<i-dropdown class="is-background-red"
:disabled="isDisabled"
:type="'button'">
  🗑
</i-dropdown>
```

#### With a Tooltip

```html
<i-dropdown class="is-outline-blue"
tooltip="This is a Tooltip"
tooltip-pos="top">
  🗑
</i-dropdown>
```

This example will open a `Tooltip` with a message to `top`. See above all
tooltips positions (`toolpos`) avaliable:

* `top`
* `left`
* `right`
* `bottom`

`toolpos` its a _String_ and has by default value: `top`

## Personalization

You can personalize your `<i-dropdown>` with class `i-dropdown` or pre-defined
classes to change background, color or outline (_list bellow_) or through some
properties.

`uppercase` (_Boolean_) if true, text will be UPPERCASED.

```html
<i-dropdown uppercase>🗑</i-dropdown>
```

`is-color` (_String: Hex_) to define a color to button.

```html
<i-dropdown is-color="#d43f3a">🗑</i-dropdown>
```

`is-outline` (_String: Hex_) to draw a custom border.

```html
<i-dropdown is-outline="#d43f3a">🗑</i-dropdown>
```

`is-background` (_String: Hex_) to get a background color.

```html
<i-dropdown is-background="#d43f3a">🗑</i-dropdown>
```

### Colors

Actually we have 10 colors, are:

* <img src="https://img.shields.io/badge/red-                    -d43f3a.svg?style=for-the-badge" alt="red">
* <img src="https://img.shields.io/badge/pink-                    -ff067c.svg?style=for-the-badge" alt="pink">
* <img src="https://img.shields.io/badge/blue-                    -0488bb.svg?style=for-the-badge" alt="blue">
* <img src="https://img.shields.io/badge/gray-                    -ada8a5.svg?style=for-the-badge" alt="gray">
* <img src="https://img.shields.io/badge/black-                    -000000.svg?style=for-the-badge" alt="black">
* <img src="https://img.shields.io/badge/white-                    -ffffff.svg?style=for-the-badge" alt="white">
* <img src="https://img.shields.io/badge/green-                    -4cae4c.svg?style=for-the-badge" alt="green">
* <img src="https://img.shields.io/badge/purple-                    -9400c8.svg?style=for-the-badge" alt="purple">
* <img src="https://img.shields.io/badge/yellow-                    -ffdf00.svg?style=for-the-badge" alt="yellow">
* <img src="https://img.shields.io/badge/orange-                    -ff9e00.svg?style=for-the-badge" alt="orange">

### Classes

Classes are formed by `prefix-type-color`, examples:

`.is-color-green`

```html
<i-dropdown class="is-color-green">🗑</i-dropdown>
```

`.is-outline-green`

```html
<i-dropdown class="is-outline-green">🗑</i-dropdown>
```

`.is-background-green`

```html
<i-dropdown class="is-background-green">🗑</i-dropdown>
```

> See more in [i-colors](https://www.npmjs.com/package/i-colors) and examples.
> It's easier :smile:

## Demo

[JSFiddle](https://jsfiddle.net/r5yf1qtv/)

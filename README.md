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
simplistic dropdown forked from
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
import iDropdown from 'i-dropdown';

// OR

var Vue = require('vue');
var iDropdown = require('i-dropdown');
```

### Module

```javascript
import iDropdown from 'i-dropdown';

// ...

export default {
  // ...
  components: {
    'my-awesome-dropdown': iDropdown,
  },
  // ...
};
```

### Browser

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script src="dist/i-dropdown.min.js"></script>
<script>
Vue.use(iDropdown);

new Vue({
  el: '#app'
});
</script>
```

## Usage

It's very useful to use `i-dropdown` you only need to register then :smile:
seems like with

```html
<i-dropdown v-model="dropdown" :options="['A', 'B', 'C']">

</i-dropdown>
```

It's easier to use, you only need to pass an `array []` with `Number` or
`String` type. When you pass an `Object` you need some attention to default key
`label`. Some like this:

```html
<i-dropdown v-model="dropdown" :options="options">

</i-dropdown>
```

```javascript
import iDropdown from 'i-dropdown';

export default {
  components: {
    iDropdown,
  },
  data: () => ({
    dropdown: '',
    options: [
      { label: 'Option 1', hide: `you'll don't see this text` },
      { label: 'Option 2', hide: `you'll don't see this text` },
      { label: 'Option 3', hide: `you'll don't see this text` },
      { label: 'Option 4', hide: `you'll don't see this text` },
    ],
  }),
};
```

But you can personalize this with some properties like

#### Options

> @type {Array}

Options to show on dropdown. You probably need to use a `v-model` to receive
this data information.

```html
<i-dropdown v-model="myModel" :options="['A', 'B', 'C']">
</i-dropdown>
```

#### Label

> @type {String}

Imagine now, you have `myOptions` that is an Array but you haven't a key
property like `label`. Are you thinking to use an `array.map()`? No way! You
should to use `label property` passing the key name that you want use instead.

```html
<i-dropdown v-model="myModel" :options="myOptions" label="name">
</i-dropdown>
```

#### Initial value

> @type {String}

Will be impressed instead _i-dropdown_

```html
<i-dropdown initial="Hi I'm Goku">
</i-dropdown>
```

#### Placeholder

> @type {String}

Add a placeholder on filter when avaliable

```html
<i-dropdown placeholder="Search here">
</i-dropdown>
```

#### No Matching

> @type {String}

Alternative message to show when has no matching on filter

```html
<i-dropdown no-matching="Sorry :/">
</i-dropdown>
```

#### Limit

> @type {String}

Limit how much results will be avaliable in Dropdown. Default are 5 results

```html
<i-dropdown limit="8">
</i-dropdown>
```

#### Filter

> @type {Boolean}

When True a input search will be avaliable

```html
<i-dropdown filter>
</i-dropdown>
```

#### Disabled

> @type {Boolean}

Disable the entire component

```html
<i-dropdown disabled>
</i-dropdown>
```

#### Hide Results

> @type {Boolean}

Hide results from Dropdown when filter is called

```html
<i-dropdown hide-results>
</i-dropdown>
```

#### Inline

> @type {Boolean}

Display element inline. Its mean, no borders

```html
<i-dropdown inline>
</i-dropdown>
```

#### Max Height

> @type {String}

Sets the max-height property on the dropdown list. Default is 200px

```html
<i-dropdown max-height="500px">
</i-dropdown>
```

## Personalization

You can personalize your `<i-dropdown>` with class `i-dropdown` or through some
properties.

#### Uppercase

> @type {Boolean}

Convert All Exibed Text to UPPERCASE

```html
<i-dropdown uppercase>
</i-dropdown>
```

#### isBackground

> @type {String}

Paint Background Color

```html
<i-dropdown is-background="#ff0"></i-dropdown>
```

#### isOutline

> @type {String}

Paint Border Color

```html
<i-dropdown is-outline="#f0f"></i-dropdown>
```

#### isColor

> @type {String}

Paint Text Color

```html
<i-dropdown is-color="#00f"></i-dropdown>
```

#### isTooltip

> @type {String}

Give us a Tooltip

```html
<i-dropdown is-tooltip="This is a Tooltip"></i-dropdown>
```

This example will open a `Tooltip` with a message to `top`. See above all
tooltips positions (`is-position`) avaliable:

* `top`
* `left`
* `right`
* `bottom`

```html
<i-dropdown is-tooltip="This is a Tooltip" is-position="right"></i-dropdown>
```

## Demo

[JSFiddle](https://jsfiddle.net/x9f7j6zt/)

# Tailwind CSS

## ToC

- [Installation](#installation)
- [Selected documentation on classes](#selected-documentation-on-classes)
- [Variants and modifiers](#variants-and-modifiers)
- [Plugins](#plugins)
- [Use Cases](#use-cases)
- [UI Libraries](#ui-libraries)

## Installation

```
yarn add -D tailwindcss
```

```
npx tailwindcss init
```

### Example tailwind.config.js

```
module.exports = {
  content: ['*.{html,js}'],
   theme: {
     extend: {
       colors: {
        dark: '#2c2c3f',
        gold: '#ceb193',
      },
    },
  },
  plugins: [
    require("@tailwindcss/forms"),
    require("@tailwindcss/typography"),
  ],
}
```

To **avoid conflicting with default styling** add this to tailwind.config.js:

```
module.exports = {
  // disable normalize css
  corePlugins: {
    preflight: false,
  },
  // add prefix to all classes
  prefix: 'tw-',
}
```

> Modifiers such as `sm:` `lg:` `focus:` `group-hover:` `odd:` etc. come **before** the prefix.
>
> Example: `tw-px-4 sm:tw-px-6 lg:tw-px-8`

### Using CLI to compile

Compile with command:

`npx tailwindcss -i ./src/input.css -o ./dist/output.css --watch`

Edit the input and output **file path**.

> The convention is to add this script to package.json under `watch:style` key.

> When using CLI, you can [import other .css files right into the base file](https://tailwindcss.com/blog/tailwindcss-v3-1#built-in-support-for-css-imports-in-the-cli)
> Example:  `@import "./swiper.bundle.css"`

### Using Webpack to compile

> Compiling straight into JS may not be necessary. You can compile CSS alongside the Webpack job with using the CLI method above.
>
> This will probably be the preferred option due to smaller bundle sizes.

Apply installation step above. Install PostCSS and style loaders.

```
yarn add -D postcss postcss-loader postcss-preset-env style-loader css-loader
```

Add style loader rule to webpack.module.rules

```
{
  test: /\.css$/i,
  exclude: /node_modules|vendor/,
  use: ['style-loader', 'css-loader', 'postcss-loader'],
},
```

Create postcss.config.js file containing:

```
const tailwindcss = require('tailwindcss');
module.exports = {
  plugins: [
    'postcss-preset-env',
    tailwindcss
  ],
};
```

Create tailwind.css file containing:

```
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Import it into your root JS file:

```
import './tailwind.css'
```

## Selected documentation on classes

### Transition

You can use `transition` class by default:

- It has a default setting which for transition properties (does not apply `transition: all` which is a bad practice).
- It has default timing function corresponding to `ease-in-out`
- It has default duration corresponding to `duration-150`

If needed, you can override this behaviour with [timing function classes](https://tailwindcss.com/docs/transition-timing-function) and [duration classes](https://tailwindcss.com/docs/transition-duration).
### [Accent Color](https://tailwindcss.com/docs/accent-color)

Allows to globally change the color of certain UI elements, such as **form inputs**, **checkboxes** and **radios**.

## Variants and modifiers

Tailwind supports a large number of variant/state selectors:

- [Aria states](https://tailwindcss.com/docs/hover-focus-and-other-states#aria-states)
- `hover`, `focus`, `active`
- `last`, `first`, `odd`, `even`
- Form states - `required`, `invalid`, `disabled`
- Dark mode variant `dark:`
- Contained elements `has-[]` e.g. `has-[:checked]` and `group-has-[:checked]` (see [video link](https://www.youtube.com/watch?v=5hF0IVQIBN8&t=147s) for possible use case)

## Plugins

### Container queries

[Plugin Docs](https://github.com/tailwindlabs/tailwindcss-container-queries)

Enables `@container` class which works as a "viewport" for child elements. You can reference this with `@sm` `@md` `@lg` etc.

## Use cases (components)

Selected components with plausible use case:
### Masonry layout

Achievable using [CSS columns](https://tailwindcss.com/docs/columns#basic-usage) ([Tailwind Play](https://play.tailwindcss.com/hrfnQ69FBs))
In future Tailwind CSS you will be able to do: `grid-rows-[masonry]`


## UI libraries

### Tailwind UI

### Flowbite

- [Alert](https://flowbite.com/docs/components/alerts/)
- [Blockquote with Icon](https://flowbite.com/docs/typography/blockquote/#blockquote-icon)
- [Breadcrumb](https://flowbite.com/docs/components/breadcrumb/#default-breadcrumb)
- [Floating Label](https://flowbite.com/docs/forms/floating-label/)
- [Input Validation Styles](https://flowbite.com/docs/forms/input-field/#validation)
- [Pagination](https://flowbite.com/docs/components/pagination/#default-pagination)
- [Spinner](https://flowbite.com/docs/components/stepper/#default-stepper)
- [Spinner on Button](https://flowbite.com/docs/components/spinner/#buttons)
- [Sticky Banner](https://flowbite.com/docs/components/banner/#default-sticky-banner)
- [Step Navigation](https://flowbite.com/docs/components/stepper/#default-stepper)
- [Tabs](https://flowbite.com/docs/components/tabs/#default-tabs)
- [Toggle](https://flowbite.com/docs/forms/toggle/#toggle-example)
- [User Notification](https://flowbite.com/docs/components/toast/#colors)
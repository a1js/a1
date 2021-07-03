# A1

A1 is a JavaScript framework for building UI.

Proof of concept (PoC)

## Spec

### Closure Component

```js
function Counter({ count = 0 }) {
  const { init, model } = snapshot({ count })

  function view(dispatch) {
    return $ => {
      $.div($ => {
        $.button()
          .onClick(() => dispatch('Decrement'))
          .text`-`

        $.div().text`${model.count}`

        $.button()
          .onClick(() => dispatch('Increment'))
          .text`+`
      })
    }
  }

  function update(message) {
    switch (message) {
      case 'Decrement':
        return {
          count: model.count - 1
        }
      case 'Increment':
        return {
          count: model.count + 1
        }
    }
  }

  return {
    init,
    view,
    update
  }
}
```

### Pure Component

```js
function Greeting({ name }) {
  return $ => $.span(`Hello, ${name}!`)
}
```

### Conditional Rendering

```js
$.div($ => {
  if (isLoggedIn) {
    $.button('Log Out')
  } else {
    $.button('Log In')
  }
})
```

### Loops

```js
$.ul($ => {
  for (const item of items) {
    $.li(`${item}`)
  }
})
```

### Usage

```js
import A1, { snapshot } from '@a1js/a1'

function Counter({ count = 0 }) {
  const { init, model } = snapshot({ count })
  ...
}

const a1 = A1()

a1.registerComponent('Counter', Counter)

a1.render($ => $.Counter({ count: 42 }), document.getElementById('root'))
```

### Plugins

```js
a1.registerPlugin('animate', (opts) => { ... })

$.button({ id: 'submit', class: 'btn btn-primary' })
  // .id('submit')
  // .class('btn btn-primary')
  .animate({ ... })
```

### Routes

```js
// $.Link({ to: path, action: 'replace' })
// $a({ href: path, 'data-a1-action': 'replace' })

a1.route('/', (req) => { ... })

a1.route('/docs', (req) => { ... })

a1.route('/about', (req) => { ... })
```

### Middlewares

```js
a1.middleware('button', filter, debounce(opts))
```

### Element Function Overloads

```js
// type signature
// (propsOrChildren?: VProps | Element, children?: Element) => Plugin

$.ElementName()

// with props
$.ElementName({ key: value })

// with text node children
$.ElementName('text')
$.ElementName(['hello', 'world'])

// with children
$.ElementName($ => { ... })

// with props and children
$.ElementName({ key: value }, $ => { ... })
```

## Half-baked idea

```js
// 1. babel-plugin

// 1.1 before
$.div {
  $.div {
  }
}

// 1.2 after
$.div($ => $.div())

// 2. html fragments
$ => fragment($)`<div>html fragments</div>`

// 3. helper

// 3.1 webpack
a1.webpackHelper() // require.context('./components', true, /\.js$/)

// 3.2 vite
a1.viteHelper() // import.meta.glob('./components/*.js')
```

## Acknowledgments

A1 is heavily inspired by [Elm](https://elm-lang.org/), [React](https://reactjs.org/) and [@Hotwire/Turbo](https://turbo.hotwire.dev/).

## License

[MIT](https://opensource.org/licenses/MIT)

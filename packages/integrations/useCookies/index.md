---
category: '@Integrations'
---

# useCookies

wrapper for [`universal-cookie`](https://www.npmjs.com/package/universal-cookie).

## Install 

```bash
npm i universal-cookie
```

## Usage

### Common usage

```html
<template>
  <div>
    <strong>locale</strong>: {{ cookies.get('locale') }}
    <hr>
    <pre>{{ cookies.getAll() }}</pre>
    <button @click="cookies.set('locale', 'ru-RU')">Russian</button>
    <button @click="cookies.set('locale', 'en-US')">English</button>
  </div>
</template>

<script>
  import { defineComponent } from 'vue'
  import { useCookies } from '@vueuse/integrations/useCookies'

  export default defineComponent({
    setup() {
      const cookies = useCookies(['locale'])
      return {
        cookies,
      }
    },
  })
</script>
```

### SSR support ([Nuxt](https://github.com/nuxt/nuxt.js) plugin example)

```ts
// universal-cookies.js
import { createCookies } from '@vueuse/integrations/useCookies'

let useUniversalCookies

const cookiesPlugin = ({ req }) => {
  useUniversalCookies = createCookies(req)
}

export default cookiesPlugin

export { useUniversalCookies } // now import and call useUniversalCookies instead of useCookies
```

## Options

Access and modify cookies using vue composition-api.

> By default, you should use it inside `setup()`, but this function also works anywhere else.

```ts
const { get, getAll, set, remove, addChangeListener, removeChangeListener } = useCookies(['cookie-name'], { doNotParse: false, autoUpdateDependencies: false })
```

### `dependencies` (optional)

Let you optionally specify a list of cookie names your component depend on or that should trigger a re-render. If unspecified, it will render on every cookie change.

### `options` (optional)

- `doNotParse` (boolean = false): do not convert the cookie into an object no matter what. **Passed as default value to `get`/`getAll` methods.**
- `autoUpdateDependencies` (boolean = false): automatically add cookie names ever provided to `get` method. If **true** then you don't need to care about provided `dependencies`.

### `cookies` (optional)

Let you provide a `universal-cookie` instance (creates a new instance by default)

> Info about methods available in the [universal-cookie api docs](https://www.npmjs.com/package/universal-cookie#api---cookies-class)

## `createCookies([req])`

Create a `universal-cookie` instance using request (default is window.document.cookie) and returns `useCookies` function with provided universal-cookie instance

- req (object): Node's [http.IncomingMessage](https://nodejs.org/api/http.html#http_class_http_incomingmessage) request object

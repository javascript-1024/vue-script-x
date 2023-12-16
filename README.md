# VueScriptX - Asynchrounous Script Loading for Vue

[![Licence](https://img.shields.io/github/license/Ileriayo/markdown-badges?style=for-the-badge)](./LICENSE)
<img alt="NodeJS" src="https://img.shields.io/badge/node.js-%2343853D.svg?&style=for-the-badge&logo=node.js&logoColor=white"/>
<img alt="Vue.js" src="https://img.shields.io/badge/vuejs-%2335495e.svg?&style=for-the-badge&logo=vue.js&logoColor=%234FC08D"/>
<img alt="TypeScript" src="https://img.shields.io/badge/typescript-%23007ACC.svg?&style=for-the-badge&logo=typescript&logoColor=white"/>

VueScriptX brings back the `<script>` tag to your Vue projects!

This package is inspired by [vue-script2](https://github.com/taoeffect/vue-script2).

This tiny package should take care of all your declarative and imperative asynchronous loading needs. Nothing too complicated from what web devs already know.

This version is tailored for the [Vue.js](https://v3.vuejs.org) framework, but it's easy to port to others.

If you like this package, a **star** to the repo is nice.

## Features

- Support for **Vue 3** and **Typescript**.
- Brings back `<script>` tag to your Vue projects as `<scriptx>`.
- Global Vue property `$scriptx` for regular code access.
- Choose asynchronous and ordered execution using `async` prop.
- Prop `unload` handles callbacks after script being unmounted.

## Requirements

- Vue.js >= 3.0.0
- Ecma version >= ES6

## Installation

```
npm install vue-scriptx --save
```

Then enable the plugin (in your `main.ts` or wherever):

```ts
import App from '../App.vue'
import ScriptX from 'vue-scriptx'

createApp(App)
.use(ScriptX)

```

## Usage

As simple as using `<scriptx>` as `<script>` tags or using `this.$scriptx`, below samples illustrates how to load jQuery asynchronously.

**DO NOT put user provided code inside `<scriptx>` or its propeties.**

### In Templates

By default, `<scriptx>` tags are serialized using promises so that one script loads after another has finished. If you don't need ordered execution, add `async` to have the script injected immediately.

The `scriptx` component takes text as slot, you can still use delimiters to change the text being sent as slot.

```html
<scriptx src="/path/to/jquery.min.js"></scriptx>
<scriptx>
  $('#msg').text('Hello from VueScriptX!')
</scriptx>
```

#### Prop `async`

Specify if this script should load itself asynchronously (ignoring execution order of other scripts). This defaults to `"false"`.

You can also mix them so that some `<scriptx>` tags are loaded asynchronously while others wait for the ones before them.

```html
<scriptx src="jquery.min.js"></scriptx>
<scriptx>$('#foo').text('hi!')</scriptx>
<!-- Load next script immediately, don't wait for jQuery -->
<scriptx src="lib.js" async></scriptx>
```

#### Prop `unload`

Callback being executed after the scriptx is being unmounted. Note that it takes a string containing the code.

```html
<scriptx src="/path/to/jquery.min.js" unload="jQuery.noConflict(true)"></scriptx>
```

### In Vue Scripts

It may not work in typescript as it may complain that `$` is undefined.

```js
this.$scriptx.load('/path/to/jquery.min.js')
.then(() => $('#msg').text('Hello from VueScriptX!'))
```

## Contributions

Contributions are welcomed, as long as it doesn't cause any harm to any users.

Please do not submit compiled files (e.g. the `dist` directory of this repo).

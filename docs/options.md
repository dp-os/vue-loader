---
sidebar: auto
---

# Options Reference

## transformAssetUrls

- type: `{ [tag: string]: string | Array<string> }`
- default:

  ``` js
  {
    video: ['src', 'poster'],
    source: 'src',
    img: 'src',
    image: ['xlink:href', 'href'],
    use: ['xlink:href', 'href']
  }
  ```

  During template compilation, the compiler can transform certain attributes, such as `src` URLs, into `require` calls, so that the target asset can be handled by webpack. For example, `<img src="./foo.png">` will attempt to locate the file `./foo.png` on your file system and include it as a dependency of your bundle.

## compiler

- Type: `VueTemplateCompiler`
- default: `require('vue-template-compiler')`

  Override the default compiler used to compile `<template>` blocks in single file components.

## compilerOptions

- type: `Object`
- default: `{}`

  Options for the template compiler. When using the default `vue-template-compiler`, you can use this option to add custom compiler directives, modules, or discard whitespaces between template tags with `{ preserveWhitespace: false }`.

  See [`vue-template-compiler` options reference](https://github.com/vuejs/vue/tree/dev/packages/vue-template-compiler#options).

## transpileOptions

- type: `Object`
- default: `{}`

  Configure ES2015+ to ES5 transpiling options for the generated render function code. The [transpiler](https://github.com/vuejs/vue-template-es2015-compiler) is a fork of [Buble](https://github.com/Rich-Harris/buble), so consult the available options [here](https://buble.surge.sh/guide/#using-the-javascript-api).

  The template render functions compilation supports a special transform `stripWith` (enabled by default), which removes the `with` usage in generated render functions to make them strict-mode compliant.

## optimizeSSR

- type: `boolean`
- default: `true` when the webpack config has `target: 'node'` and `vue-template-compiler` is at version 2.4.0 or above.

  Enable Vue 2.4 SSR compilation optimization that compiles part of the vdom trees returned by render functions into plain strings, which improves SSR performance. In some cases you might want to explicitly turn it off because the resulting render functions can only be used for SSR and cannot be used for client-side rendering or testing.

## hotReload

- type: `boolean`
- default: `true` in development mode, `false` in production mode or when the webpack config has `target: 'node'`.
- allowed value: `false` (`true` will not force Hot Reload neither in production mode nor when `target: 'node'`)

  Whether to use webpack [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) to apply changes in the browser **without reloading the page**.
  Use this option (value `false`) to disable the Hot Reload feature in development mode.

## productionMode

- type: `boolean`
- default: `process.env.NODE_ENV === 'production'`

Force production mode, which prohibits the loader from emitting code (e.g. hot-reload related code) that is development-only.

## shadowMode

- type: `boolean`
- default: `false`

Compiled the component for usage inside Shadow DOM. In this mode, the styles of the component will be injected into `this.$root.$options.shadowRoot` instead of the document head.

## cacheDirectory / cacheIdentifier

- type: `string`
- default: `undefined`

When both options are specified, enables file-system-based template compilation caching (requires `cache-loader` to be installed in the same project).

::: tip
  Interaction between `vue-loader` and `cache-loader` uses [inline loader import syntax](https://webpack.js.org/concepts/loaders/#inline) under the hook, the `!` will be treated as the separator between different loaders, so please ensure `cacheDirectory` doesn't contain `!`.
:::

## prettify

- type: `boolean`
- default: `true` if prettier v1 or v2 is installed, `false` otherwise

In development mode, we use [prettier](https://prettier.io/) to format the compiled render function for ease of debugging by default. However, if you encounter any obscure bug of prettier, such as [exponential compilation time for deeply nested functions](https://github.com/prettier/prettier/issues/4672), you can disable this option to circumvent it.

## exposeFilename

- type: `boolean`
- default: `false`

In non-production environments, vue-loader injects a `__file` property to components for better debugging experience. If the `name` property is missing in a component, Vue will infer it from the `__file` field to display in console warnings.

This property is stripped in production builds by default. But you may want to retain it if you are developing a component library and don't want to bother specifying `name` in each component. Then you can turn this option on.

# babel-plugin-add-import-extension ![Lint and test](https://ci.prieb.me/api/badges/karl/babel-plugin-add-import-extension/status.svg?branch=main)

Back to Github :)
This project started on Github and I moved that to SourceHut, but I found that this project had so much more interactions and was so much more reachable for other devs on Github that I decide to move that back. Sadly I deleted the old repo and we lost some issues.

A plugin to add extensions to import and export declarations, is very useful when you use Typescript with Babel and don't want to explicity import or export module with extensions.

## How to install:

```sh
# using npm
npm install --save-dev babel-plugin-add-import-extension
# usin yarn
yarn add -D babel-plugin-add-import-extension
```

Add to your `plugins` on your babel config file:

```js
plugins: ["babel-plugin-add-import-extension"]; // defaults to .js extension
```

Is possible to set the extension when you set the plugin:

```js
plugins: [
  ["babel-plugin-add-import-extension", { extension: "jsx" }], // will add jsx extension
];
```

You can also replace existing extensions with the one you want

```js
plugins: [
  ["babel-plugin-add-import-extension", { extension: "jsx", replace: true }], // will replace the "observedScriptExtensions" [see below] to jsx
];
```

To be able to handle file with a *.* in the filename (e.g *component.style.ts*) the plugin is configured
to only handle a certain set of file extensions. If needed you can adjust the default of `['js','ts','jsx','tsx']` 
by changing the `observedScriptExtensions` option

```js
plugins: [
  ["babel-plugin-add-import-extension", { extension: "jsx", replace: true, observedScriptExtensions: ['js','ts','jsx','tsx', 'mjs', 'cjs'] }], // will add jsx extension
];
```

To skip the files that already have an extension that shouldn't be replaced or appended,
you can change the `omittedScriptExtensions` option. The default list of omitted extensions
is `['json']`

```js
plugins: [
  ["babel-plugin-add-import-extension", { omittedScriptExtensions: ['json', 'svg'] }], // will not add extension to *.json or *.svg imports
];
```

## Let's the transformation begin :)

A module import without extension:

```js
import { add, double } from "./lib/numbers";
```

will be converted to:

```js
import { add, double } from "./lib/numbers.js";
```

A module export without extension:

```js
export { add, double } from "./lib/numbers";
```

will be converted to:

```js
export { add, double } from "./lib/numbers.js";
```

If you add the `replace:true` option, extensions will be overwritten like so

```js
import { add, double } from "./lib/numbers.ts";
```

will be converted to:

```js
import { add, double } from "./lib/numbers.js";
```

and

```js
export { add, double } from "./lib/numbers.ts";
```

will be converted to:

```js
export { add, double } from "./lib/numbers.js";
```

What this plugin does is to check all imported modules, and if the module is 
local, or it is not a "root import", it will add the extension to the import.

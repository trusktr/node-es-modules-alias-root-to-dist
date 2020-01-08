Show that an `"exports"` alias in `package.json` can be used to point the
import root of a Node.js package to a subfolder (f.e. `dist/`).

Run the example with Node.js v13.2 or higher:

```
node .
```

The output is:

```
$ node .
(node:73031) ExperimentalWarning: The ESM module loader is experimental.
Bar
Lorem
Foo
```

See `node_modules/test-package`.

### More detailed explantation

Some projects compile TypeScript, CoffeeScript, or other source code in a
`src/` folder into final output in a `dist/` folder, but they may not want
consumers of their package to have to include the `dist/` folder in any import
statements.

A package author can achieve this with an `exports` configuration in their
`package.json` like so:

```json
{
	"name": "some-package",
	"type": "module",
	"exports": {
		".": "./dist/index.js",
		"./": "./dist/"
	}
}
```

which allows end users to import things relative to the package root instead of
the dist folder:

```js
import {...} from 'some-package'
// loads some-package/dist/index.js

import {...} from 'some-package/subfolder/foo.js'
// loads some-package/dist/subfolder/foo.js
```

Related issue: https://github.com/nodejs/node/issues/14970


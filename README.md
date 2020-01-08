Show that "exports" aliases in package.json cause ES Modules not to be able to import subfolders.

> Note, if you're not using the latest version of Node.js, you may need to add the `--experimental-modules` flag.

The following works, the file imports relative to `test-package/dist/` which is specified by value `"./": "./dist/"` of the `"exports"` field in `test-package/package.json`:

```
node testFooImport.js
```

The following throws an error wen the file tries to import from `test-package/subfolder/`, the because the value of `"exports"` causes the `subfolder`  invisible to the consumer of test-package:

```
node testBarImport.js
```

Related issue: https://github.com/nodejs/node/issues/14970

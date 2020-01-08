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

The output is similar to this:


```
$ node --experimental-modules testFooImport.js 
Foo

$ node --experimental-modules testBarImport.js 
(node:16752) ExperimentalWarning: The ESM module loader is experimental.
internal/modules/esm/default_resolve.js:82
  let url = moduleWrapResolve(specifier, parentURL);
            ^

Error: Cannot find module /home/trusktr/test/node_modules/test-package/dist/subfolder/subfolder/Bar.js imported from /home/trusktr/test/testBarImport.js
    at Loader.resolve [as _resolve] (internal/modules/esm/default_resolve.js:82:13)
    at Loader.resolve (internal/modules/esm/loader.js:73:33)
    at Loader.getModuleJob (internal/modules/esm/loader.js:147:40)
    at ModuleWrap.<anonymous> (internal/modules/esm/module_job.js:41:40)
    at link (internal/modules/esm/module_job.js:40:36) {
  code: 'ERR_MODULE_NOT_FOUND'
}
```

Related issue: https://github.com/nodejs/node/issues/14970


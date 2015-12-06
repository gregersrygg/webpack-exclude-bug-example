A minimal example project to reproduce a weird exclude behavior
https://github.com/webpack/webpack/issues/1712

1. Clone this repo into a folder named tmp (or /tmp)
2. Run `npm install && npm test`
3. Output is "checksum-loader did not run", but expected value is: "checksum-loader was applied successfully"
4. Check dist/bundle.js to see that the checksum has not been applied.

5. Clone this repo into another folder that does not have a parent folder containing "tmp"
6. Run `npm install && npm test`
7. Output is "checksum-loader was applied successfully"
8. Check dist/bundle.js to see that the checksum has been applied.

The reason the first fails and the second works is because the `exclude` rule in webpack.config.js, and it is checked against the full path instead of a project-relative path (that I expected).

```js
preLoaders: [
    {
        test: /\.js$/,
        loader: "checksum-loader",
        exclude: /tmp/
    }
]
```

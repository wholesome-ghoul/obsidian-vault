## Why Bundling

- Reducing HTTP requests. node_modules has tons of dependencies and those dependencies have their own dependencies and so on. Making separate HTTP request for each of them would take a lot of time. In order to solve this, we will use bundling. Bundlers are used to convert our application source code into a smaller number of self-contained "bundles" that can be loaded with a single request.
- Code transforms. Modern apps are commonly built with Typescript, JSX, SCSS, and so on. Browsers support plain Javascript and CSS, so we need to somehow transpile modern solutions into plain solutions.
- Framework features. Frameworks rely on bundler plugins.

## Steps

```bash
bun add -d typescript \
  webpack@^5 webpack-cli@^4 webpack-dev-server@^4 \
  ts-loader html-webpack-plugin \
  @pmmmwh/react-refresh-webpack-plugin react-refresh
```

## Info

```json
{
  "name": "@bunch/button",
  "version": "0.0.1",
  "description": "Bunch React Button component",
  "author": "Wholesome Ghoul <wholesome.ghoul@gmail.com>",
  "publishConfig": {
    "access": "public"
  },
  "main": "", // entry point for user
  "files": [], // entries to be included when package is installed as dependency
  "scripts": {
    "build": ""
  },
  "peerDependencies": {},
  "dependencies": {},
  "devDependencies": {}
}
```

## nx

```bash
nx run-many -t test lint e2e
nx run-many -t build --skip-nx-cache

nx list @nx/react
nx list @nx/react

nx g @nx/react:component hello --dry-run

nx graph
nx graph --affected

nx affected -t build
```

```json
// nx.json
{
  "targetDefaults": {
    "build": {
      "dependsOn": [
        "^build" // run dependent projects' `build` first
      ]
    }
  }
}
```

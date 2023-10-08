# Why Bundling

- Reducing HTTP requests. node_modules has tons of dependencies and those dependencies have their own dependencies and so on. Making separate HTTP request for each of them would take a lot of time. In order to solve this, we will use bundling. Bundlers are used to convert our application source code into a smaller number of self-contained "bundles" that can be loaded with a single request.
- Code transforms. Modern apps are commonly built with Typescript, JSX, SCSS, and so on. Browsers support plain Javascript and CSS, so we need to somehow transpile modern solutions into plain solutions.
- Framework features. Frameworks rely on bundler plugins.

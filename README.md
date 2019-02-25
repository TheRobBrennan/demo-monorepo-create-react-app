# Overview
This project is intended to serve as an example working with a monorepo that contains multiple create react apps - originally based on the guide available at [https://itnext.io/guide-react-app-monorepo-with-lerna-d932afb2e875](https://itnext.io/guide-react-app-monorepo-with-lerna-d932afb2e875).

## Cheat sheet
```sh
$ lerna init

// Install common dependencies to have Storybook
//  + Run at the root of your project
//  + Have Storybook auto-install the right modules for your React project
//  + Have Babel transpile correctly for code, testing, and Storybook

$ npm i -D react react-dom @babel/core@^7.0.0-0 @babel/cli babel-plugin-transform-es2015-modules-commonjs babel-jest enzyme enzyme-adapter-react-16 jest react-test-renderer babel-core@7.0.0-bridge.0 @babel/preset-env @babel/preset-react

// Install and initialize Storybook version 4
// Note: Installing the @alpha version (currently @4.0.0-rc.6), will allow us to set our Babel configuration inside of
// our package.json files which will make configuration easier for sub-packages.

$ npx -p @storybook/cli@alpha sb init

// Create your React app under ./packages/my-react-app
$ cd packages
$ npx create-react-app my-react-app

// If you run "npm run start" you should see an error containing a message like:
//    If nothing else helps, add SKIP_PREFLIGHT_CHECK=true to an .env file in your project.
// Do that. Create a .env file and add that environment variable. "npm run start" should work as expected.

// Create an external component to be shared across the app
$ cd ../packages
$ touch package.json

// Use package.json from the original guide as an example noting the following:
//  + name - The organizational namespance for your component when installing via NPM or cross-linked Lerna
//  + main - The compiled code that will be shipped with the build of your React app
//  + module - The pre-compiled code that will be imported as a local run-time dependency while developing the app or running tests
//  + transpile - An NPM script to start the transpile of your code with Babel
//    - IMPORTANT: Note that we are using "transpile" and not "build" here; we will need a build script later to build apps using "lerna run build"
//  + babel - This setup configures our component to transpile with Babel 7 for React
//    - IMPORTANT: Note that because we initially installed components like `react`, `react-dom`, `@babel/core@^7.0.0-0` etc we do not need to install them here

// Create a source directory and our component
$ mkdir src
$ cd src
$ touch index.js

// Transpile your component (within the packages/comp-button/src directory)
$ lerna run transpile

// You should see the transpiled file in packages/comp-button/dist

// Add a test for the component (within the packages/comp-button/src directory)
$ touch index.spec.js

// See the example from the original guide

// Add the jest section to your component's package.json
//  When jest runs, it will run the setupTests.js file in the monorepo root, so let's create that file now
$ cd <my-project>
$ touch setupTests.js

// Use the example from the original guide (note how we our using older require syntax so we do not need additionab babel configuration here)

// Run jest using lerna (within the packages/comp-button/src directory)
//  Note that we are using "jest" and not "test" - we want to reserve the word test for running all tests (end-to-end, linting, etc)
$ lerna run jest

// Add a story for this new globally available React component (within the packages/comp-button/src directory)
$ touch index.stories.js

// Modify your storybook configuration in .storybook/config.js to load them from all `packages/**` directories instead of the `stories` directory

// Run Storybook and verify that your newly added story is loaded
$ npm run storybook

```
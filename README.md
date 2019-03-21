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

// Add a reference to the shared component in `packages/my-react-app/package.json`

// From the root of the directory, crosslink our packages with `lerna bootstrap`
$ lerna bootstrap

// Use your shared component in the React app (see `packages/my-react-app/src/App.js`)
// Start the React app with `npm run start` and you should see an error. This is a known issue.
# See [Support Lerna and/or Yarn Workspaces #1333](https://github.com/facebook/create-react-app/issues/1333)

// So...What is a solution if Lerna is not officially supported in Create React App (CRA)? [babel-loader-lerna-cra](https://www.npmjs.com/package/babel-loader-lerna-cra) will work; just bear in mind this will override the Webpack configuration of the specified Create-React-App projects in a Lerna repo. Proceed with caution!
$ npm i -D babel-loader-lerna-cra

// Modify the Lerna root `package.json` to define `babel-loader-lerna-cra`
//  `imports` refers to components that the React app will need to transpile
//  `apps` refers to where the Webpack overrides will needs to happen

// Bootstrap the Webpack configs in our React app with `babel-loader-lerna-cra`
$ npx babel-loader-lerna-cra

// From the root of the directory, you may need to crosslink the packages one last time with `lerna bootstrap`
$ lerna bootstrap

// Voila!
```

At this point, you will have a fully functioning Lerna monorepo project that will give you
- Automatic transpilation of Lerna siblings
- React app hot reloading
  - This includes hot reloading of changes made to our `comp-button` component
  - Create React App (CRA) changes will continue to be hot reloaded
- Storybook hot reloading

### Upgrading to Storybook 5.0
When this guide was originally created, this project was using storybook modules `^4.1.13`.

The upgrade steps below are a consolidated recap of the [Storybook 5 Migration Guide](https://medium.com/storybookjs/storybook-5-migration-guide-d804b38c739d).

```sh
// Check for updates and install
$ npx npm-check-updates '/storybook/' -u && npm install

// Run the Storybook development server to verify changes have taken effect
$ npm run storybook
```

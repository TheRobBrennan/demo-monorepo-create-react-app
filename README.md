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

```
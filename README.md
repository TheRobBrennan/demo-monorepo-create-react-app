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

```
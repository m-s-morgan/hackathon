# Lowe's Grows Garden App

Want to be a better gardener? Follow the advice from experts at Lowe's and turn your black thumb into a green thumb in no time!

## Requirements

0. `Node/npm` - https://nodejs.org/en/
0. Cordova (`npm install cordova -g`)
0. Cordova platforms - https://cordova.apache.org/docs/en/4.0.0/guide/platforms/

## Installation

Use `npm install` to install the dependencies for this package.

## Typical Scenarios

### Development

When developing your app you want to do a couple of things:

0. Run a server to serve your app on `localhost`
0. Watch your TypeScript files and compile/bundle your TS/JS when a file changes.
0. Watch your Less files and compile your Less when a file changes.

Your `package.json` has a `start` script that will do these things for you. So when you are developing you can simply run:

```
$ npm start
```

The default server configuration is to serve assets on `http://localhost:3000` from the `app` directory.

> **NOTE**: `npm start` does not minify your javascript files in order to speed up your development builds

### Cordova (Deploying to devices)

When you're ready to build for your cordova project there are a couple of things you want to do:

0. Compile/Bundle/minify your TypeScript, JavaScript, and Less files.
0. Copy your assets (fonts, images) as well as your distribution source files (app.js, app.css, index.html) into the `cordova/www` directory
0. Modify your index.html to include cordova specific tags (i.e. including `cordova.js`, specifying the CSP meta tag, etc)
0. Build your cordova project

Your `package.json` has a `build:cordova` script that will do all of this for you. When you are ready to build your cordova project simple run:

```
$ npm run build:cordova
```

## Build Configuration

This app uses `npm run-script` or `npm run` scripts to manage all the building/deployment. You can see all
the scripts in the `package.json`. You can also print the scripts to the command line console by typing `npm run`.

### Useful Scripts

The following are descriptions for a few of the useful npm scripts. All of these scripts can be executed using the `npm run <script>` command.

- **build**
  - Builds/bundles/minifies your `less` and `ts`
  - copies the necessary files to `/cordova/www`
  - runs `cordova build`

- **clean**
  - Cleans your `app` directory, removing css/js/map files and your `dist` directory

- **lint**
  - Runs `tslint` on all of your `ts` files
  - You can specify your custom lint rules in your `tsconfig.json`
  - Default rules can be found at the [tsconfig-lint](https://github.com/wjohnsto/tsconfig-lint#user-content-default-rules) project

- **prepublish**
  - Runs during `npm install`
  - Installs/links TypeScript declarations using [tsd](http://definitelytyped.org/tsd/)
  - Runs [tsconfig-glob](https://github.com/wjohnsto/tsconfig-glob) to setup your `tsconfig.json`

- **start**
  - Builds/bundles your src files and watches them for changes
  - Rebuilds/bundles when your src files change
  - Starts server that serves assets from the `app` directory on http://localhost:3000

- **tsd**
  - Installs/links TypeScript declarations using [tsd](http://definitelytyped.org/tsd/)

## Project Structure

This project is setup to be as flat as possible while still providing a separation of concerns and allowing for extensibility.
The `app` directory contains all the public files used in the app. If you need to add a server/backend you can separate it out
as a `server` directory on the same level as `app`. Inside of `app` you have:

- **assets**
  - contains images/fonts/media files used in the app

- **lib**
  - contains any JS/CSS libraries not installed using node

- **src**
  - contains all the TS, HTML, and componentized LESS files

- **styles**
  - contains any global LESS styles used in the app
  - a `main.less` file exists in here as the entry point for all the styles in your app.
  - the `main.less` file is automatically managed by the CLI for components in the `src` directory

### The `src` Directory

Inside the `src` directory you will find all the TS, HTML, and componentized LESS files. The following subdirectories exist
inside `src` to help separate components in the app:

- **app**
  - contains the global plat.App object, used to configure your application and respond to global lifecycle events

- **injectables**
  - contains the injectables used in the app (such as converters, helper classes, formatting classes)

- **models**
  - contains a `models.d.ts` file, in which you can declare the interfaces for each model

- **services**
  - contains all the service classes
  - service classes are used to communicate with external APIs

- **templatecontrols**
  - contains any common TemplateControls

- **viewcontrols**
  - contains all the ViewControls for the app

There is also a `main.ts` file, which is used to pull all of your unreferenced TS sources together. You can reference any libraries
here. The CLI will automatically manage this file for you for the components in the directories listed above.

## Cordova

When creating an app you can choose whether or not you want to also create a Cordova app. There are a few things you want to know when creating/interacting with your Cordova app.

### Requirements

```
$ npm install cordova -g
```

Refer to the [android](http://cordova.apache.org/docs/en/edge/guide_platforms_android_index.md.html#Android%20Platform%20Guide) and [ios](http://cordova.apache.org/docs/en/edge/guide_platforms_ios_index.md.html#iOS%20Platform%20Guide)
platform guides to help you get setup with a Cordova environment.

### Working in Teams

It can be tricky to share a Cordova app project with other team members over source control. The recommended way we've found is by checking in your configuration to source control.

- Make sure you check-in your `cordova/config.xml`
- Check-in the `cordova/res` files
- Ignore everything else

You can use the `config.xml` to store project configuration, so that other team members can restore the same project specification on their machine.

- When adding a plugin, use the `--save` flag to save the plugin information (name and version specification) to the `config.xml`
- When adding a platform, the `--save` flag will store the platform information similar to the plugins
  - **NOTE** It is not recommended to save `ios` or `windows` platform information to the `config.xml` to avoid compatibility issues across platforms.

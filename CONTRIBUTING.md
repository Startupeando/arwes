# How to contribute

The Arwes project would love to welcome your contributions :blue_heart:

There are many ways to help out:

- Create an issue on GitHub, if you have found a bug
- Write test cases for open bug issues
- Write patches for open bug/feature issues, preferably with test cases included
- Contribute to the documentation
- Blog about how to use the package and its tools or brag about how it works for you

When contributing to this repository, please first discuss the change you wish to
make [via issue](https://github.com/arwesjs/arwes/issues/new) with the owners
of this repository before making a change.

Please note we have a [code of conduct](./CODE_OF_CONDUCT.md), please follow it
in all your interactions with the project.

## Guidelines

The Airbnb [JavaScript Style Guide](https://github.com/airbnb/javascript) is used.

In your editor or IDE, install the following tools plugins/packages:

- [editorconfig](http://editorconfig.org)
- [eslint](https://eslint.org)
- [prettier](https://prettier.io)

So the code style is code formatting of the project is followed.

## Development

This is a monorepo maintained with [lerna](https://lernajs.io) for bootstrapping,
but we don't used it for publishing. Each package is published independently.

### Install and setup

To install repository dependencies and bootstrap repository packages:

```bash
$ npm install
$ npm i -g lerna
$ lerna bootstrap
```

### Testing and code guidelines

To test the components and modules [jest](https://facebook.github.io/jest/),
[sinon](http://sinonjs.org), and [enzyme](http://airbnb.io/enzyme/) are used.
Run them using:

```bash
# test all packages once
$ npm run test

# tests with watcher
$ npm run test-dev

# run linter
$ npm run lint

# format code when needed
$ npm run format
```

### Git commit messages

For Git commit messages we use the following format:

- `feat: add a new feature with tests`
- `update: improve a current feature with tests`
- `fix: resolve a bugfix or issue`
- `refactor: change code structure with possibly breaking changes`
- `chore: changes in building, playing, testing, or any other process`
- `docs: update documentation either in code or markdown`

The syntax `[change] message` is now deprecated in this repo.

### Playground

TODO:

## Architecture

### Components

The React components follow this folder structure:

```text
/[componentNameInCamelCase]/
    [componentNameInCamelCase].js - The React component code without HOCs
    [componentNameInCamelCase].test.js - Component test cases
    styles.js - The component styles using JSS if they apply
    index.js - Export the component with their HOCs
    Readme.js - Component docs and small demos
```

- Components should be simple functions unless they really require to be classes.
- Use `React.PureComponent` when applicable.
- Components does not use their dependencies directly, they should be passed
down as props so the testing is easier.

### Tools makers

These are general purpose modules to be used independently of React components
and should work universally, client and server side.

These are modules to create tools instances. The tools makers should follow the
name convention: `make[ToolNameInCamelCase]`.

```text
/make[ToolNameInCamelCase]/
    make[ToolNameInCamelCase].js - The module
    make[ToolNameInCamelCase].test.js - Their test cases
    index.js - Export the module
    Readme.md - How to use
```

- All tools are creators to facilitate the dependency injection for any
purpose, mostly for testing. e.g. in a "water" package, this is a water cleaner
maker tool. All dependencies are optional but we can define our own.

```js
// packages/water/src/makeWaterCleaner/makeWaterCleaner.js //
const makeWaterCleaner = dependencies => {
    return {
        clean: water => dependencies.removeBacteria(dependencies.removeDirt(water))
    };
};

// packages/water/src/makeWaterCleaner/index.js //
import removeDirt from 'removeDirt';
import removeBacteria from 'removeBacteria';
import makeWaterCleaner from './makeWaterCleaner';
export default providedDependencies =>
  makeWaterCleaner({
    removeDirt,
    removeBacteria,
    ...providedDependencies
  });

// usage.js //
import makeWaterCleaner from '@arwes/water/makeWaterCleaner';
const removeBacteria = water => { ... };
const waterCleaner = makeWaterCleaner({ removeBacteria });
const cleanWater = waterCleaner.clean(myWater);
```

- When using the browser APIs, always reference the `window` object. e.g.
`window.document`, `window.localStorage`, `window.URL`...
- There should not be any call to either node or browser APIs on imports.

## Releasing

Before releasing, please make sure all tests pass and code format and styles
are ok.

Lerna is not used for releasing.

### NPM

Each package has to be compiled and released to npm independently.

In each package `/packages/[packageName]/` you wish to release, update their
`package.json` versions accordingly and run:

```bash
$ npm run compile
$ npm run release
```

### Git

We use the `master` branch to release the packages. Other branches are used
for development.

To release to Git in GitHub, in the `master` branch, use the npm command:

```bash
$ npm run release-git
```

It will update the `[CHANGELOG.md](./CHANGELOG.md)` file, create a commit and
a tag with the version of the `@arwes/arwes` package and push it to GitHub.

-------

Thanks for your contributions :blue_heart:

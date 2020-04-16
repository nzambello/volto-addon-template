# <volto-addon-name>

A widget for [Volto](https://github.com/plone/volto) to insert values for any language enabled

To be used with mrs-developer, see [Volto docs](https://docs.voltocms.com/customizing/add-ons/) for further usage informations.

## Setup with voltocli

```bash
voltocli
```

and insert `<volto-addon-name>` as addon name and `<volto-addon-url>` as addon URL.

## Manual setup

In your Volto project:

```bash
yarn add mrs-developer <volto-addon-path>
```

and in `package.json`:

```json
  "scripts": {
    "develop:npx": "npx -p mrs-developer missdev --config=jsconfig.json --output=addons",
    "develop": "missdev --config=jsconfig.json --output=addons",
    "preinstall": "if [ -f $(pwd)/node_modules/.bin/missdev ]; then yarn develop; else yarn develop:npx; fi",
    "postinstall": "rm -rf ./node_modules/volto-* && yarn omelette",
    ...
  }
```

Create a `mrs.developer.json` file:

```json
{
  "<volto-addon-name>": {
    "url": "<volto-addon-url>"
  }
}
```

In `jsconfig.json`:

```json
{
  "compilerOptions": {
    "paths": {
      "<volto-addon-name>": ["addons/<volto-addon-name>"]
    },
    "baseUrl": "src"
  }
}
```

Fix tests, in `package.json`:

```json
"jest": {
    ...
    "moduleNameMapper": {
      "@plone/volto/(.*)$": "<rootDir>/node_modules/@plone/volto/src/$1",
      "@package/(.*)$": "<rootDir>/src/$1",
      "<volto-addon-name>/(.*)$": "<rootDir>/src/addons/<volto-addon-name>/src/$1",
      "~/(.*)$": "<rootDir>/src/$1"
    },
    "testMatch": [
      "**/__tests__/**/*.[jt]s?(x)",
      "**/?(*.)+(spec|test).[jt]s?(x)",
      "!**/src/addons/volto/**/*"
    ],
    ...
```

Edit `.eslintrc`:

```json
{
  "extends": "./node_modules/@plone/volto/.eslintrc",
  "settings": {
    "import/resolver": {
      "alias": {
        "map": [
          ["@plone/volto", "@plone/volto/src"],
          ["@package", "./src"],
          ["<volto-addon-name>", "./src/addons/<volto-addon-name>/src"]
        ],
        "extensions": [".js", ".jsx", ".json"]
      },
      "babel-plugin-root-import": {
        "rootPathSuffix": "src"
      }
    }
  }
}
```

Add `src/addons` in `.gitignore`:

```
# .gitignore
src/addons
```

Then, run `yarn` and install dependencies:

```bash
yarn
```

## Usage

`Document your addon here`

# No-Sweat™ Eslint and Prettier Setup

## TLDR;

`npx install-peerdeps --dev eslint-config-matt123miller`

These are my settings for [ESLint](https://eslint.org/) and [Prettier](https://prettier.io/), including support for [TypeScript](https://www.typescriptlang.org/). Originally based on the [repo from Wes Bos](https://github.com/wesbos/eslint-config-wesbos) but forging my own path 🚀.

This also includes optional suggestions for adding TypeScript linting.

## Publishing changes

Ensure `np` is installed globally

`npx np --no-yarn --no-tests`

You can optionally add `--preview` as well to check what will happen.

## What it does

- Lints JavaScript based on the latest standards
- Fixes issues and formatting errors with Prettier
- Lints + Fixes inside of html script tags
- Lints + Fixes React via eslint-config-airbnb

## Per Project Installing - Recommended

I prefer to install this locally once per project, that way you can have project specific settings as well using this config as the default. This works with any text editor and shell but I personally use VSCode so I reference that.

1. Open your project folder in VSCode and open the integrated terminal.

2. If you don't already have a `package.json` file, create one with `npm i --y`.

3. Then we need to install everything needed by the config:

```
npx install-peerdeps --dev eslint-config-matt123miller
```

4. You can see in your package.json there are now a big list of devDependencies.

5. Create 2 files in the root of your project's directory (it should live where package.json does) named `.prettierrc.js` and `.eslintrc.js`. You can run the following 2 commands to create them.

```js
cat > .eslintrc.js << EOF
module.exports = {
  extends: ['matt123miller'],
};
EOF
```

and

```js
cat > .prettierrc.js << EOF
module.exports = {
  semi: true,
  trailingComma: 'es5',
  singleQuote: true,
  printWidth: 100,
  endOfLine: 'lf',
  tabWidth: 2,
};
EOF
```

Tip: You can alternatively put this object in your `package.json`. Read the docs for both prettier and eslint for examples.

6. You can add two scripts to your package.json to lint and/or fix:

```js
"scripts": {
  "lint": "eslint .",
  "lint:fix": "eslint . --fix"
},
```

7. Now you can manually lint your code by running `npm run lint` and fix all fixable issues with `npm run lint:fix`. You probably want your editor to do this though.

### Optiona TypeScript Support

1. Install additiona dev dependencies in your project.

```
npm i --save-dev @typescript-eslint/eslint-plugin @typescript-eslint/parser
```

2. Alter your `.eslintrc.js` to include extra support for the new packages.

```js
module.exports = {
  parser: '@typescript-eslint/parser',
  parserOptions: {
    sourceType: 'module',
    project: './tsconfig.json',
  },
  extends: [
    'matt123miller',
    'plugin:@typescript-eslint/recommended', // Add the default recommendations @typescript-eslint/eslint-plugin
    'prettier/@typescript-eslint', // Add prettier settings to avoid rules conflicts @typescript-eslint/eslint-plugin
    'plugin:prettier/recommended', // Add prettier plugin
  ],
};
```

Then install the now missing packages `npm i -D plugin:@typescript-eslint prettier/@typescript-eslint plugin:prettier`

## Updating the Per Project Install

Rerun the install command in your project to grab the latest version.

`npx install-peerdeps --dev eslint-config-matt123miller`

## Global Install

You can optionally globally install this if you want to have it affect all JS on your system as a backup. Typically eslint will look for a config file in your current directory and then keep searching the parent directory until it finds one. If your linting somewhere that doesn't have ESLint already set up then it would eventually fall back to the global version.

If this sounds useful to you then see [the section in Wes' repo](https://github.com/wesbos/eslint-config-wesbos#global-install).

## Settings

If you'd like to overwrite eslint or prettier settings, you can add the rules in your `.eslintrc.js` file. The [ESLint rules](https://eslint.org/docs/rules/) go directly under `"rules"` while [prettier options](https://prettier.io/docs/en/options.html) go under `"prettier/prettier"`. Note that prettier rules overwrite anything in my config (trailing comma, and single quote), so you'll need to include those as well.

```js
{
  "extends": [
    "matt123miller"
  ],
  "rules": {
    "no-console": 2,
    "prettier/prettier": [
      "error",
      {
        "trailingComma": "es5",
        "singleQuote": true,
        "printWidth": 120,
        "tabWidth": 8,
      }
    ]
  }
}
```

## With VS Code

You should read this entire thing. Serious!

Once you have done one, or both, of the above installs. You probably want your editor to lint and fix for you. Here are the instructions for VS Code:

1. Install the [ESLint package](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
2. Now we need to setup some VS Code settings via `Code/File` → `Preferences` → `Settings`. It's easier to enter these settings while editing the `settings.json` file, so click the Open (Open Settings) icon in the top right corner:

```js
// These are all my auto-save configs
"editor.formatOnSave": true,
// turn it off for JS and JSX, we will do this via eslint
"[javascript]": {
  "editor.formatOnSave": false
},
"[javascriptreact]": {
  "editor.formatOnSave": false
},
// show eslint icon at bottom toolbar
"eslint.alwaysShowStatus": true,
// tell the ESLint plugin to run on save
"editor.codeActionsOnSave": {
  "source.fixAll": true
},
// Optional BUT IMPORTANT: If you have the prettier extension enabled for other languages like CSS and HTML, turn it off for JS since we are doing it through Eslint already
"prettier.disableLanguages": ["javascript", "javascriptreact"],
```

After attempting to lint your file for the first time, you may need to click on 'ESLint' in the bottom right and select 'Allow Everywhere' in the alert window.

Finally you'll usually need to restart VS code. They say you don't need to, but it's never worked for me until I restart.

## With Create React App

1. Run `npx install-peerdeps --dev eslint-config-matt123miller`
1. Crack open your `package.json` and replace `"extends": "react-app"` with `"extends": "matt123miller"`

## With Gatsby

1. Run `npx install-peerdeps --dev eslint-config-matt123miller`
1. If you have an existing `.prettierrc` file, delete it.
1. follow the `Local / Per Project Install` steps above

## With Yarn

It should just work, but if they aren't showing up in your package.json, try `npx install-peerdeps --dev eslint-config-matt123miller -Y`

## 🤬🤬🤬🤬 IT'S NOT WORKING

Start fresh. Sometimes global modules can goof you up. This will remove them all:

```
npm uninstall @babel/eslint-parser eslint-config-matt123miller
 eslint eslint-config-prettier eslint-config-airbnb eslint-plugin-html eslint-plugin-prettier eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react prettier eslint-plugin-react-hooks
```

> To do the above a global install add the `--global` flag.

Then remove your `package-lock.json` file and delete the `node_modules/` directory.

Then follow the above instructions again.

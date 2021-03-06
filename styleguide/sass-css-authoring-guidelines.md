---
layout: layout.njk
title: Our SCSS and CSS Authoring Guidelines
tags: css
---

# {{ title }}

The Minimalist codebase is written in Sass(using the `SCSS` variant), an extension to the `CSS` language.

> Sass is a CSS preprocessor, which adds special features such as variables, nested rules and mixins (sometimes referred to as syntactic sugar) into regular CSS. The aim is to make the coding process simpler and more efficient.

For [more on Sass head over to the website](https://sass-lang.com/).

## Formatting

All formatting for `SCSS` and `CSS` files are auto formatted using [Prettier](https://prettier.io) defaults.
All projects should have the following dependencies installed and base configuration.

In the root of the project should be a file called `.prettierrc` with the following contents:

```json
{}
```

### Dependencies

- [Prettier](https://www.npmjs.com/package/prettier)
- [pretty-quick](https://www.npmjs.com/package/pretty-quick)
- [husky](https://www.npmjs.com/package/husky)

Install the above with:

```bash
yarn add prettier pretty-quick husky --dev
```

### NPM scripts

Inside `package.json` add the following inside the `scripts` block:

```json
...
"scripts": {
  ...
  "precommit": "npm run prettier:staged",
  "prettier:check": "prettier --check **/*.scss",
  "prettier:format": "prettier --write **/*.scss",
  "prettier:staged": "pretty-quick --staged --pattern \"**/*.scss\"",
  ...
},
...
```

You also need to tell Husky to run our `precommit` hook. Add the following to the end of your `package.json`

```json
"husky": {
  "hooks": {
    "pre-commit": "yarn run precommit"
  }
}
```

### Configure your editor

For details on configuring Prettier for your chosen editor, please follow
the [instructions on the Prettier website](https://prettier.io/docs/en/editors.html).

With the above set-up, you no longer have to worry about formatting your files and
can simply let Prettier take care of it o/\o

## Linting

Besides code style, we also enforce some general code quality via [Stylelint](https://stylelint.io/).

The following configuration and dependencies need to be present for all projects.

In the root of the project add a file named `.stylelintrc.json` with the following contents:

```json
{
  "extends": [
    "stylelint-config-recommended",
    "stylelint-config-recommended-scss",
    "stylelint-config-sass-guidelines",
    "stylelint-a11y/recommended",
    "stylelint-prettier/recommended"
  ],
  "rules": {
    "max-nesting-depth": 2,
    "declaration-no-important": true,
    "font-weight-notation": "named-where-possible",
    "color-hex-length": "short",
    "selector-pseudo-element-colon-notation": "double"
  }
}
```

### Dependencies

- [stylelint](https://www.npmjs.com/package/stylelint)
- [stylelint-a11y](https://www.npmjs.com/package/stylelint-a11y)
- [stylelint-prettier](https://www.npmjs.com/package/stylelint-prettier)
- [stylelint-config-prettier](https://www.npmjs.com/package/stylelint-config-prettier)
- [stylelint-config-recommended](https://www.npmjs.com/package/stylelint-config-recommended)

Because we use [`Sass\SCSS`](https://sass-lang.com/) to write our `CSS`, you also need the following:

- [stylelint-scss](https://www.npmjs.com/package/stylelint-scss)
- [stylelint-config-sass-guidelines](https://www.npmjs.com/package/stylelint-config-sass-guidelines)

For details on the rules enabled by `stylelint-config-sass-guidelines`, please see the [SASS Guidelines website](https://sass-guidelin.es/).

```bash
yarn add stylelint stylelint-a11y stylelint-prettier stylelint-config-prettier stylelint-config-recommended stylelint-scss stylelint-config-sass-guidelines --dev
```

### NPM scripts

Inside `package.json` add the following inside the `scripts` block:

```json
...
"scripts": {
  ...
  "lint": "stylelint sass/**/*.scss",
  ...
},
...
```

### Configure your editor

To configure your editor for `stylelint`, please [read the instructions on the Stylelint website](https://stylelint.io/user-guide/integrations/editor).

## The rules

With the above set-up your editor will inform you whever the code you write does not pass the linting rules. You can incrementally learn what the rules are that we adhere to by writing code and reacting to any warnings or errors.

There is benefit to having a general understanding of what these rules are though.
Because we use Prettier, we do not use any of the Stylelint rules that are simply
around code style. We do however turn on [all possible errors](https://stylelint.io/user-guide/rules/list#possible-errors) via the `stylelint-config-recommended` sharable config so, having a read over those is a good idea.

In addition to the above rules, we also enable the following:

```json
"rules": {
  "max-nesting-depth": 2,
  "declaration-no-important": true,
  "font-weight-notation": "named-where-possible"
}
```

### max-nesting-depth

In general one level of nesting is sufficient for most cases. We do acknowledge that this is not always reasonable and so, we allow up to two levels of nesting. i.e.

```css
a {
  b {
    /* 1 */
    .foo {
    } /* 2 */
  }
}
```

The following would therefore be an error:

```css
a {
  b {
    /* 1 */
    .foo {
      /* 2 */
      input {
        /* 3 */
      }
    }
  }
}
```

You can [read more about the rule here](https://stylelint.io/user-guide/rules/max-nesting-depth).

### declaration-no-important

The use of the `!important` in declarations are not allowed and there will have to be a very compelling reason for us to consider an [exception](https://stylelint.io/user-guide/ignore-code).

You can [read more about the rule here](https://stylelint.io/user-guide/rules/declaration-no-important).

### font-weight-notation

For this rule we have opted for [`named-where-possible`](https://stylelint.io/user-guide/rules/font-weight-notation#named-where-possible). This means you have the freedom to use numeric values for your font weights unless those weights are `400` or `700`. These are equivalent to the named values `normal` and `bold` which are preferred.

You can [read more about the rule here](https://stylelint.io/user-guide/rules/font-weight-notation).

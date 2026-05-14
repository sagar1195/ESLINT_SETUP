# ESLint Setup Guide

## Install and Initialize ESLint 9

```bash
    pnpm install eslint @eslint/js eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-react-refresh @typescript-eslint/parser @typescript-eslint/eslint-plugin --save-dev
```

## 1. Initialize ESLint:

```bash
    npx eslint --init
```
### When prompted:
-   Choose: “To check syntax, find problems, and enforce code style”
-   Choose: “JavaScript modules (import/export)”
-   Choose: “React”
-   Choose: “TypeScript”
-   Choose: “Browser”
-   Accept the automatic dependency installation

This creates a file named eslint.config.js — the new flat config format.

## 2. Configure ESLint (Flat Config)

Open your eslint.config.js and replace its content with:

```js
import js from "@eslint/js";
import globals from "globals";
import reactPlugin from "eslint-plugin-react";
import reactHooks from "eslint-plugin-react-hooks";
import tseslint from "@typescript-eslint/eslint-plugin";
import tsParser from "@typescript-eslint/parser";

export default [
  {
    ignores: ["dist", "node_modules"],
  },
  {
    files: ["**/*.{ts,tsx,js,jsx}"],
    languageOptions: {
      parser: tsParser,
      parserOptions: {
        ecmaVersion: "latest",
        sourceType: "module",
        ecmaFeatures: { jsx: true },
      },
      globals: globals.browser,
    },
    plugins: {
      react: reactPlugin,
      "react-hooks": reactHooks,
      "@typescript-eslint": tseslint,
    },
    rules: {
      ...js.configs.recommended.rules,
      ...tseslint.configs.recommended.rules,
      ...reactPlugin.configs.recommended.rules,
      ...reactHooks.configs.recommended.rules,
      // Custom project rules
      "react/react-in-jsx-scope": "off",
      "react/prop-types": "off",
      "@typescript-eslint/no-unused-vars": ["warn", { argsIgnorePattern: "^_" }],
    },
    settings: {
      react: { version: "detect" },
    },
  },
];
```

## 3. Add Integration
Next, install Prettier and ESLint plugins to integrate it seamlessly:

```bash
pnpm install prettier eslint-config-prettier eslint-plugin-prettier --save-dev
```

Create a .prettierrc file at the root:
```json
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 100,
  "tabWidth": 2,
  "endOfLine": "auto"
}
```
Then, update eslint.config.js to include Prettier:
```js
import prettier from "eslint-plugin-prettier";
import prettierConfig from "eslint-config-prettier";
```

## 4. Add NPM Scripts
Open your package.json and include these scripts:
```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "lint": "eslint .",
    "lint:fix": "eslint . --fix",
    "format": "prettier --write ."
  }
}
```

## 5. Integrate ESLint and Prettier with VS Code
-   press 'shif + ,' to open vscode setting
-   go to workshop tab
-   click on "Open settings (JSON)". This should create new settings.json inside .vscode folder if it doesn't exit
-   past the json
```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "always"
  }
}
```
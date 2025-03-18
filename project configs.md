# Vite


Находясь в нужной папке в терминале пишешь `npm create vite@latest .`

Точка означает текущую папку

Выбираем vanile => JavaScripte или TypeScripte. 

После установки `npm install`

В корне проекта создаем файл `vite.config.ts` 

```
import { defineConfig } from 'vite';

export default defineConfig({
  base: '',
  build: {
    outDir: 'dist',
  },
});
```

# gh-pages


`npm install gh-pages --save-dev`

в `package.json` добавляем 

```
"scripts": {
 "deploy": "gh-pages -d dist -e нужноеИмяПапки"
}
```


# Prettier


`npm install --save-dev prettier`

В корне проекта создаем файл `.prettierrc` с содержимым
```
{ 
  "tabWidth": 2,
  "semi": true, 
  "singleQuote": true, 
  "printWidth": 80, 
  "trailingComma": "all", 
  "bracketSpacing": true, 
  "arrowParens": "always" 
}
```

В корне проекта создаем файл `.prettierignore` с содержимым

```
node_modules
dist
```

в `package.json` добавляем 

```
"scripts": {
  "format": "prettier --write '{src/**/*, *}.{js,ts,json,css}'",
  "check-format": "prettier --list-different --ignore-unknown ."
}
```
• --write — автоматически форматирует файлы, указываем нужные папки и расширения

• --list-different — показывает файлы, которые не соответствуют форматированию

• --ignore-unknown — игнорирует неизвестные типы файлов, точка означает всю текущую папку


# ESLint


`npm install eslint @eslint/js @eslint/eslintrc @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint-plugin-unicorn eslint-plugin-prettier eslint-config-prettier globals --save-dev`

•	@eslint/js: Поддержка линтинга JavaScript.

•	@eslint/eslintrc: Новый формат конфигурации.

•	@typescript-eslint/parser: Поддержка TypeScript для линтинга.

•	@typescript-eslint/eslint-plugin: Плагин с правилами TypeScript.

•	eslint-plugin-unicorn: Современные оптимизации для JavaScript.

•	eslint-plugin-prettier: Интеграция правил Prettier.

•	eslint-config-prettier: Отключение конфликтующих правил.

• globals: Предоставляет предопределенные наборы глобальных переменных для различных окружений (например, браузер, Node.js).


В корне проекта создаем файл `eslint.config.js` с содержимым

```
import js from "@eslint/js";
import tsPlugin from "@typescript-eslint/eslint-plugin";
import tsParser from "@typescript-eslint/parser";
import unicorn from "eslint-plugin-unicorn";
import prettier from "eslint-plugin-prettier";
import prettierConfig from "eslint-config-prettier";
import globals from "globals";

export default [
  js.configs.recommended,
  {
    files: ["**/*.ts", "**/*.js"],
    ignorePatterns: ["dist/", "node_modules/"],
    languageOptions: {
      parser: tsParser,
      ecmaVersion: "latest",
      sourceType: "module",
      globals: {
        ...globals.browser,
      },
    },
    plugins: {
      "@typescript-eslint": tsPlugin,
      unicorn,
      prettier,
    },
    rules: {
      ...tsPlugin.configs["recommended-type-checked"].rules,
      ...unicorn.configs.recommended.rules,
      "prettier/prettier": "error", 
      "@typescript-eslint/consistent-type-assertions": [
        "error",
        { assertionStyle: "never" },
      ],
      "@typescript-eslint/consistent-type-imports": "error",
      "@typescript-eslint/explicit-function-return-type": "error",
      "@typescript-eslint/explicit-member-accessibility": [
        "error",
        { accessibility: "explicit", overrides: { constructors: "off" } },
      ],
      "@typescript-eslint/member-ordering": "error",
      "class-methods-use-this": "error",
      "noInlineConfig": "error",
      "reportUnusedDisableDirectives": "error",
    },
  },
  prettierConfig,
];
```

в `package.json` добавляем 

```
"scripts": {
  "lint": "eslint .",
  "lint:fix": "eslint . --fix"
}
```


# StyleLint


`npm install stylelint stylelint-config-standard stylelint-config-clean-order --save-dev`

в корне создаем файл `.stylelintrc` с содержанием

```
{
  "extends": ["stylelint-config-standard", "stylelint-config-clean-order"],
  "rules": {
    "at-rule-no-unknown": [
      true,
      {
        "ignoreAtRules": [
           "value",
           "import",
           "layer",
           "media"
        ]
      }
    ],
    "selector-class-pattern": "^[a-z][a-zA-Z0-9]*$",
    "no-descending-specificity": null,
    "property-no-unknown": true
  }
}

```
• at-rule-no-unknown: Проверяет, что используются только известные директивы @rule. В ignoreAtRules можно указать исключения (например, import, value)

• selector-class-pattern: Устанавливает шаблон для имен CSS-классов. Шаблон для camelCase: "^[a-z][a-zA-Z0-9]*$" 

• no-descending-specificity: Отключено (null), чтобы не проверять конфликты, где более специфичные селекторы следуют за менее специфичными.

• property-no-unknown: Проверяет, что используются только корректные CSS-свойства. Помогает исключить опечатки или несуществующие свойства.


в `package.json` добавляем 

```
"scripts": {
  "stylelint": "stylelint 'src/**/*.{css,module.css}'",
  "stylelint:fix": "stylelint 'src/**/*.{css,module.css}' --fix"
}
```

# Husky

`npm install --save-dev husky lint-staged @commitlint/cli @commitlint/config-conventional`

`npx husky init`

Это создаст папку .husky и настроит её для работы с git-хуками.

```
echo "npx lint-staged" > .husky/pre-commit
```

```
echo "npx --no commitlint --edit $1" > .husky/commit-msg
```

```
echo "export default { extends: ['@commitlint/config-conventional'] };" > commitlint.config.js
```

в `package.json` добавляем под script{}

```
"lint-staged": {
  "src/*.{js,ts,json}": [
    "npm run lint:fix",
    "npm run format"
  ],
  "src/*.{css,scss,module.css}": [
    "npm run stylelint:fix",
    "npm run format"
  ]
}
```


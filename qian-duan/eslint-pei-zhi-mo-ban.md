# eslint 配置模板

## 为啥写这个

每次配置都挺费时间的，还得去老项目里捞，不如直接按所需类型将步骤抽离，每次直接拷贝复用。

## eslint

* 安装

```text
npm install --save-dev eslint
# or
yarn add -D eslint
```

* 配置

```javascript
// .eslintrc.js
module.exports = {
    env: {
        browser: true,
        es2020: true,
        node: true,
    },
    extends: ['eslint:recommended'],
    parserOptions: {
        ecmaVersion: 11,
        sourceType: 'module',
    },
}
```

## eslint + standard

eslint 提供一套基于 standard 的格式化规范，具体可查看[https://standardjs.com/](https://standardjs.com/)，其团队也提供一个叫 [standard](https://www.npmjs.com/package/standard) 的工具，但是在我看来还是配合 vscode + eslint 食用更加

**建议和 prettier & eslint 二选一**

* 安装

```text
# eslint 部分
npm install --save-dev eslint
# or
yarn add -D eslint

# standard 部分
npm install --save-dev eslint eslint-config-standard eslint-plugin-standard eslint-plugin-promise eslint-plugin-import eslint-plugin-node
# or
yarn add -D eslint eslint-config-standard eslint-plugin-standard eslint-plugin-promise eslint-plugin-import eslint-plugin-node
```

* 配置

```javascript
// .eslintrc.js
module.exports = {
    env: {
        browser: true,
        es2020: true,
        node: true,
    },
    extends: ['eslint:recommended', 'standard'],
    parserOptions: {
        ecmaVersion: 11,
        sourceType: 'module',
    },
}
```

## eslint + prettier

prettier 和 eslint 配合食用，强迫症很舒服

**建议和 standard 二选一**

* [prettier 配置](https://prettier.io/docs/en/options.html)

这是我个人比较喜欢的配置，按照描述自己配置就好，每次翻文档好累，直接在这按照描述改就好~

```javascript
// .prettierrc.js
module.exports = {
    // 换行列数 <int>
    printWidth: 120,
    // 取代 tab 的空格数 <int>
    tabWidth: 4,
    // 使用 tab 而不是空格 <bool>
    useTabs: false,
    // 每行末尾是否添加分号 <bool>
    semi: false,
    // 单引号还是双引号 <bool>
    singleQuote: true,
    // object key 是否带括号 <as-needed|consistent|preserve>
    // "as-needed" - 仅在需要时在对象属性周围添加引号。
    // "consistent" - 如果对象中至少有一个属性需要引号，则引用所有属性。
    // "preserve" - 尊重对象属性中引号的输入用法。
    quoteProps: 'as-needed',
    // jsx 的属性用单引号还是双 <bool>
    jsxSingleQuote: false,
    // 多行时是否打印尾随逗号 <"none" - 没有尾随逗号, "es5" - 在ES5中有效的尾随逗号（对象，数组等）, "all" - 尽可能使用尾随逗号（包括函数参数）>
    trailingComma: 'es5',
    // Object 文字是否填充空格 {'1e2': 1} -> { '1e2': 1 } <bool>
    bracketSpacing: true,
    //  多行 JSX 末尾结束符位置  (...\n>)  -> (...>) <bool>
    jsxBracketSameLine: true,
    // 单参数的箭头函数参数是否加上括号 <always|avoid>
    arrowParens: 'always',
    // 换行符用啥 <lf|crlf|cr|auto>
    endOfLine: 'lf'
}
```

* 安装

```text
# eslint 部分
npm install --save-dev eslint
# or
yarn add -D eslint

# prettier 部分
npm install --save-dev prettier eslint-plugin-prettier eslint-config-prettier
# or
yarn add -D prettier eslint-plugin-prettier eslint-config-prettier
```

* 配置

```javascript
module.exports = {
    env: {
        browser: true,
        es2020: true,
        node: true,
    },
    extends: ['eslint:recommended', 'plugin:prettier/recommended'],
    parserOptions: {
        ecmaVersion: 11,
        sourceType: 'module',
    },
}
```

## eslint + prettier + typescript

* 安装

```text
# eslint 部分
npm install --save-dev eslint
# or
yarn add -D eslint

# prettier 部分
npm install --save-dev prettier eslint-plugin-prettier eslint-config-prettier
# or
yarn add -D prettier eslint-plugin-prettier eslint-config-prettier

# TS 语言部分
npm install --save-dev @typescript-eslint/eslint-plugin @typescript-eslint/parser typescript
# or
yarn add -D @typescript-eslint/eslint-plugin @typescript-eslint/parser typescript

# ALL
npm install --save-dev eslint prettier eslint-plugin-prettier eslint-config-prettier @typescript-eslint/eslint-plugin @typescript-eslint/parser typescript
# or
yarn add -D eslint prettier eslint-plugin-prettier eslint-config-prettier @typescript-eslint/eslint-plugin @typescript-eslint/parser typescript
```

* 配置

```javascript
module.exports = {
    env: {
        browser: true,
        es2020: true,
        node: true,
    },
    // https://www.npmjs.com/package/@typescript-eslint/parser
    parser: '@typescript-eslint/parser',
    parserOptions: {
        ecmaVersion: 11,
    },
    extends: ['eslint:recommended', 'plugin:@typescript-eslint/recommended', 'plugin:prettier/recommended'],
    plugins: ['@typescript-eslint'],
}
```

## eslint + prettier + react

* 安装

```text
# eslint 部分
npm install --save-dev eslint
# or
yarn add -D eslint

# prettier 部分
npm install --save-dev prettier eslint-plugin-prettier eslint-config-prettier
# or
yarn add -D prettier eslint-plugin-prettier eslint-config-prettier

# react 部分
npm install --save-dev eslint-plugin-react eslint-plugin-react-hooks
# or
yarn add -D eslint-plugin-react eslint-plugin-react-hooks

# ALL
npm install --save-dev eslint prettier eslint-plugin-prettier eslint-config-prettier eslint-plugin-react eslint-plugin-react-hooks
# or
yarn add -D eslint prettier eslint-plugin-prettier eslint-config-prettier eslint-plugin-react eslint-plugin-react-hooks
```

* 配置

```javascript
module.exports = {
    env: {
        browser: true,
        es2020: true,
        node: true,
    },
    extends: ['eslint:recommended', 'plugin:prettier/recommended','plugin:react/recommended', 'plugin:react-hooks/recommended'],
    parserOptions: {
        ecmaVersion: 11,
        sourceType: 'module',
    },
}
```

## eslint + prettier + react + typescript

* 安装

```text
# eslint 部分
npm install --save-dev eslint
# or
yarn add -D eslint

# prettier 部分
npm install --save-dev prettier eslint-plugin-prettier eslint-config-prettier
# or
yarn add -D prettier eslint-plugin-prettier eslint-config-prettier

# TS 语言部分
npm install --save-dev @typescript-eslint/eslint-plugin @typescript-eslint/parser typescript
# or
yarn add -D @typescript-eslint/eslint-plugin @typescript-eslint/parser typescript

# react 部分
npm install --save-dev eslint-plugin-react eslint-plugin-react-hooks
# or
yarn add -D eslint-plugin-react eslint-plugin-react-hooks

# ALL
npm install --save-dev eslint prettier eslint-plugin-prettier eslint-config-prettier @typescript-eslint/eslint-plugin @typescript-eslint/parser typescript eslint-plugin-react eslint-plugin-react-hooks
# or
yarn add -D eslint prettier eslint-plugin-prettier eslint-config-prettier @typescript-eslint/eslint-plugin @typescript-eslint/parser typescript eslint-plugin-react eslint-plugin-react-hooks
```

* 配置

```javascript
module.exports = {
    env: {
        browser: true,
        es2020: true,
        node: true,
    },
    // https://www.npmjs.com/package/@typescript-eslint/parser
    parser: '@typescript-eslint/parser',
    parserOptions: {
        ecmaVersion: 11,
    },
    extends: [
        'eslint:recommended',
        'plugin:@typescript-eslint/recommended',
        'plugin:prettier/recommended',
        'plugin:react/recommended',
        'plugin:react-hooks/recommended',
    ],
    plugins: ['@typescript-eslint'],
}
```

## eslint + prettier + vue \(待定\)

* 安装

```text
# eslint 部分
npm install --save-dev eslint
# or
yarn add -D eslint

# prettier 部分
npm install --save-dev prettier eslint-plugin-prettier eslint-config-prettier
# or
yarn add -D prettier eslint-plugin-prettier eslint-config-prettier

# vue 部分
npm install --save-dev eslint-plugin-vue
# or
yarn add -D eslint-plugin-vue

# ALL
npm install --save-dev eslint prettier eslint-plugin-prettier eslint-config-prettier eslint-plugin-vue
# or
yarn add -D eslint prettier eslint-plugin-prettier eslint-config-prettier eslint-plugin-vue
```

* 配置

```javascript
module.exports = {
    env: {
        browser: true,
        es2020: true,
        node: true,
    },
    extends: ['eslint:recommended', 'plugin:vue/recommended', 'plugin:prettier/recommended'],
    parserOptions: {
        ecmaVersion: 11,
        sourceType: 'module',
    },
    rules: {
        'vue/html-indent': 0,
    },
}
```


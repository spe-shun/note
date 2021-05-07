# 基于已有 webpack 配置引入

## 安装

STEP 1：下载安装

```bash
# 使用了 postcss 的其他插件，只能安装 postcss 7 兼容版本
npm install tailwindcss@npm:@tailwindcss/postcss7-compat @tailwindcss/postcss7-compat postcss@^7 autoprefixer@^9
# or
yarn add tailwindcss@npm:@tailwindcss/postcss7-compat @tailwindcss/postcss7-compat postcss@^7 autoprefixer@^9

# 正常安装
npm install tailwindcss postcss
# or
yarn add tailwindcss postcss
```

STEP 2：webpack 增加 tailwindcss 插件

```javascript
/* 方式 1 */

// webpack.config.js
...
{
    test: /\.css$/,
    exclude: /\.module\.css$/,
    use: [
        'css-loader',
        {
        // Options for PostCSS as we reference these options twice
        // Adds vendor prefixing based on your specified browser support in
        // package.json
        loader: 'postcss-loader',
        options: {
            plugins: () => [
                ...
                require('tailwindcss'),
                ...
            ]
        },
    ],
}
...

/* 方式 2 （官方）*/

// postcss.config.js
module.exports = {
  plugins: {
    tailwindcss: {}
  }
}

// webpack.config.js
...
{
    test: /\.css$/,
    exclude: /\.module\.css$/,
    use: [
        'css-loader',
        'postcss-loader',
    ],
}
...
```

STEP 3：在 index.css 增加引入

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

STEP 4：更目录增加 tailwind 配置文件

```javascript
// tailwind.config.js
module.exports = {
    purge: [],
    darkMode: false, // or 'media' or 'class'
    theme: {
        extend: {},
    },
    variants: {},
    plugins: [],
}
```

## TIPS

### vscode 的 css 报错

在 `setting.json`增加以下配置：

```javascript
{
    ...
    "css.customData": [
        ".vscode/css.customData.json"
    ]
    ...
}
```

在 `.vscode` 文件夹增加一个 `css.customData.json` 文件：

```javascript
{
    "atDirectives": [
        {
            "name": "@tailwind",
            "description": "Use the @tailwind directive to insert Tailwind’s `base`, `components`, `utilities`, and `screens` styles into your CSS.",
            "references": [
                {
                    "name": "Tailwind’s “Functions & Directives” documentation",
                    "url": "https://tailwindcss.com/docs/functions-and-directives/#tailwind"
                }
            ]
        }
    ]
}
```


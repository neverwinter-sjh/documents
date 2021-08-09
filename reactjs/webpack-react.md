# Webpack에서 React 설정하기

CRA가 아닌 Webpack부터 시작하여 React 개발에 필요한 것들을 설정한다.

## 시작하기

폴더를 만들고 package.json 파일을 생성한다.

```
mkdir my-app
cd my-app
npm init -y
```

## react 설치

```
yarn add react react-dom
```

## babel, eslint 설치

```
yarn add --dev @babel/core babel-loader @babel/preset-react @babel/preset-env eslint
```

## webpack 및 plugin

```
yarn add -D webpack webpack-cli html-loader file-loader css-loader sass-loader sass html-webpack-plugin mini-css-extract-plugin dotenv-webpack uglifyjsWebpackPlugin
```

## webpack.config.js

```
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const Dotenv = require('dotenv-webpack');

module.exports = (env, options) => {
  const config = {
    // 앱 시작 js 파일
    entry: path.resolve(__dirname, './src/index.js'),

    // 번들링 결과 파일 설정
    output: {
      publicPath: '/',
      filename: 'bundle.[chunkhash].js',
    },

    // 모듈 설정
    module: {
      rules: [
        // { // pre 설정으로 eslint부터 로드하도록 설정
        //   enforce: 'pre',
        //   test: /\.js$/,
        //   exclude: /node_modules/,
        //   loader: 'eslint-loader',
        // },
        {
          // js, jsx 파일을 babel-loader로 해석하여 트랜스파일링한다.
          test: /\.(js|jsx)$/,
          exclude: /node_modules/,
          use: {
            loader: 'babel-loader',
          },
        },
        {
          // html 파일을 읽을 수 있다.
          test: /\.html$/,
          use: [
            {
              loader: 'html-loader',
              options: {
                minimize: true,
              },
            },
          ],
        },

        // 파일 로더(주로 폰트 파일)
        {
          test: /\.(woff|woff2|eot|ttf|otf)$/,
          loader: 'file-loader',
        },

        // css, sass 파일 로더
        {
          test: /\.(css|sass|scss)$/,
          use: [MiniCssExtractPlugin.loader, 'css-loader', 'sass-loader'],
        },
      ],
    },
    plugins: [
      // 템플레이트를 사용할 수 있도록 설정
      new HtmlWebpackPlugin({
        template: path.resolve(__dirname, './public/index.html'),
      }),
      // css 추출
      new MiniCssExtractPlugin({
        filename: '[name].css',
        chunkFilename: '[id].css',
      }),
      // 이전 build 삭제
      new CleanWebpackPlugin(),
      // dotEnv 환경 파일 설정
      new Dotenv({
        safe: true, // load '.env.example' to verify the '.env' variables are all set. Can also be a string to a different file.
        allowEmptyValues: true, // allow empty variables (e.g. `FOO=`) (treat it as empty string, rather than missing)
        systemvars: true, // load all the predefined 'process.env' variables which will trump anything local per dotenv specs.
        silent: true, // hide any errors
        defaults: false, // load '.env.defaults' as the default values if empty.
      }),
    ],
    devtool: 'inline-source-map',
  };

  // 배포
  if (options.mode === 'production') {
    config.mode = 'production';
    config.plugins = [
      ...config.plugins,
      ...[
        new uglifyjsWebpackPlugin({
          cache: true,
          parallel: true,
          sourceMap: true,
        }),
      ],
    ];
  } else {
    // 개발
    config.mode = 'development';
    // 개발용 서버 설정
    config.devServer = {
      host: 'localhost',
      port: process.env.PORT || 3000,
      contentBase: path.resolve(__dirname, './src/assets/'), // 정적 파일을 배치할 폴더
      open: true,
      hot: true,
      historyApiFallback: true,
      stats: {
        cached: false,
        cachedAssets: false,
        chunks: false,
        chunkModules: false,
        chunkOrigins: false,
        modules: false,
      },
    };
  }

  return config;
};
```

## /public/index.html
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="description" content="Web site created using webpack, react" />
    <title>Webpack React</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>
```

## src 폴더 구조

```
src - assets - css (sass 가능)
             - images
             - fonts
```

## index.js

sass 파일 불러올 

```
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './assets/css/common.scss';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```

## App.js

img tag src 경로 참조 => /src/assets, 기본 루트로 설정되어 있다.

```
import React from 'react';

const App = () => { 
  return (
  <div className="app-container">
    <img src="/images/image1.jpg" alt="" />
  </div>
  )
};

export default App;
```

## /assets/css/common.scss

font 파일 및 background image 경로 설정 참조

```
@font-face {
  font-family: "NanumGothic";
  font-style: normal;
  font-weight: 400;
  // src: url("../fonts//NanumGothic.eot");
  src: url("../fonts//NanumGothic.woff2") format("woff2"),
       url("../fonts//NanumGothic.woff") format("woff");
}


body {
  font-family: 'NanumGothic';

  .d2coding {
    font-family: 'D2Coding';
  }

  .normal {
    font-family:'Courier New', Courier, monospace;
  }
  .app-container {
    background-color: yellow;
    height:500px;
    background:url(../images/image1.jpg) no-repeat 0 0;
  }
}
```

## .babelrc

```
{
  "presets": [
      "@babel/env",
      "@babel/react"
  ]
}
```

## package.json 

```
"scripts": {
  "start": "webpack serve --mode development --hot",
  "build": "webpack --mode production"
},
```

### eslint 플러그인 설치

```
yarn eslint --init
```

eslint의 기본 설정을 init하고 마지막에 npm 설치를 해준다.

#### eslint init 순서
1. To check syntax, find problems, and enforce code style
2. Javascript modules (import/export)
3. React
4. Does your project use Typescript (NO)
5. Where does your code run? > Check Browser & Node (click space to check)
6. Use a popular style guide
7. Airbnb
8. Javascript
9. NPM 설치 (YES)

Airbnb 관련 설정은 쓰지 않을 것이므로 삭제한다.

```
yarn remove eslint-config-airbnb
```

yarn을 사용하고 있으므로 충돌 방지를 위해 npm lock을 제거한다.

```bash
rm -rf package-lock.json

(windows)
del /f /q /a package-lock.json 
```

### .eslintrc.js 수정

```
module.exports = {
  env: {
    browser: true,
    commonjs: true,
  },
  extends: ['eslint:recommended', 'plugin:react/recommended'],
  parser: '@babel/eslint-parser',
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 2020,
    sourceType: 'module',
  },
  plugins: ['react', 'eslint-plugin'],
  rules: {},
};
```

### .prettierrc 설정

```
{
  "singleQuote": true,
  "semi": true,
  "useTabs": false,
  "tabWidth": 2,
  "trailingComma": "es5",
  "printWidth": 80,
  "jsxBracketSameLine": false,
  "arrowParens": "avoid",
  "proseWrap": "never"
}
```

## Typscript 설정

```
yarn add -D typescript ts-loader @types/react @types/react-dom @types/webpack
```

### webpack.config.js

```
// rules에 추가
{
  test: /\.(ts|tsx)$/,
  exclude: /node_modules/,
  use: 'ts-loader'
},
```

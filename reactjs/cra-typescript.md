# CRA + Typescript

## Stacks

* React(create-react-app)
* Typescript
* Eslint
* Prettier
* Craco

## 순서

### create-react-app

```
yarn create react-app my-app --template typescript
```

create-react-app을 이용해서 template typescript로 설치한다.

typescript를 사용하지 않을 때는 아래와 같이 생성한다.

```
yarn create react-app my-app
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
4. Does your project use Typescript (Yes)
5. Where does your code run? > Check Browser & Node (click space to check)
6. Use a popular style guide
7. Airbnb
8. .eslintrc
9. yarn 설치 (YES)

Airbnb 관련 설정은 쓰지 않을 것이므로 삭제한다.

```
yarn remove eslint-config-airbnb
```

### .eslintrc 수정

```
{
  "env": {
    "browser": true,
    "commonjs": true
  },
  "extends": ["eslint:recommended", "plugin:react/recommended", "plugin:@typescript-eslint/recommended"],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaFeatures": {
      "jsx": true
    },
    "ecmaVersion": "latest",
    "sourceType": "module"
  },
  "plugins": ["react", "@typescript-eslint"],
  "rules": {
    "@typescript-eslint/explicit-module-boundary-types": "off",
    "@typescript-eslint/no-var-requires": "off"
  }
}
```

### .prettierrc 설정

```
{
  "singleQuote": true,
  "semi": false,
  "useTabs": false,
  "tabWidth": 2,
  "trailingComma": "es5",
  "printWidth": 400,
  "jsxBracketSameLine": false,
  "arrowParens": "avoid",
  "proseWrap": "never",
  "singleAttributePerLine": true,
}
```

### croco 설치

```
yarn add @craco/craco craco-alias
```

### craco.config.ts 작성(typescript)

```
const CracoAlias = require('craco-alias')

module.exports = {
  plugins: [
    {
      plugin: CracoAlias,
      options: {
        source: 'tsconfig',
        tsConfigPath: 'tsconfig.paths.json',
      },
    },
  ],
}
```

### tsconfig.paths.json 작성
```
{
  "compilerOptions": {
    "baseUrl": "./",
    "paths": {
      "@hooks/*": ["src/hooks/*"],
      "@pages/*": ["src/pages/*"],
      "@services/*": ["src/services/*"],
      "@utils/*": ["src/utils/*"],
      "@screens/*": ["src/screens/*"],
      ...
    }
  }
}
```

### tsconfig.json 설정
```
{
  "extends": "./tsconfig.paths.json",
  "compilerOptions": {
    ...
  },
  "include": [
  	...
  ]
}
```

### craco.config.js (javascript)
```
const CracoAlias = require('craco-alias')

module.exports = {
  plugins: [
    {
      plugin: CracoAlias,
      options: {
        source: 'options',
        baseUrl: './',
        aliases: {
          '@api': './src/api',
          '@assets': './src/assets',
          '@components': './src/components',
          '@hooks': './src/hooks',
          '@pages': './src/pages',
          '@style': './src/style',
          '@utils': './src/utils',
        },
      },
    },
  ],
}
```

### package.json 수정
```
{
  "dependencies": {
    ...
  },
  "scripts": {
    "start": "craco start",
    "build:dev": "craco start",
    "build:prod": "craco start",
    "build": "craco build",
    "test": "craco test",
    "eject": "react-scripts eject",
    "format": "prettier --write ."
  },
  "devDependencies": {
    ...
  }
}
```


### Sass(Scss) 사용하기

```
yarn add sass sass-loader
```




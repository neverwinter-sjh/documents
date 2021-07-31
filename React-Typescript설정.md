# React Typescript 설정

## Stacks

* React
* Typescript
* Eslint
* Prettier

## 순서

### create-react-app

```
yarn create react-app my-app --template typescript
```

create-react-app을 이용해서 template typescript로 설치한다.

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
8. Javascript
9. NPM 설치 (YES)

Airbnb 관련 설정은 쓰지 않을 것이므로 삭제한다.

```
yarn remove eslint-config-airbnb
```

### 플러그인 설명

* eslint : 코드의 문법을 검사하는 린팅과 코드의 스타일을 잡아주는 포맷팅 기능
* @typescript-eslint/eslint-plugin : Typescript 관련 린팅규칙을 설정하는 플러그인
* @typescript-eslint/parser : Typescript 를 파싱하기 위해 사용
* eslint-plugin-react : React에 관한 린트설정을 지원
* eslint-plugin-react-hooks : React Hooks의 규칙을 강제하도록 하는 플러그인
* eslint-plugin-jsx-a11y : JSX 내의 접근성 문제에 대해 즉각적인 AST 린팅 피드백을 제공
* eslint-plugin-import : ES2015+의 import/export 구문을 지원하도록 함

### .eslintrc.js 수정

```
module.exports = {
  env: {
    browser: true,
    commonjs: true,
  },
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:@typescript-eslint/recommended',
  ],
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 2020,
    sourceType: 'module',
  },
  plugins: ['react', '@typescript-eslint'],
  rules: {
    '@typescript-eslint/explicit-module-boundary-types': 'off',
  },
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

### cross-env 설치

react를 

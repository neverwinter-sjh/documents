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

eslint의 기본 설정을 init하고 마지막에 npm 설치는 하지 않는다. yarn으로 한꺼번에 설치.

이후에 prettier 관련 플러그인을 설치한다.

```
yarn add -D eslint prettier eslint-config-prettier eslint-plugin-prettier eslint-plugin-jsx-a11y eslint-plugin-import
```

### 플러그인 설명

* eslint : 코드의 문법을 검사하는 린팅과 코드의 스타일을 잡아주는 포맷팅 기능
* prettier : 코드의 스타일을 잡아주는 포맷팅 기능
* @typescript-eslint/eslint-plugin : Typescript 관련 린팅규칙을 설정하는 플러그인
* @typescript-eslint/parser : Typescript 를 파싱하기 위해 사용
* eslint-config-prettier : prettier와 충돌을 일으키는 ESLint 규칙들을 비활성화 시키는 config
* eslint-plugin-prettier : Prettier에서 인식하는 코드상의 포맷 오류를 ESLint 오류로 출력
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
    'plugin:prettier/recommended',
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
    'prettier/prettier': 'error',
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

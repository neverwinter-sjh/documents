# CRA + Typescript

## Stacks

* React(create-react-app)
* Typescript
* Eslint
* Prettier

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
    "ecmaVersion": 2020,
    "sourceType": "module"
  },
  "plugins": ["react", "@typescript-eslint"],
  "rules": {
    "@typescript-eslint/explicit-module-boundary-types": "off"
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

### cross-env 설치

각종 설정 node 환경변수를 window와 mac, iinux에서 같은 방식으로 추가해주기 위해 설치한다.

주요 목적은 react를 실행할 때 browser가 계속 띄워지므로 이를 제거하기 위해서이다.

```
yarn add -D cross-env
```

설치 후에 package.json을 수정한다.

```
"start": "react-scripts start",

=>

"start": "cross-env BROWSER=none react-scripts start",
```

### Sass(Scss) 사용하기

```
yarn add sass sass-loader
```




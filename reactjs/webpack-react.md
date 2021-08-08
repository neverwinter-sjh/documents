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

## babel 설치

```
yarn add --dev @babel/core babel-loader @babel/preset-react @babel/preset-env 
```

### @babel/core
* 리액트는 es6를 사용하므로 여러 브라우저에서 사용가능하도록 es5문법으로 바꿔줌

### @babel/preset-react
* jsx -> javascript

### @babel/preset-env
* es6 -> es5 transpiler

### babel-loader
* 자바스크립트 파일을 babel preset/plugin과 webpack을 사용하여 es5로 컴파일 해주는 plugin
* jsx -> javascript 로 컴파일
* html webpack plugin

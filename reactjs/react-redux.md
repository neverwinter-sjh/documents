# React Redux 설치

React용 Redux를 설치하고 옵션을 설정한다.

## 설치

redux와 react-redux를 설치한다.

```
yarn add redux react-redux
```

typescript를 사용한다면, types를 설치해준다.

```
yarn add -D @types/react-redux
```

## redux counter example 파일 생성

테스트를 위해 예제로 숫자를 더하고 빼는 기능을 가진 counter 파일을 생성한다.

## /src/store/modules/counter.ts

```
// Interface
type CounterAction = ReturnType<typeof increase> | ReturnType<typeof decrease>;

type CounterState = {
  value: number;
};

// Constants
export const INCREASE = 'counter/INCREASE' as const;
export const DECREASE = 'counter/DECREASE' as const;

// Actions
export const increase = () => ({ type: INCREASE });
export const decrease = () => ({ type: DECREASE });

// Initialize
const initialState: CounterState = {
  value: 0,
};

// Reducer
const reducer = (state: CounterState = initialState, action: CounterAction) => {
  switch (action.type) {
    case INCREASE:
      return {
        ...state,
        value: state.value + 1,
      };

    case DECREASE:
      return {
        ...state,
        value: state.value - 1,
      };

    default:
      return { ...state };
  }
};

export default reducer;
```

## /src/store/rootReducer.ts

```
import { combineReducers } from 'redux';
import counter from './modules/counter';

const rootReducer = combineReducers({
  counter,
});

export default rootReducer;
```

## src/store/configureStore.ts

```
import { createStore, applyMiddleware } from 'redux';
import { composeWithDevTools } from 'redux-devtools-extension/developmentOnly';
import rootReducer from './rootReducer';

const composeEnhancers = composeWithDevTools({
  // Specify here name, actionsBlacklist, actionsCreators and other options
});

const store = createStore(rootReducer, composeEnhancers(applyMiddleware()));

export type RootState = ReturnType<typeof store.getState>;

export default store;
```

## /src/index.ts

```
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import { Provider } from 'react-redux';
import store from './store/configureStore';
import reportWebVitals from './reportWebVitals';

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
```

## /src/components/Counter.tsx

```
import React from 'react';

interface Props {
  number: number;
  onIncrease: () => void;
  onDecrease: () => void;
}

const Counter = ({
  number,
  onIncrease,
  onDecrease,
}: Props) => {
  return (
    <div>
      <h1>{number}</h1>
      <button onClick={onIncrease}>+ 1</button>
      <button onClick={onDecrease}>- 1</button>
    </div>
  );
};

export default Counter;
```

## /src/components/CounterContainer.tsx

```
import React, { useCallback } from 'react';
import { RootState } from '../store/configureStore';
import { useSelector, useDispatch } from 'react-redux';
import Counter from './Counter';
import { increase, decrease } from '../store/modules/counter';

const CounterContainer = () => {
  const counterNum = useSelector((state: RootState) => state.counter.value);
  const dispatch = useDispatch();
  const onIncrease = useCallback(() => dispatch(increase()), [dispatch]);
  const onDecrease = useCallback(() => dispatch(decrease()), [dispatch]);

  return <Counter number={counterNum} onIncrease={onIncrease} onDecrease={onDecrease} />;
};

export default CounterContainer;
```

## /src/App.tsx
```
import React from 'react';
import CounterContainer from './components/CounterContainer';

export default function App() {
  return (
    <CounterContainer />
  );
}
```





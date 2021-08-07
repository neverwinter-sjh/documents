# Redux-saga 설치

redux-saga를 설치하고 옵션을 설정한다.

redux 설치까지 끝낸 이후에 계속해서 설정해야 한다.

## 설치

```
yarn add redux-saga
```

## counter example 수정

redux 설정 때 사용했던 counter example을 수정한다.

+1을 할 때는 1초 후에 실행되도록 하고, -1을 실행할 때는 모든 연속 액션이 끝난 후에 마지막 액션만 취하기로 한다.

### /src/store/modules/counter.ts

```
import { delay, put, takeEvery, takeLatest } from 'redux-saga/effects';

// Interface
type CounterAction = ReturnType<typeof increase> | ReturnType<typeof decrease> | ReturnType<typeof increaseAsync> | ReturnType<typeof decreaseAsync>;

type CounterState = {
  value: number;
};

// Constants
export const INCREASE = 'counter/INCREASE' as const;
export const DECREASE = 'counter/DECREASE' as const;
export const INCREASE_ASYNC = 'counter/INCREASE_ASYNC';
export const DECREASE_ASYNC = 'counter/DECREASE_ASYNC';

// Actions
export const increase = () => ({ type: INCREASE });
export const decrease = () => ({ type: DECREASE });
export const increaseAsync = () => ({ type: INCREASE_ASYNC });
export const decreaseAsync = () => ({ type: DECREASE_ASYNC });

// Saga actions
function* increaseSaga() {
  yield delay(1000);
  yield put(increase());
}

function* decreaseSaga() {
  yield delay(1000);
  yield put(decrease());
}

export function* counterSaga() {
  yield takeEvery(INCREASE_ASYNC, increaseSaga);
  yield takeLatest(DECREASE_ASYNC, decreaseSaga);
}

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

### /src/store/rootSaga.ts

```
import { all } from 'redux-saga/effects';
import { counterSaga } from './modules/counter';

export default function* rootSaga() {
  yield all([counterSaga()]);
}
```

### /src/store/configureStore.ts

```
import { createStore, applyMiddleware } from 'redux';
import createSagaMiddleware from 'redux-saga';
import { composeWithDevTools } from 'redux-devtools-extension/developmentOnly';
import rootReducer from './rootReducer';
import rootSaga from './rootSaga';

const composeEnhancers = composeWithDevTools({
  // Specify here name, actionsBlacklist, actionsCreators and other options
});

const sagaMiddleware = createSagaMiddleware();

const middleware = [sagaMiddleware];

const store = createStore(
  rootReducer,
  composeEnhancers(applyMiddleware(...middleware))
);

sagaMiddleware.run(rootSaga);

export type RootState = ReturnType<typeof store.getState>;

export default store;
```

### /src/components/Counter.tsx

```
import React from 'react';

interface Props {
  number: number;
  onIncrease: () => void;
  onDecrease: () => void;
  onIncreaseAsync: () => void;
  onDecreaseAsync: () => void;
}

const Counter = ({
  number,
  onIncrease,
  onDecrease,
  onIncreaseAsync,
  onDecreaseAsync,
}: Props) => {
  return (
    <div>
      <h1>{number}</h1>
      <button onClick={onIncrease}>+ 1</button>
      <button onClick={onDecrease}>- 1</button>
      <hr />
      <button onClick={onIncreaseAsync}>+ 1(Async)</button>
      <button onClick={onDecreaseAsync}>- 1(Async)</button>
    </div>
  );
};

export default Counter;
```

### /src/components/CounterContainer.tsx

```
import React, { useCallback } from 'react';
import { RootState } from '../store/configureStore';
import { useSelector, useDispatch } from 'react-redux';
import Counter from './Counter';
import {
  increase,
  decrease,
  increaseAsync,
  decreaseAsync,
} from '../store/modules/counter';

const CounterContainer = () => {
  const counterNum = useSelector((state: RootState) => state.counter.value);
  const dispatch = useDispatch();
  const onIncrease = useCallback(() => dispatch(increase()), [dispatch]);
  const onDecrease = useCallback(() => dispatch(decrease()), [dispatch]);
  const onIncreaseAsync = useCallback(
    () => dispatch(increaseAsync()),
    [dispatch]
  );
  const onDecreaseAsync = useCallback(
    () => dispatch(decreaseAsync()),
    [dispatch]
  );

  return (
    <Counter
      number={counterNum}
      onIncrease={onIncrease}
      onDecrease={onDecrease}
      onIncreaseAsync={onIncreaseAsync}
      onDecreaseAsync={onDecreaseAsync}
    />
  );
};

export default CounterContainer;
```

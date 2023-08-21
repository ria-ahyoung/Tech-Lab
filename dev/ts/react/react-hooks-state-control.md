# Basic React hooks - `state control`

- React Hooks(v16.8 도입)은 리액트의 강력한 기능 중 하나로 함수형 컴포넌트에 상태를 제어하고 다양한 기능을 추가할 수 있다.

### useState

- 함수형 컴포넌트에 상태를 추가하고, 업데이트를 위해 사용

- 초기값을 인수로 받아 현재 상태 값을 포함하는 배열과 그 상태를 업데이트하는 함수를 반환한다.

state 업데이트 예시 :

```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  const increment= () => {
    setCount(count + 1);
  }
const decrement = () => {
    setCount(count - 1);
  }

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>decrement</button>
    </div>
  );
}
```

### useEffect

- 함수형 컴포넌트에서 생명 주기를 제어하며 컴포넌트에서 Side Effect를 수행한다.

> ex. API에서 데이터를 가져오기, DOM을 업데이트하기, 이벤트를 구독하기 등

```jsx
import React, { useState, useEffect } from 'react';

function DataFetcher() {
  const [data, setData] = useState([]);

  useEffect(() => {
    fetch('https://api.example.com/data')
      .then(response => response.json())
      .then(data => setData(data));
  }, []);

  return (
    <ul>
      {data.map(item => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
}
```

### useContext

> context는 데이터 props를 수동으로 전달하지 않고 컴포넌트 트리를 통해 전달하는 방법

- useContext 훅을 사용하면 함수형 컴포넌트에서 컨텍스트 객체로 접근이 가능하다.

컨텍스트 접근 코드 예시 :

```jsx
// 테마 컨텍스트를 생성하고 useContext 훅을 통해 컨텍스트 접근한다.
import React, { useContext } from 'react';

const ThemeContext = React.createContext('light');

function ThemeButton() {
  const theme = useContext(ThemeContext);

  return (
    <button style={{ background: theme === 'dark' ? 'black' : 'white', color: theme === 'dark' ? 'white' : 'black' }}>
      Toggle Theme
    </button>
  );
}
```

### useReducer
- 함수형 컴포넌트에서 복잡한 상태를 관리할 수 있게 해준다.
- useState 훅과 비슷하지만 간단한 값 대신 `리듀서 함수`와 `초기 상태`를 인자로 받음

쇼핑 카트를 관리하는 코드 예시 :

```jsx
import React, { useReducer } from 'react';

// 상태와 액션을 받아 액션 유형에 따라 새로운 상태를 반환하는 cartReducer 함수 정의
function cartReducer(state, action) {
  switch (action.type) {
    case 'add':
      return [...state, action.payload];
    case 'remove':
      return state.filter(item => item.id !== action.payload.id);
    default:
      return state;
  }
}

function ShoppingCart() {
  // useReducer 훅을 사용하여 cartReducer 함수로 카트 상태를 관리한다.
  const [cart, dispatch] = useReducer(cartReducer, []);

  // 상태 추가 & 제거를 위해 cartReducer에 액션을 디스패치하는 함수를 정의한다.
  const addItem = (item) => {
    dispatch({ type: 'add', payload: item });
  }

  const removeItem = (item) => {
    dispatch({ type: 'remove', payload: item });
  }


  /*
  렌더링된 화면에서 버튼을 클릭하면 addItem 또는 removeItem 함수가 호출되고
  useReducer 훅에서 반환된 디스패치 함수를 사용하여 카트 상태를 업데이트한다.
  */
  return (
    <div>
      <h2>Shopping Cart</h2>
      <ul>
        {cart.map(item => (
          <li key={item.id}>
            {item.name} - ${item.price}
            <button onClick={() => removeItem(item)}>Remove</button>
          </li>
        ))}
      </ul>
      <button onClick={() => addItem({ id: 1, name: 'Item 1', price: 9.99 })}>Add Item</button>
    </div>
  );
}
```

### useRef

- useRef 훅을 사용하면 컴포넌트의 수명 동안 지속되는 가변 ref 객체를 생성할 수 있다.
- ref를 활용해 리렌더링을 트리거하지 않는 값을 저장하고 액세스가 가능하다.

useRef를 사용하여 입력 요소의 값을 액세스하는 예시 :

```jsx
import React, { useRef } from 'react';

function InputWithFocus() {
  const inputRef = useRef();

  const handleClick = () => {
    inputRef.current.focus();
  }

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={handleClick}>Focus Input</button>
    </div>
  );
}
```


### 레퍼런스
- [All React hooks in one short](https://medium.com/@AbidKazmi/all-react-hooks-in-one-short-4b0ed4b5a6e4)

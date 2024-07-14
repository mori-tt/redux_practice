# Redux Practice Project

このプロジェクトは、React と Redux を使用したカウントアップアプリのチュートリアルです。ここでは、Redux Toolkit を使用して状態管理を行う方法について詳しく解説します。

## プロジェクトのセットアップ

### 必要な依存関係のインストール

まず、必要な依存関係をインストールします。

```zsh
npm install @reduxjs/toolkit react-redux
```

### Redux Toolkit の設定

Redux Toolkit を使用して、カウンターの状態管理を設定します。

#### スライスの作成

`src/redux/counter.Slice.ts`ファイルを作成し、以下のコードを追加します。

```typescript:src/redux/counter.Slice.ts
import { createSlice } from "@reduxjs/toolkit";
export const counterSlice = createSlice({
name: "counter",
initialState: {
value: 0,
},
reducers: {
increment: (state) => {
state.value += 1;
},
decrement: (state) => {
state.value -= 1;
},
incrementByAmount: (state, action) => {
state.value += action.payload;
},
},
});
export const { increment, decrement, incrementByAmount } = counterSlice.actions;
export default counterSlice.reducer;
```

このスライスは、カウンターの状態と 3 つのアクション（increment, decrement, incrementByAmount）を定義しています。

- `createSlice`は、スライスの名前、初期状態、およびリデューサー関数を受け取ります。
- `increment`、`decrement`、`incrementByAmount`は、それぞれカウンターの値を増減させるアクションです。

#### ストアの設定

次に、Redux ストアを設定します。`src/redux/store.ts`ファイルを作成し、以下のコードを追加します。

```typescript:src/redux/store.ts
import { configureStore } from "@reduxjs/toolkit";
import counterReducer from "./counter.Slice";
export const store = configureStore({
reducer: { counter: counterReducer },
});
```

このストアは、カウンタースライスのリデューサーを使用して構成されています。

- `configureStore`は、Redux ストアを設定するための関数です。
- `reducer`オプションにカウンタースライスのリデューサーを渡します。

### React コンポーネントでの使用

React コンポーネントで Redux ストアを使用するために、`src/main.tsx`ファイルを以下のように設定します。

```typescript:src/main.tsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.tsx";
import "./index.css";
import { Provider } from "react-redux";
import { store } from "./redux/store";
ReactDOM.createRoot(document.getElementById("root")!).render(
<React.StrictMode>
<Provider store={store}>
<App />
</Provider>
</React.StrictMode>
);
```

これにより、アプリ全体で Redux ストアが利用可能になります。

- `Provider`コンポーネントでアプリ全体をラップし、`store`を渡します。

### カウンターコンポーネントの作成

`src/App.tsx`ファイルにカウンターコンポーネントを作成し、以下のコードを追加します。

```typescript:src/App.tsx
import { useDispatch, useSelector } from "react-redux";
import "./App.css";
import { decrement, increment, incrementByAmount } from "./redux/counter.Slice";
import { useState } from "react";
function App() {
const count = useSelector((state: any) => state.counter.value);
const dispatch = useDispatch();
const [incrementAmount, setIncrementAmount] = useState("2");
return (
<div className="App">
<h1>count: {count}</h1>
<input
onChange={(e) => setIncrementAmount(e.target.value)}
value={incrementAmount}
/>
<button onClick={() => dispatch(increment())}>+</button>
<button onClick={() => dispatch(decrement())}>-</button>
<button
onClick={() => dispatch(incrementByAmount(Number(incrementAmount)))}
>
追加
</button>
</div>
);
}
export default App;
```

このコンポーネントでは、`useSelector`を使用してカウンターの値を取得し、`useDispatch`を使用してアクションをディスパッチしています。

- `useSelector`は、Redux ストアの状態を取得するためのフックです。
- `useDispatch`は、アクションをディスパッチするためのフックです。

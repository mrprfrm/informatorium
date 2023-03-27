## Создание React.js проекта

Клонируем репозиторий из вне (github, bitbucket, gitlab и т.д.).

```zsh
git clone git@github.com:<user_name>/<repository_name>.git
```

Запускаем `create-react-app` в корневой директории репозитория.

```zsh
create-react-app .
```

> В случае если планируется создать проект исключительно для локального использования можно воспользоваться `creaete-react-app <project_name>` и сразу указать имя будущего проекта. 

## Добавление CSS-фреймвёрка (Tailwindcss)

Устанавливаем `tailwindccs` для работы с react приложением и инициализируем его внутри проекта.

```zsh
npm install -D tailwindcss
npx tailwindcss init
```

Обновляем конфигурацию `tailwindcss` (`tailwind.config.js`) для последующей оптимальной сборки на основе шаблонов в проекте. 

```JavaScript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./public/**/*.html", "./src/**/*.{js,jsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};

```

> В данном случае добавляем и маску для `js|jsx` файлов в директории с исходным кодом, и для `html` файлов в директории `public/`, чтобы в `css` сборке иметь стили для всех файлов проекта.

Добавляем директивы `tailwindcss` в рутовый `index.css`.

```css
@tailwind base; 
@tailwind components; 
@tailwind utilities;
```

Вносим изменения в разметку `public/index.html` для корректного отображения контента на странице.

```HTML
<!-- ./public/index.html -->

<html>
  <head> ... </head>
  <body>
    ...
    <div id="root" class="flex flex-col flex-1 h-screen overflow-hidden"></div>
    ...
  </body>
<html>
```

## Добавление роутинга в проект

Устанавливаем `react-router` для обработки адресной строки.

```zsh
npm install react-router-dom
```

Устанавливаем `redux` для работы с реактивным хранилищем.

Cоздаём базовое представление `./views/Home.js`

```JavaScript
// ./views/Home.js 

export function Home() {
  return <div>Hello world</div>;
}
```

Добавляем `BrowserRouter` в проект

```JavaScript
// ./index.js

import React from "react";
import ReactDOM from "react-dom/client";
import { BrowserRouter } from "react-router-dom";

import "./index.css";
import App from "./App";
import reportWebVitals from "./reportWebVitals";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
);
```

Добавляем список роутов и по умолчанию используем ранее созданное представление `Home`

```JavaScript
// ./App.js

import { Routes, Route } from "react-router-dom";

import "./App.css";
import { Home } from "./views/Home";

function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
    </Routes>
  );
}

export default App;
```

## Создание реактивного хранилища данных

Устанавливаем `redux` для работы с хранилищем в компонентах.

```zsh
npm install react-redux 
```

Устанавливаем инструменты `redux` для описания хранилища.

```zsh
npm intall @redux/toolkit
```

Создаём слой хранилища для работы с базовым представлением

```JavaScript
// ./features/homeSlice.js

import { createSlice } from "@reduxjs/toolkit";

export const homeSlice = createSlice({
  name: "home",

  initialState: {
    text: "Hello world!",
  },

  reducers: {
    setText: (state, action) => {
      state.text = action.payload;
    },
  },
});

export const { setText } = homeSlice.actions;
export default homeSlice.reducer;
```

Создаём хранилище

```JavaScript
// ./features/index.js

import { configureStore } from "@reduxjs/toolkit";
import homeReducer from "./homeSlice";

export default configureStore({
  reducer: {
    home: homeReducer,
  },
});
```

Добавляем провайдер в проект

```JavaScript
// ./index.js

import React from "react";
import ReactDOM from "react-dom/client";
import { BrowserRouter } from "react-router-dom";
import { Provider } from "react-redux";

import "./index.css";
import App from "./App";
import reportWebVitals from "./reportWebVitals";
import store from "./features";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <BrowserRouter>
      <Provider store={store}>
        <App />
      </Provider>
    </BrowserRouter>
  </React.StrictMode>
);
```

Интегрируем хранилище в базовое представление

```JavaScript
// ./views/Home.js

import { useSelector } from "react-redux";

export function Home() {
  const { text } = useSelector((store) => store.home);

  return <div>{text}</div>;
}
```

## Список источников

- [React router examples](https://github.com/remix-run/react-router/tree/dev/examples)
- [React router Tutorial](https://reactrouter.com/en/main/start/tutorial)
- [Redux Getting Started](https://redux.js.org/introduction/getting-started)
- [Install Tailwind CSS with Create React App](https://tailwindcss.com/docs/guides/create-react-app)

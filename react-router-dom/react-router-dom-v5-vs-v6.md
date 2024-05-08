# React Router Dom: переход с V5 на V6
## V5 vs V6
В V6 было много внутренних изменений, будь то улучшенный алгоритм сопоставления шаблонов путей или добавление новых компонентов. Размер пакета был уменьшен почти на 58%.

### `Switch` заменён на `Routes`
Основное отличие в том, что Routes не требует жесткого порядка роутов внутри.
```
// V5
import { BrowserRouter, Route, Switch } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/about" component={About} />
        <Route path="/contact" component={Contact} />
      </Switch>
    </BrowserRouter>
  );
}
```

```
// V6
import { BrowserRouter, Routes, Route } from 'react-router-dom'; 

function App() { 
  return ( 
    <BrowserRouter> 
      <Routes> 
        <Route path="/" element={<Home />} /> 
        <Route path="/about" element={<About />} /> 
        <Route path="/contact" element={<Contact />} /> 
      </Routes> 
    </BrowserRouter> 
  ); 
}
```

### `element` в место `children` 
Отличия в том, что вместо функции в пропе render или children используется element.
```
// V5
<Route path={urls.courses} render={props => <CoursesList {...props} otherProp={myProp} />} />
```

```
// V6
<Route path={urls.courses} element={<CoursesList otherProp={myProp} />} />
```
### Вложенные маршруты
 Больше не нужно матчить маршруты каждый раз, а можно просто задавать относительные пути. Дочерние компоненты смотрят на родительские. Если у родительских есть какой-то путь, то все дочерние будут отображаться в адресной строке относительно предыдущего.
 ```
function Users() {
  return (
    <div>
        <nav>
            <Link to="me">My profile</Link>
        </nav>

        <Routes>
            <Route path=":id" element={<UserProfile />} />
            <Route path="me" element={<OwnUserProfile />} />
        </Routes>
    </div>
  );
}
 ```


### Проп `exact` больше не нужн
Раньше, без exact, загружался любой URL-адрес, начинающийся с соответствующего ключевого слова, поскольку процесс сопоставления выполнялся сверху вниз по определениям маршрутов. Но теперь нам не нужно об этом беспокоиться, поскольку React Router имеет лучший алгоритм загрузки наилучшего маршрута для определенного URL-адреса, порядок определения сейчас не имеет особого значения.

Дочерние компоненты смотрят на родительские. Если у родительских есть какой-то путь, то все дочерние будут отображаться в адресной строке относительно предыдущего. 

### `useHistory` заменён на `useNavigate`
```
//V5
import { useHistory } from "react-router-dom";

function News() {
  let history = useHistory();

  return (
    <div>
      <button onClick={()=>{
           history.push("/home");
      }}>Home</button>
    </div>
  );
}
```
```
//V6
import { useNavigate } from "react-router-dom";

function News() {
  let navigate = useNavigate();

  return (
    <div>
      <button onClick={()=>{
          navigate("/home");
      }}>go home</button>
    </div>
  );
}
```
Еще некоторыми общими чертами `useHistory` были `go`, `goBack` и `goForward`. Этого также можно достичь с помощью API навигации, нам просто нужно указать количество шагов, на которые мы хотим двигаться вперед или назад («+» для вперед и «-» для назад).
```
//V6
import { useNavigate } from "react-router-dom";

function Exchanges() {
  const navigate = useNavigate();

  return (
    <>
      <button onClick={() => navigate(-2)}>
        2 steps back
      </button>
      <button onClick={() => navigate(-1)}>1 step back</button>
      <button onClick={() => navigate(1)}>
        1 step forward
      </button>
      <button onClick={() => navigate(2)}>
        2 steps forward
      </button>
    </>
  );
}
```

### `activeStyle` и `activeClassName` удалены из `<NavLink />`
В предыдущей версии мы могли установить отдельный класс или объект стиля на время, когда он `<NavLink/>` будет активен. В V6 эти два реквизита удалены, вместо этого в случае с реквизитами `className` и `style` `<NavLink/>` они принимают функцию, которая, в свою очередь, выдает некоторую информацию о ссылке, чтобы мы могли лучше контролировать стили.
```
//V5
<NavLink
  to="/news"
  style={{ color: 'black' }}
  activeStyle={{ color: 'blue' }}>
  Exchanges
</NavLink>

<NavLink
  to="/news"
  className="nav-link"
  activeClassName="active">
  Exchanges
</NavLink>
```
```
//V6
<NavLink
  to="/news"
  style={({isActive}) => { color: isActive ? 'blue' : 'black' }}>
  Exchanges
</NavLink>

<NavLink
  to="/news"
  className={({ isActive }) => "nav-link" + (isActive ? "active" : "")}>
  Exchanges
</NavLink>
```
### `Redirect` заменён на `​Navigate`
`Redirect` больше не экспортируется из `react-router-dom`, вместо этого мы используем `Navigate` для достижения тех же функций.
```
//V5
import { Redirect } from "react-router-dom";

<Route exact path="/latest-news">
    <Redirect to="/news">
</Route>
<Route exact path="/news">
    <News />
</Route>

```
```
//V6
import { Navigate } from "react-router-dom";

<Route path="/latest-news" element={<Navigate replace to="/news">} />
<Route path="/news" element={<Home />} />
```

## Что нового? 
В V6 много нового, но только некоторые моменты рассмотрим.
### Компоненты
#### `<Outlet />`
Следует использовать в родительских компонентах маршрута для визуализации дочерних компонентов. Это позволяет отображать вложенный пользовательский интерфейс при отрисовке дочерних маршрутов. Если родительский маршрут точно совпал, он отобразит дочерний индексный маршрут или ничего, если индексный маршрут отсутствует.
```
function Dashboard() {
  return (
    <div>
      <h1>Dashboard</h1>

      {/* This element will render either <DashboardMessages> when the URL is
          "/messages", <DashboardTasks> at "/tasks", or null if it is "/"
      */}
      <Outlet />
    </div>
  );
}

function App() {
  return (
    <Routes>
      <Route path="/" element={<Dashboard />}>
        <Route
          path="messages"
          element={<DashboardMessages />}
        />
        <Route path="tasks" element={<DashboardTasks />} />
      </Route>
    </Routes>
  );
}
```

#### `<ScrollRestoration />`
React Router эмулирует восстановление прокрутки браузера при навигации, ожидая загрузки данных перед прокруткой. Это гарантирует, что позиция прокрутки будет восстановлена в нужном месте.

Вы также можете настроить поведение, восстанавливая прокрутку не по местоположению (например, по пути к url) и запрещая прокрутку по определенным ссылкам (например, по вкладкам в середине страницы)
```
<ScrollRestoration
  getKey={(location, matches) => {
    return location.pathname;
  }}
/>
```
[Подробнее](https://reactrouter.com/en/main/components/scroll-restoration#scrollrestoration-)

### Хуки
В разработке...

**Полезные ссылки:**
- [Upgrading from v5](https://reactrouter.com/en/main/upgrading/v5)

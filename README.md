# react-blog-app

## My React Blog

1. [Navigation Menu](#Navigation-Menu)
2. [Responsive Navigation Menu](#Responsive-Navigation-Menu)

---

## Navigation Menu

- `yarn start`
- `yarn add node-sass@4.14.1 sass-loader`
  - `yarn add <package>`, `yarn remove <package>`
- `yarn add react-router react-router-dom`

```js
// App.js
import React from 'react'
import Navigation from './components/navigation'
import {
  BrowserRouter as Router,
  Switch,
  Route,
  Redirect,
} from 'react-router-dom'
import PageRenderer from './page-renderer'

function App() {
  return (
    <Router>
      <div className="app">
        <Navigation />
        <Switch>
          <Route path="/:page" component={PageRenderer} />
          <Route path="/" render={() => <Redirect to="/home" />} />
          <Route component={() => 404} />
        </Switch>
      </div>
    </Router>
  )
}

export default App

// components/navigation.js
import React from 'react'
import { Link } from 'react-router-dom'

const navLinks = [
  {
    title: 'Home',
    path: '/',
  },
  {
    title: 'Blog',
    path: '/blog',
  },
  {
    title: 'Contact Us',
    path: '/contact-us',
  },
  {
    title: 'Login',
    path: '/login',
  },
]

export default function Navigation() {
  return (
    <nav className="site-navigation">
      <span>Mini Blog</span>
      <ul>
        {navLinks.map((link, index) => (
          <li key={index}>
            <Link to={link.path}>{link.title}</Link>
          </li>
        ))}
      </ul>
    </nav>
  )
}

// index.js
import './assets/scss/base.scss'

// scss/base.scss
@import './_variables';
@import './_navigation';

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
    Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  font-weight: 500;
  margin: 0;
  background-color: #f8f9fa;
}

// scss/_variables.scss
$text-link: #2997ff;
$text-color: #444;

// scss/_navigation_.scss
.site-navigation {
  display: flex;
  justify-content: space-between;
  width: 100%;
  height: 65px;
  background: white;
  color: $text-color;

  span {
    display: flex;
    align-items: center;
    margin-left: 30px;
    font-size: 24px;
    font-weight: 400;
  }

  ul {
    display: flex;
    align-items: center;
    height: 100%;
    margin-block-start: 0;
    margin-block-end: 0;
    padding-inline-start: 0;
    margin-right: 20px;
    list-style-type: none;

    a {
      text-decoration: none;
      padding: 0 20px 0 20px;
      font-size: 20px;
      color: $text-color;

      &:hover {
        color: $text-link;
      }
    }
  }
}

// page-renderer.js
import React from 'react'
import { useRouteMatch } from 'react-router-dom'

const generatePage = page => {

  const component = () => require(`./pages/${page}`).default

  try {
    return React.createElement(component())

  } catch (err) {
    console.warn(err)
    return React.createElement(() => 404)
  }
}

export default function PageRenderer() {

  const {
    params: { page }
  } = useRouteMatch()

  return generatePage(page)
}

// pages/home,login,contact-us,blog.js
import React from 'react'

export default function Home () {
  return <div>Home</div>
}

```

<br>[Top](#My-React-Blog)

---

## Responsive Navigation Menu

- **CSS 프레임워크 antd의 아이콘과 폰트**
  - `yarn add antd @quasar/extras`
- [ionicons-v4 참고](https://ionicons.com/)

- `&.active` : `.menu-content-container.active`

```js
// index.js
import 'antd/dist/antd.css'
import '@quasar/extras/ionicons-v4/ionicons-v4.css'

// App.js
function App() {

  const user = {
    firstName: 'Sungmin',
    lastName: 'Park'
  }

  return (
    ...
  )
}

// components/navigation.js
import { Avatar } from 'antd'

export default function Navigation ({ user }) {

    const [menuActive, setMenuActive] = useState(false)

    return (
        <nav className="site-navigation">
            <span className="menu-title">My React Blog</span>
            <div className={`menu-content-container ${menuActive && `active`}`}>
                <ul>
                { navLinks.map((link, index) => (
                    <li key={index}>
                        <Link to={link.path}>{link.title}</Link>
                    </li>
                    ))
                }
                </ul>
                <span className="menu-avatar-container">
                    <Avatar src="https://zos.alipayobjects.com/rmsportal/ODTLcjxAfvqbxHnVXCYX.png"
                        size={38} />
                    <span className="menu-avatar-name">
                        {`${user.firstName} ${user.lastName}`}
                    </span>
                </span>
            </div>
            <i className="ionicons icon ion-ios-menu"
                onClick={() => setMenuActive(!menuActive)} />
        </nav>
    )
}

// scss/base.scss
html body {
  ...
  background-color: $background;
}

// scss/_variables.scss
$background: #f8f9fa;
$border: #ccc;

// scss/_navigation.scss
.site-navigation {
  display: flex;
  justify-content: space-between;
  width: 100%;
  height: 65px;
  background: white;
  color: $text-color;

  .menu-title {
    display: flex;
    align-items: center;
    margin-left: 30px;
    font-size: 24px;
    font-weight: 400;
  }

  .ion-ios-menu {
    font-size: 36px;
    width: 65px;
    align-items: center;
    cursor: pointer;
    display: none;
  }

  .menu-content-container {
    display: flex;
    align-items: center;
    padding-right: 30px;

    .ant-avatar {
      background: $background;
      border: 1px solid $border;
    }

    .menu-avatar-name {
      font-size: 18px;
      margin-left: 15px;
    }
  }

  ul {
    display: flex;
    align-items: center;
    height: 100%;
    margin-block-start: 0;
    margin-block-end: 0;
    padding-inline-start: 0;
    margin-right: 20px;
    list-style-type: none;

    a {
      text-decoration: none;
      padding: 0 20px 0 20px;
      font-size: 20px;
      color: $text-color;

      &:hover {
        color: $text-link;
      }
    }
  }
}

@media screen and (max-width: 900px) {
  .site-navigation {
    .menu-content-container {
      width: 300px;
      height: calc(100% - 65px);
      background: white;
      position: absolute;
      top: 65px;
      left: -300px;
      transition: 300ms ease left;
      padding: 0;
      padding-left: 30px;

      a {
        width: 100%;
        padding: 0;
      }

      li {
        height: 65px;
        display: flex;
        align-items: center;
      }

      ul {
        flex-direction: column;
        justify-content: flex-start;
        padding: 0;
        padding-top: 20px;
      }

      ul,
      li,
      .menu-avatar-container {
        width: 100%;
      }

      .menu-avatar-container {
        padding-top: 20px;
      }

      &.active {
        left: 0px;
        flex-direction: column-reverse;
      }
    }

    .ion-ios-menu {
      display: flex;
    }
  }
}
```

<br>[Top](#My-React-Blog)

---

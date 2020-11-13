# react-blog-app

## My React Blog

1. [Navigation Menu](#Navigation-Menu)

---

## Navigation Menu

`yarn start`
`yarn add node-sass@4.14.1 sass-loader`
cf) yarn add / yarn remove
`yarn add react-router react-router-dom`

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

// assets/scss/base.scss
@import './_variables';
@import './_navigation';

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
    Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  font-weight: 500;
  margin: 0;
  background-color: #f8f9fa;
}

// assets/scss/_variables.scss
$text-link: #2997ff;
$text-color: #444;

// assets/scss/_navigation_.scss
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
```

<br>[Top](#My-React-Blog)

---

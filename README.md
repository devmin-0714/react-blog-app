# react-blog-app

## My React Blog

1. [Navigation Menu](#Navigation-Menu)
2. [Responsive Navigation Menu](#Responsive-Navigation-Menu)
3. [CSS Grids and Flex Box](#CSS-Grids-and-Flex-Box)

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

## CSS Grids and Flex Box

- **날짜 라이브러리**
  - `yarn add moment`

```js
// mocks/featured, trending.js
import moment from 'moment'

export default [
  {
    title: '[featured]Can anyone code?',
    date: moment().format('MMMM DD, YYYY'),
    categories: ['Tech Culture', 'Tech News'],
    link: '#',
    image: 'anyone_can_code.jpg',
  },
  {
    title: '[featured]Using AWS S3 for Storing Blog Images',
    date: moment().format('MMMM DD, YYYY'),
    categories: ['Cloud'],
    link: '#',
    image: 'cloud_storage.jpg',
  },
  {
    title: '[featured]Popular Programming Languages in 2020',
    date: moment().format('MMMM DD, YYYY'),
    categories: ['Tech Culture', 'Tech News'],
    link: '#',
    image: 'programming_languages.jpg',
  },
  {
    title: '[featured]Brain Makes for Learning to Program',
    date: moment().format('MMMM DD, YYYY'),
    categories: ['Brain Health'],
    link: '#',
    image: 'neuron.jpg',
  },
]

// pages/home.js
import React from 'react'
import { PostMasonry, MasonryPost } from '../components/common'
import trending from '../assets/mocks/trending'
import featured from '../assets/mocks/featured'

const trendingConfig = {
  1: {
    gridArea: '1 / 2 / 3 / 3',
  },
}

const featuredConfig = {
  0: {
    gridArea: '1 / 1 / 2 / 3',
    height: '300px',
  },
  1: {
    height: '300px',
  },
  3: {
    height: '630px',
    marginLeft: '30px',
    width: '630px',
  },
}

const mergeStyles = function (posts, config) {
  posts.forEach((post, index) => {
    post.style = config[index]
  })
}

mergeStyles(trending, trendingConfig)
mergeStyles(featured, featuredConfig)

const lastFeatured = featured.pop()

export default function Home() {
  return (
    <section className="container home">
      <div className="row">
        <h1>Featured Posts</h1>
        <section className="featured-posts-container">
          <PostMasonry posts={featured} columns={2} tagsOnTop={true} />
          <MasonryPost post={lastFeatured} tagsOnTop={true} />
        </section>
        <h1>Trending Posts</h1>
        <PostMasonry posts={trending} columns={3} />
      </div>
    </section>
  )
}

// common/index.js
import MasonryPost from './masonry-post'
import PostMasonry from './post-masonry'

export { MasonryPost, PostMasonry }

// common/post-masonry.js
import React from 'react'
import { MasonryPost } from './'

export default function PostMasonry({ posts, columns, tagsOnTop }) {
  return (
    <section
      className="masonry"
      style={{ gridTemplateColumns: `repeat(${columns}, minmax(275px, 1fr))` }}
    >
      {posts.map((post, index) => (
        <MasonryPost {...{ post, index, tagsOnTop, key: index }} />
      ))}
    </section>
  )
}

// common/masonry-post.js
import React from 'react'
import { categoryColors } from './styles'

export default function MasonryPost({ post, tagsOnTop }) {
  const windowWidth = window.innerWidth
  const imageBackground = {
    backgroundImage: `url("${require(`../../assets/images/${post.image}`)}")`,
  }
  const style =
    windowWidth > 900 ? { ...imageBackground, ...post.style } : imageBackground

  return (
    <a className="masonry-post overlay" style={style} href={post.link}>
      <div
        className="image-text"
        style={{ justifyContent: tagsOnTop ? 'space-between' : 'flex-end' }}
      >
        <div className="tags-container">
          {post.categories.map((tag, ind) => (
            <span
              className="tag"
              key={ind}
              style={{ backgroundColor: categoryColors[tag] }}
            >
              {tag.toUpperCase()}
            </span>
          ))}
        </div>
        <div>
          <h2 className="image-title">{post.title}</h2>
          <span className="image-date">{post.date}</span>
        </div>
      </div>
    </a>
  )
}

// common/styles.js
const categoryColors = {
  'Tech Culture': 'rgb(255,59,48)',
  'Tech News': 'rgb(0,113,164)',
  Cloud: 'rgb(255,45,85)',
  Vue: 'rgb(52,199,89)',
  React: 'rgb(64,156,255)',
  JavaScript: 'rgb(255,179,64)',
  Cloud: 'rgb(175,82,250)',
}

export { categoryColors }
```

```css
// scss/base.scss
@import './home';
@import './masonry-post';
@import './post-masonry';
h1 {
  font-size: 2rem;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
}

.row {
  margin: 4em 20px;
}

.overlazy::before {
  z-index: 1;
  content: '';
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background: linear-gradient(
    180deg,
    trasparent 0%,
    trasparent 18%,
    rgba(0, 0, 0, 0.8) 99%,
    rgba(0, 0, 0, 0.8) 100%
  );
}

// scss/_variables.scss
$text-light: #999;

// scss/_home.scss
.home {
  h1 {
    width: '100%';
    color: $text-color;
  }

  .featured-posts-container {
    display: flex;
  }
}

@media screen and (max-width: 900px) {
  .home {
    h1 {
      margin-block-start: 1.5em;
      margin-block-end: 1.5em;
    }

    .featured-posts-container {
      flex-direction: column;
    }
  }
}

// scss/_post-masonry.scss
.masonry {
  display: grid;
  grid-gap: 30px;
}

@media screen and (max-width: 900px) {
  .masonry {
    display: flex;
    flex-direction: column;
  }
}

// scss/_masonry-post.scss
.masonry-post {
  position: relative;
  border-radius: 5px;
  overflow: hidden;
  background-size: cover;
  background-position: center center;
  height: 100%;
  width: 100%;
  text-decoration: none;
  margin: 0 auto;

  .image-text {
    height: 100%;
    width: 100%;
    display: flex;
    flex-direction: column;
    padding: 20px;
    box-sizing: border-box;
  }

  .image-title,
  .image-date,
  .tags-container,
  div {
    float: left;
    width: 100%;
    z-index: 10;
    font-weight: 300;
  }

  .image-title {
    font-size: 20px;
    margin-block-start: 0;
    margin-block-end: 10px;
    color: white;
  }

  .image-date {
    font-size: 18px;
    color: $text-light;
  }

  .tags-container {
    font-size: 12px;
    font-weight: 500;
    margin-bottom: 15px;
    margin-left: -1px;
    color: white;

    .tag {
      border-radius: 5px;
      padding: 4px 8px;
      margin-right: 5px;
    }
  }
}

@media screen and (max-width: 900px) {
  .masonry-post-image {
    width: 100%;
    margin: 10px 0;
    height: 300px;
  }
}
```

<br>[Top](#My-React-Blog)

---

 npm install --save  react-router-dom@5.2.0

 npm install react-loader-spinner@4.0.0 --save
------------------------------------------------------------------------
 # Navegating to specific blog

 * steps :

 * Add link for Blogitem
 * Add BlogitemDetails Route
 * Make API call for Blog Specific Details
 -----------------------------------------------------------------------

 # API Calls:

 In General we make API Calls inside componentDidMount(). So that it doesn’t block render().

# Fetch
fetch is a promise-based API which returns a response object. In the backend, we use snake_case for naming conventions.

# Route Props
When a component is rendered by the Route, some additional props are passed

* match
* location
* history

# Match
The match object contains the information about the path from which the component is rendered.

## Publish

https://SaiDevBlog.ccbp.tech

------------------------------------------------------------------------

## Rendering Promise Object

# Mistake:

Async Function returns a Promise Object But, React cannot render this Promise Object.
The best way to make API call is in the componentDidMount().

## File: src/components/BlogsList/index.js

import { Component } from 'react'
import Loader from 'react-loader-spinner'

import 'react-loader-spinner/dist/loader/css/react-spinner-loader.css'

import BlogItem from '../BlogItem'

import './index.css'

class BlogsList extends Component {
  state = { isLoading: true, blogsData: [] }

  getBlogsData = async () => {
    const response = await fetch('https://apis.ccbp.in/blogs')
    const statusCode = await response.statusCode
    console.log(statusCode)
    const data = await response.json()

    const formattedData = data.map(eachItem => ({
      id: eachItem.id,
      title: eachItem.title,
      imageUrl: eachItem.image_url,
      avatarUrl: eachItem.avatar_url,
      author: eachItem.author,
      topic: eachItem.topic,
    }))
  }

  render() {
    const { blogsData, isLoading } = this.state
    console.log(isLoading)

    return (
      <div className="blog-list-container">
       {this.getBlogsData()}
      </div>
    )
  }
}

export default BlogsList

## Solution:

# 
File: src/components/BlogsList/index.js

import { Component } from 'react'
import Loader from 'react-loader-spinner'

import 'react-loader-spinner/dist/loader/css/react-spinner-loader.css'

import BlogItem from '../BlogItem'

import './index.css'

class BlogsList extends Component {
  state = { isLoading: true, blogsData: [] }

  componentDidMount() {
    this.getBlogsData()
  }

  getBlogsData = async () => {
    const response = await fetch('https://apis.ccbp.in/blogs')
    const statusCode = await response.statusCode
    console.log(statusCode)
    const data = await response.json()

    const formattedData = data.map(eachItem => ({
      id: eachItem.id,
      title: eachItem.title,
      imageUrl: eachItem.image_url,
      avatarUrl: eachItem.avatar_url,
      author: eachItem.author,
      topic: eachItem.topic,
    }))
    this.setState({ blogsData: formattedData, isLoading: false })
  }

  render() {
    const { blogsData, isLoading } = this.state
    console.log(isLoading)

    return (
      <div className="blog-list-container">
        {isLoading ? (
          <Loader type="TailSpin" color="#00BFFF" height={50} width={50} />
        ) : (
          blogsData.map(item => <BlogItem blogData={item} key={item.id} />)
        )}
      </div>
    )
  }
}

export default BlogsList
------------------------------------------------------------------------

## 2. Using Link and Route Components without BrowserRouter

# Mistake:

# File: src/App.js

import {Route, Switch } from 'react-router-dom'

import Header from './components/Header'
import About from './components/About'
import Contact from './components/Contact'
import BlogsList from './components/BlogsList'
import BlogItemDetails from './components/BlogItemDetails'
import NotFound from './components/NotFound'

import './App.css'

const App = () => (
    <Header />
    <Switch>
      <Route exact path="/" component={BlogsList} />
      <Route path="/about" component={About} />
      <Route path="/contact" component={Contact} />
      <Route path="/blogs/:id" component={BlogItemDetails} />
      <Route component={NotFound} />
    </Switch>
)

export default App

## Solution:

# File: src/App.js

import { BrowserRouter, Route, Switch } from 'react-router-dom'

import Header from './components/Header'
import About from './components/About'
import Contact from './components/Contact'
import BlogsList from './components/BlogsList'
import BlogItemDetails from './components/BlogItemDetails'
import NotFound from './components/NotFound'

import './App.css'

const App = () => (
  <BrowserRouter>
    <Header />
    <Switch>
      <Route exact path="/" component={BlogsList} />
      <Route path="/about" component={About} />
      <Route path="/contact" component={Contact} />
      <Route path="/blogs/:id" component={BlogItemDetails} />
      <Route component={NotFound} />
    </Switch>
  </BrowserRouter>
)

export default App


## File: src/components/Header/index.js

import { Link } from 'react-router-dom'

import './index.css'

const Header = () => (
  <nav className="nav-header">
    <div className="blog-container">
      <h1 className="blog-title">Dev Blog</h1>
      <ul className="nav-menu">
        <Link className="nav-link" to="/">
          <li>Home</li>
        </Link>
        <Link className="nav-link" to="/about">
          <li>About</li>
        </Link>
        <Link className="nav-link" to="/contact">
          <li>Contact</li>
        </Link>
      </ul>
    </div>
  </nav>
)

export default Header
------------------------------------------------------------------------

## 3. Providing Wrong Route Path

## Mistake:

# File: src/App.js

import { BrowserRouter, Route, Switch } from 'react-router-dom'

import Header from './components/Header'
import About from './components/About'
import Contact from './components/Contact'
import BlogsList from './components/BlogsList'
import BlogItemDetails from './components/BlogItemDetails'
import NotFound from './components/NotFound'

import './App.css'

const App = () => (
  <BrowserRouter>
    <Header />
    <Switch>
      <Route exact path="/" component={BlogsList} />
      <Route path="/abot" component={About} />
      <Route path="/contact" component={Contact} />
      <Route path="/blogs/:id" component={BlogItemDetails} />
      <Route component={NotFound} />
    </Switch>
  </BrowserRouter>
)

export default App

## Solution:

# File: src/App.js

import { BrowserRouter, Route, Switch } from 'react-router-dom'

import Header from './components/Header'
import About from './components/About'
import Contact from './components/Contact'
import BlogsList from './components/BlogsList'
import BlogItemDetails from './components/BlogItemDetails'
import NotFound from './components/NotFound'

import './App.css'

const App = () => (
  <BrowserRouter>
    <Header />
    <Switch>
      <Route exact path="/" component={BlogsList} />
      <Route path="/about" component={About} />
      <Route path="/contact" component={Contact} />
      <Route path="/blogs/:id" component={BlogItemDetails} />
      <Route component={NotFound} />
    </Switch>
  </BrowserRouter>
)

export default App

## File: src/components/Header/index.js

import { Link } from 'react-router-dom'

import './index.css'

const Header = () => (
  <nav className="nav-header">
    <div className="blog-container">
      <h1 className="blog-title">Dev Blog</h1>
      <ul className="nav-menu">
        <Link className="nav-link" to="/">
          <li>Home</li>
        </Link>
        <Link className="nav-link" to="/about">
          <li>About</li>
        </Link>
        <Link className="nav-link" to="/contact">
          <li>Contact</li>
        </Link>
      </ul>
    </div>
  </nav>
)

export default Header
------------------------------------------------------------------------
## 4. Missing exact keyword
# Mistake:

Switch renders a Route that matches the path.
If Switch is missed, Browser will render Multiple Routes in the single path.

# File: src/App.js

import { BrowserRouter, Route, Switch } from 'react-router-dom'

import Header from './components/Header'
import About from './components/About'
import Contact from './components/Contact'
import BlogsList from './components/BlogsList'
import BlogItemDetails from './components/BlogItemDetails'
import NotFound from './components/NotFound'

import './App.css'

const App = () => (
  <BrowserRouter>
    <Header />
    <Switch>
      <Route  path="/" component={BlogsList} />
      <Route path="/about" component={About} />
      <Route path="/contact" component={Contact} />
      <Route path="/blogs/:id" component={BlogItemDetails} />
      <Route component={NotFound} />
    </Switch>
  </BrowserRouter>
)

export default App

## Solution:

# File: src/App.js

import { BrowserRouter, Route, Switch } from 'react-router-dom'

import Header from './components/Header'
import About from './components/About'
import Contact from './components/Contact'
import BlogsList from './components/BlogsList'
import BlogItemDetails from './components/BlogItemDetails'
import NotFound from './components/NotFound'

import './App.css'

const App = () => (
  <BrowserRouter>
    <Header />
    <Switch>
      <Route exact path="/" component={BlogsList} />
      <Route exact path="/about" component={About} />
      <Route exact path="/contact" component={Contact} />
      <Route exact path="/blogs/:id" component={BlogItemDetails} />
      <Route component={NotFound} />
    </Switch>
  </BrowserRouter>
)

export default App
------------------------------------------------------------------------

## 5. Missing Switch
# Mistake:

# File: src/App.js

import { BrowserRouter, Route} from 'react-router-dom'

import Header from './components/Header'
import About from './components/About'
import Contact from './components/Contact'
import BlogsList from './components/BlogsList'
import BlogItemDetails from './components/BlogItemDetails'
import NotFound from './components/NotFound'

import './App.css'

const App = () => (
  <BrowserRouter>
      <Header />
      <Route exact path="/" component={BlogsList} />
      <Route path="/about" component={About} />
      <Route path="/contact" component={Contact} />
      <Route path="/blogs/:id" component={BlogItemDetails} />
      <Route component={NotFound} />
  </BrowserRouter>
)

export default App

## Solution:

# File: src/App.js

import { BrowserRouter, Route, Switch } from 'react-router-dom'

import Header from './components/Header'
import About from './components/About'
import Contact from './components/Contact'
import BlogsList from './components/BlogsList'
import BlogItemDetails from './components/BlogItemDetails'
import NotFound from './components/NotFound'

import './App.css'

const App = () => (
  <BrowserRouter>
      <Header />
      <Switch>
      <Route exact path="/" component={BlogsList} />
      <Route path="/about" component={About} />
      <Route path="/contact" component={Contact} />
      <Route path="/blogs/:id" component={BlogItemDetails} />
      <Route component={NotFound} />
       </Switch>
  </BrowserRouter>
)

export default App
------------------------------------------------------------------------

## 6. Placing Common component inside Switch
# Mistake:

# File: src/App.js

import { BrowserRouter, Route, Switch } from 'react-router-dom'

import Header from './components/Header'
import About from './components/About'
import Contact from './components/Contact'
import BlogsList from './components/BlogsList'
import BlogItemDetails from './components/BlogItemDetails'
import NotFound from './components/NotFound'

import './App.css'

const App = () => (
  <BrowserRouter>
      <Switch>
      <Header />
      <Route exact path="/" component={BlogsList} />
      <Route path="/about" component={About} />
      <Route path="/contact" component={Contact} />
      <Route path="/blogs/:id" component={BlogItemDetails} />
      <Route component={NotFound} />
       </Switch>
  </BrowserRouter>
)

export default App

## Solution:

# File: src/App.js

import { BrowserRouter, Route, Switch } from 'react-router-dom'

import Header from './components/Header'
import About from './components/About'
import Contact from './components/Contact'
import BlogsList from './components/BlogsList'
import BlogItemDetails from './components/BlogItemDetails'
import NotFound from './components/NotFound'

import './App.css'

const App = () => (
  <BrowserRouter>
      <Header />
      <Switch>
      <Route exact path="/" component={BlogsList} />
      <Route path="/about" component={About} />
      <Route path="/contact" component={Contact} />
      <Route path="/blogs/:id" component={BlogItemDetails} />
      <Route component={NotFound} />
       </Switch>
  </BrowserRouter>
)

export default App
------------------------------------------------------------------------

## 7. Providing the same PATH for multiple Routes
# Mistake:

If same path is given for multiple Routes, the Switch renders the first matched Component.

# File: src/App.js

import { BrowserRouter, Route, Switch } from 'react-router-dom'

import Header from './components/Header'
import About from './components/About'
import Contact from './components/Contact'
import BlogsList from './components/BlogsList'
import BlogItemDetails from './components/BlogItemDetails'
import NotFound from './components/NotFound'

import './App.css'

const App = () => (
  <BrowserRouter>
      <Header />
      <Switch>
      <Route exact path="/about" component={BlogsList} />
      <Route path="/about" component={About} />
      <Route path="/contact" component={Contact} />
      <Route path="/blogs/:id" component={BlogItemDetails} />
      <Route component={NotFound} />
       </Switch>
  </BrowserRouter>
)

export default App

## Solution:

# File: src/App.js

import { BrowserRouter, Route, Switch } from 'react-router-dom'

import Header from './components/Header'
import About from './components/About'
import Contact from './components/Contact'
import BlogsList from './components/BlogsList'
import BlogItemDetails from './components/BlogItemDetails'
import NotFound from './components/NotFound'

import './App.css'

const App = () => (
  <BrowserRouter>
      <Header />
      <Switch>
      <Route exact path="/" component={BlogsList} />
      <Route path="/about" component={About} />
      <Route path="/contact" component={Contact} />
      <Route path="/blogs/:id" component={BlogItemDetails} />
      <Route component={NotFound} />
       </Switch>
  </BrowserRouter>
)

export default App
------------------------------------------------------------------------

## 8. Missing Colon(:) while providing Path Parameters
# Mistake:

# File: src/App.js

import { BrowserRouter, Route, Switch } from 'react-router-dom'

import Header from './components/Header'
import About from './components/About'
import Contact from './components/Contact'
import BlogsList from './components/BlogsList'
import BlogItemDetails from './components/BlogItemDetails'
import NotFound from './components/NotFound'

import './App.css'

const App = () => (
  <BrowserRouter>
    <Header />
    <Switch>
      <Route exact path="/" component={BlogsList} />
      <Route path="/about" component={About} />
      <Route path="/contact" component={Contact} />
      <Route path="/blogs/id" component={BlogItemDetails} />
      <Route component={NotFound} />
    </Switch>
  </BrowserRouter>
)

export default App

## Solution:

# File: src/App.js

import { BrowserRouter, Route, Switch } from 'react-router-dom'

import Header from './components/Header'
import About from './components/About'
import Contact from './components/Contact'
import BlogsList from './components/BlogsList'
import BlogItemDetails from './components/BlogItemDetails'
import NotFound from './components/NotFound'

import './App.css'

const App = () => (
  <BrowserRouter>
    <Header />
    <Switch>
      <Route exact path="/" component={BlogsList} />
      <Route path="/about" component={About} />
      <Route path="/contact" component={Contact} />
      <Route path="/blogs/:id" component={BlogItemDetails} />
      <Route component={NotFound} />
    </Switch>
  </BrowserRouter>
)

export default App
------------------------------------------------------------------------

## 9. Accessing Promise Object
# Mistake:

In the Promise Object response, we got an error message with key error_msg but in the below code snippet, we are trying to access the promise object with the wrong key errorText so undefined will be logged in the console.

# File: src/components/LoginForm/index.js

import {Component} from 'react'
import Cookies from 'js-cookie'
import {Redirect} from 'react-router-dom'

import './index.css'

class LoginForm extends Component {
  state = {
    username: '',
    password: '',
    showSubmitError: false,
    errorMsg: '',
  }

  onChangeUsername = event => {
    this.setState({username: event.target.value})
  }

  onChangePassword = event => {
    this.setState({password: event.target.value})
  }

  onSubmitSuccess = jwtToken => {
    const {history} = this.props

 
    Cookies.set('jwt_token', jwtToken, {
      expires: 30,
    })

    history.replace('/')
  }

  onSubmitFailure = errorMsg => {
    this.setState({showSubmitError: true, errorMsg})
  }

  submitForm = async event => {
    event.preventDefault()
    const {username, password} = this.state
    const userDetails = {username, password}
    const url = 'https://apis.ccbp.in/login'
    const options = {
      method: 'POST',
      body: JSON.stringify(userDetails),
    }
    const response = await fetch(url, options)
    const data = await response.json()
    console.log(data.errorText)
    if (response.ok === true) {
      this.onSubmitSuccess(data.jwt_token)
    } else {
      this.onSubmitFailure(data.error_msg)
    }
  }

  renderPasswordField = () => {
    const {password} = this.state
    return (
      <>
        <label className="input-label" htmlFor="password">
          PASSWORD
        </label>
        <input
          type="password"
          id="password"
          className="password-input-filed"
          value={password}
          onChange={this.onChangePassword}
          placeholder="Password"
        />
      </>
    )
  }

  renderUsernameField = () => {
    const {username} = this.state
    return (
      <>
        <label className="input-label" htmlFor="username">
          USERNAME
        </label>
        <input
          type="text"
          id="username"
          className="username-input-filed"
          value={username}
          onChange={this.onChangeUsername}
          placeholder="Username"
        />
      </>
    )
  }

  render() {
    const {showSubmitError, errorMsg} = this.state
    const jwtToken = Cookies.get('jwt_token')
    if (jwtToken !== undefined) {
      return <Redirect to="/" />
    }
    return (
      <div className="login-form-container">
        <img
          src="https://assets.ccbp.in/frontend/react-js/nxt-trendz-logo-img.png"
          className="login-website-logo-mobile-image"
          alt="website logo"
        />
        <img
          src="https://assets.ccbp.in/frontend/react-js/nxt-trendz-login-img.png"
          className="login-image"
          alt="website login"
        />
        <form className="form-container" onSubmit={this.submitForm}>
          <img
            src="https://assets.ccbp.in/frontend/react-js/nxt-trendz-logo-img.png"
            className="login-website-logo-desktop-image"
            alt="website logo"
          />
          <div className="input-container">{this.renderUsernameField()}</div>
          <div className="input-container">{this.renderPasswordField()}</div>
          <button type="submit" className="login-button">
            Login
          </button>
          {showSubmitError && <p className="error-message">{errorMsg}</p>}
        </form>
      </div>
    )
  }
}

export default LoginForm

## Solution:

# File: src/components/LoginForm/index.js

import {Component} from 'react'
import Cookies from 'js-cookie'
import {Redirect} from 'react-router-dom'

import './index.css'

class LoginForm extends Component {
  state = {
    username: '',
    password: '',
    showSubmitError: false,
    errorMsg: '',
  }

  onChangeUsername = event => {
    this.setState({username: event.target.value})
  }

  onChangePassword = event => {
    this.setState({password: event.target.value})
  }

  onSubmitSuccess = jwtToken => {
    const {history} = this.props

 
    Cookies.set('jwt_token', jwtToken, {
      expires: 30,
    })

    history.replace('/')
  }

  onSubmitFailure = errorMsg => {
    this.setState({showSubmitError: true, errorMsg})
  }

  submitForm = async event => {
    event.preventDefault()
    const {username, password} = this.state
    const userDetails = {username, password}
    const url = 'https://apis.ccbp.in/login'
    const options = {
      method: 'POST',
      body: JSON.stringify(userDetails),
    }
    const response = await fetch(url, options)
    const data = await response.json()
    console.log(data.error_msg)
    if (response.ok === true) {
      this.onSubmitSuccess(data.jwt_token)
    } else {
      this.onSubmitFailure(data.error_msg)
    }
  }

  renderPasswordField = () => {
    const {password} = this.state
    return (
      <>
        <label className="input-label" htmlFor="password">
          PASSWORD
        </label>
        <input
          type="password"
          id="password"
          className="password-input-filed"
          value={password}
          onChange={this.onChangePassword}
          placeholder="Password"
        />
      </>
    )
  }

  renderUsernameField = () => {
    const {username} = this.state
    return (
      <>
        <label className="input-label" htmlFor="username">
          USERNAME
        </label>
        <input
          type="text"
          id="username"
          className="username-input-filed"
          value={username}
          onChange={this.onChangeUsername}
          placeholder="Username"
        />
      </>
    )
  }

  render() {
    const {showSubmitError, errorMsg} = this.state
    const jwtToken = Cookies.get('jwt_token')
    if (jwtToken !== undefined) {
      return <Redirect to="/" />
    }
    return (
      <div className="login-form-container">
        <img
          src="https://assets.ccbp.in/frontend/react-js/nxt-trendz-logo-img.png"
          className="login-website-logo-mobile-image"
          alt="website logo"
        />
        <img
          src="https://assets.ccbp.in/frontend/react-js/nxt-trendz-login-img.png"
          className="login-image"
          alt="website login"
        />
        <form className="form-container" onSubmit={this.submitForm}>
          <img
            src="https://assets.ccbp.in/frontend/react-js/nxt-trendz-logo-img.png"
            className="login-website-logo-desktop-image"
            alt="website logo"
          />
          <div className="input-container">{this.renderUsernameField()}</div>
          <div className="input-container">{this.renderPasswordField()}</div>
          <button type="submit" className="login-button">
            Login
          </button>
          {showSubmitError && <p className="error-message">{errorMsg}</p>}
        </form>
      </div>
    )
  }
}

export default LoginForm
------------------------------------------------------------------------

## 10. Ordering of Routes inside Switch
# Mistake:

In the below code snippet NotFound Component is placed first in the Switch Component.
If the path is not mentioned and route (NotFound Route) is placed first inside Switch, Browser will render it in all the paths.

# File: src/App.js

import {Route, Switch } from 'react-router-dom'

import Header from './components/Header'
import About from './components/About'
import Contact from './components/Contact'
import BlogsList from './components/BlogsList'
import BlogItemDetails from './components/BlogItemDetails'
import NotFound from './components/NotFound'

import './App.css'

const App = () => (
    <Header />
    <Switch>
      <Route exact path="/" component={BlogsList} />
      <Route path="/about" component={About} />
      <Route path="/contact" component={Contact} />
      <Route path="/blogs/:id" component={BlogItemDetails} />
      <Route component={NotFound} />
    </Switch>
)

export default App

## Solution:

# File: src/App.js

import { BrowserRouter, Route, Switch } from 'react-router-dom'

import Header from './components/Header'
import About from './components/About'
import Contact from './components/Contact'
import BlogsList from './components/BlogsList'
import BlogItemDetails from './components/BlogItemDetails'
import NotFound from './components/NotFound'

import './App.css'

const App = () => (
  <BrowserRouter>
    <Header />
    <Switch>
      <Route exact path="/" component={BlogsList} />
      <Route path="/about" component={About} />
      <Route path="/contact" component={Contact} />
      <Route path="/blogs/:id" component={BlogItemDetails} />
      <Route component={NotFound} />
    </Switch>
  </BrowserRouter>
)

export default App
------------------------------------------------------------------------

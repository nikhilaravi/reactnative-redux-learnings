# Learnings

For the past 4 months i've been working on a project using several cool new technologies. I've learnt so much and I want to make sure I document at least some of the best practices, patterns, and coding styles we have been using.

This is all very much Work In Progress!!  

## Front end
* React
* React Native
* Redux

## Back end
* Hapi
* Joi
* Postgres
* Redis

## Other
* Xcode


## React

### Always declare PropTypes in Components
  - Be as descriptive as possible using `isRequired`, and `oneOf` etc - this can help with giving more descriptive errors
  - When modifying a component, also update the PropTypes

### Use ES6 classes for components

### Group imports at the top of the component e.g

  ```js
  //npm modules
  import React from 'react-native'

  //components
  import Footer from './Footer.js';
  ```

### Add comments at the top of re-usable components giving a description of what it does and the props it takes so other people can use the component more easily!

  ```js
  /**
  * SEARCH BAR Component
  *
  * Reusable search bar featuring a text input field and a submit button.
  *
  * Props
  * - onChangeText: function to be called on text input from the user
  * - onPress: function to be called when the submit button is clicked
  * - placeholderText: placeholder for the text input field
  *
  **/
  ```

### Destructure out the props and state that you need at the beginning of each component method

  ```js
  const {
    state: {},
    props: {}
  } = this;
  ```

### Higher Order components - use to extend and enhance functionality of components similar to mixins when using ES5 with React

  ```js
    // higher order component e.g. wrapper.js
    module.exports = ComponentToExtend => class Test extends Component {

      // add lifecycle methods, methods and state

      render(){
        return <ComponentToExtend /> // pass state and methods as props
      }
    }

  // use inside another component e.g. test-compoent.js

  import Wrapper from './wrapper.js';

  //component code

  module.exports = Wrapper(TestComponent);
  ```

### Add a React linter to help keep style consistent across the project

`eslint` has a plugin for React called `eslint-plugin-react`. Install both of these modules along with `babel-eslint` to use ES6. Then add an .eslintrc file with the options you want enabled, add a lint script to your package.json and lint away!

Example .eslintrc file

```json
  {
    "parser": "babel-eslint",
    "plugins": ["react"],
    "env": {
      "node": true
    },
    "ecmaFeatures": {
      "jsx": true
    },
    "extends": "eslint:recommended",
    "rules": {
      "react/jsx-max-props-per-line": [1, {"maximum": 3}],
      "react/jsx-no-duplicate-props": 1,
      "react/jsx-no-undef": 1,
      "react/jsx-uses-react": 1,
      "react/jsx-uses-vars": 1,
      "react/no-danger": 1,
      "react/no-did-mount-set-state": 1,
      "react/no-did-update-set-state": 1,
      "react/no-direct-mutation-state": 1,
      "react/prop-types": 1,
      "react/react-in-jsx-scope": 1,
      "react/self-closing-comp": 1,
      "react/sort-comp": 1,
      "strict": 0,
      "no-unreachable": 0,
      "no-console": 0,
      "comma-dangle": 0,
      "jsx-quotes": [1, "prefer-single"],
    }
  }
  ```

## React Native

### Split styles out into small reusable elements

  ```js
  const styles = Stylesheet.create({
    text: {
      fontSize: 10
    },
    bold: {
      fontWeight: 'bold'
    }
  })

  // combine styles using an array:
  <Text style = {[styles.text, styles.bold]} />Hello</Text>
  ```

### Indent styles to mimic the JSX component hierarchy

  ```js
  //inside the render function:

  render() {
    return (
      <View style={styles.container}>
        <View style={styles.titleContainer}>
          <Text style={styles.title}><Text>
        </View>
        <View style={styles.footerContainer}>
          <Text style={styles.credits}></Text>
        <View>
      </View>
    )
  }

  // in the Stylesheet

  const styles = Stylesheet.create({
    container: {
      //style
    },
      titleContainer: {

      },
        title: {

        },
      footerContainer: {

      },
        credits: {

        }
  })
  ```

## Xcode!

### Simulator

There's a handy 'inspector' setting you can use similar to 'inspect' element in the browser. When running the simulator hit cmd + z to show the settings


## Redux

### Folder structure

Organize your files in the following way:

```js
- app
  - components
  - containers
  - actions
  - reducers
    - index.js // import and combine reducers
  - constants
    - action_types.js
```
### Containers

Containers should not have any styling. They should take actions and state and in the
render function return a React component with these passed as props.

### Middleware

- [Redux logger](https://github.com/fcomb/redux-logger) is extremely useful for tracking when actions are called and how the store is modified. Every action is logged in the console in the browser.

### Testing

#### Reducers
Export the initial state so it can be imported into tests.

```js
export const initialState = {
  loggedIn:     false,
  message:      '',
  loginPage:    'login'
};
```

## Hapi

- use Joi validation at post request endpoints so the handler isn't called if the data doesn't match a schema
- don't return descriptive errors - security by obscurity!

e.g.

  ```js
  import Joi from 'joi'
  import login from './handlers/login';

  var signIn = Joi.object().keys({
    username: Joi.string().required(),
    password: Joi.string().required()
  });

  var routes = [
    {
      method: 'POST',
      path: '/login',
      config: {
        auth: false,
        validate: {
          payload: signIn,
          failAction: (req,res) => res({status:'error',message:'data missing or incorrect'}).code(400),
        },
      },
      handler: login,
    }
  ]

  ```

## General Style

### Add comments before each function declaration

  ```js
  /*
   * @param {Number} - unix time stamp
   * @return {String} - time formated as 'SAT 19TH MAY 15:30'
   */
  ```

### Format objects to make them more readable

  ```js
  const object = {key: value, key2: v2}; ok

  const object = {k1: v1, k1: v1, k1: v1, k1: v1, k1: v1, k1: v1, k1: v1} bad

  const object = {
    k1: v1,
    k1: v1,
    k1: v1,
    k1: v1,
    k1: v1,
    k1: v1,
    k1: v1
  } good
  ```

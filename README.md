# React-Knowledge

### Tip 1 (ساختن قالب پروژه بسیار مهمه)

##### قالب اول
![Screenshot (25)](https://user-images.githubusercontent.com/53907570/89124336-7dba8400-d4eb-11ea-9809-00d06072d154.png)

##### قالب دوم
![Screenshot (26)](https://user-images.githubusercontent.com/53907570/89124380-c6723d00-d4eb-11ea-8ab7-0f720862ea88.png)

##### قالب سوم
`index.html`
![Screenshot (27)](https://user-images.githubusercontent.com/53907570/89124514-ddfdf580-d4ec-11ea-8c0d-eaa798972d02.png)
as you can see here I just changed the body `dir='rtl'`

##### قالب چهارم
`manifest.json`
![Screenshot (28)](https://user-images.githubusercontent.com/53907570/89124569-4e0c7b80-d4ed-11ea-8df7-708a8da452bf.png)

#### eslint.rc
```js
{
  "extends": [
    "airbnb",
    "airbnb/hooks",
    "plugin:prettier/recommended",
    "prettier/react",
    "prettier/standard"
  ],
  "plugins": ["prettier"],
  "parserOptions": {
    "ecmaFeatures": {
      "jsx": true
    },
    "ecmaVersion": 2018
  },
  "rules": {
    "react/jsx-filename-extension": [
      1,
      {
        "extensions": [".js", ".jsx"]
      }
    ],
    "react/prop-types": 0,
    "import/imports-first": ["error", "absolute-first"],
    "import/newline-after-import": "error",
    "import/prefer-default-export": "off",
    "react/jsx-props-no-spreading": "off"
  },
  "globals": {
    "window": true,
    "document": true,
    "localStorage": true,
    "FormData": true,
    "FileReader": true,
    "Blob": true,
    "navigator": true,
    "fetch": true
  }
}
```

#### .gitignore
```js
# See https://help.github.com/articles/ignoring-files/ for more about ignoring files.

# dependencies
/node_modules
/.pnp
.pnp.js

# testing
/coverage

# production
/build

# misc
.DS_Store
.env.local
.env.development.local
.env.test.local
.env.production.local

npm-debug.log*
yarn-debug.log*
yarn-error.log*
```

#### .prettierrc
```js
{
  "singleQuote": true,
  "semi": false,
  "endOfLine": "auto",
  "trailingComma": "none"
}
```

#### package.json
```js
{
  "name": "goodreads",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "@material-ui/core": "^4.11.0",
    "@material-ui/icons": "^4.9.1",
    "@material-ui/lab": "^4.0.0-alpha.52",
    "@testing-library/jest-dom": "^4.2.4",
    "@testing-library/react": "^9.5.0",
    "@testing-library/user-event": "^7.2.1",
    "jss-rtl": "^0.3.0",
    "moment-jalaali": "^0.9.2",
    "prettier": "^2.0.5",
    "react": "^16.13.1",
    "react-code-input": "^3.9.0",
    "react-dom": "^16.13.1",
    "react-ga": "^2.7.0",
    "react-helmet": "^6.0.0",
    "react-material-ui-carousel": "^1.5.1",
    "react-router-dom": "^5.2.0",
    "react-scripts": "3.4.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "eslintConfig": {
    "extends": "react-app"
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  },
  "devDependencies": {
    "eslint": "^6.8.0",
    "eslint-config-airbnb": "^18.1.0",
    "eslint-config-prettier": "^6.11.0",
    "eslint-plugin-import": "^2.20.1",
    "eslint-plugin-jsx-a11y": "^6.2.3",
    "eslint-plugin-prettier": "^3.1.3",
    "eslint-plugin-react": "^7.19.0",
    "eslint-plugin-react-hooks": "^2.5.0"
  }
}
```

#### Now the most important section `src` folder

### Tip 2

Always remember when submitting a form to set the input states back to null, always setState the inputs to empty string.

```js
this.setState({
  name: '',
  password: ''
})
```


### Tip 3 (Calling 2 different actions from the same onClick event)

In some Case You want to call a different action depending on different condition. Actually You want to have one onClick event with 2 different functions: Do this:

```js
addGroupsRelUsers = () => {
        const selectedArrayToString = this.state.selected.toString()
        this.props.groupsRelUsersInsert(this.props.groupId, selectedArrayToString, this.props.token)
}

addUsersRelGroups = () => {
        const selectedArrayToString = this.state.selected.toString()
        this.props.addUsersRel_Group(selectedArrayToString, this.props.userID, this.props.token)
}
    
    
<Button onClick={() => {
                    this.addUsersRelGroups();
                    this.addGroupsRelUsers();
                }}>
</Button>
```

As you can see i call 2 different function in 2 different cases.


### Tip 4

To remove [] from an array =>>>  ["100", "102"]   =>>> "100", "102"

```js
array.toString()
```


### Tip 5

Any time you want to send data which you have sent as payload in addition to res.data you have to go send data in payload just like this:

```js
export const groupRelMenuInsert = (groupID, menuIDs, token) => dispatch => {
    const config = {
        headers: {
            'Token': token
        }
    }

    const body = { groupID, menuIDs }

    axios.post(`${API_BASE_ADDRESS}Groups/GroupsRelMenu_Insert`, body, config)
        .then(res => {
            dispatch({
                type: GROUP_REL_MENU_INSERT,
                payload: [res.data, { menuIDs }]
            })
        })
        .catch(err => {
            console.log(err)
        })
}
```

And also, you have to initialize the initialState as an empty array just like this:

```js
import { GROUP_REL_MENU_INSERT } from '../../../../actions/dashboard/types';

const initialState = []

export const groupRelMenuInsert = (state = initialState, action) => {
    switch (action.type) {
        case GROUP_REL_MENU_INSERT:
            return {
                ...state,
                ...action.payload
            }
        default: return state
    }
}
```

By doing this you have a reducer just like an array which the second parameter of the array is the data which you've sent, in this case is menuIDs.


### Tip 6

To split a string by comma and setState to an array use this method:

```js
const string = 'arghun, sahand, shahla';

const seperateByComma = string.split(',');

console.log(seperateByComma);

["arghun", " sahand", " shahla"]
```

### Tip 7

to sort an array of objects according to some specific element:

for example to sort an array of objects according to the MenuID:

```js
const MyArray = [{MenuID: 100200, Access: "c"}, {MenuID: 100400, Access: "d"}, {MenuID: 100200, Access: "e"}, {MenuID: 101000, Access: "e"}];
```

Use this function first: (compareValues)

```js
function compareValues(key, order = 'asc') {
  return function innerSort(a, b) {
    if (!a.hasOwnProperty(key) || !b.hasOwnProperty(key)) {
      // property doesn't exist on either object
      return 0;
    }

    const varA = (typeof a[key] === 'string')
      ? a[key].toUpperCase() : a[key];
    const varB = (typeof b[key] === 'string')
      ? b[key].toUpperCase() : b[key];

    let comparison = 0;
    if (varA > varB) {
      comparison = 1;
    } else if (varA < varB) {
      comparison = -1;
    }
    return (
      (order === 'desc') ? (comparison * -1) : comparison
    );
  };
}
```

and then 

```js
MyArray.sort(compareValues('MenuID'));
```

MyArray:

```js
0: {MenuID: 100200, Access: "c"}
1: {MenuID: 100200, Access: "e"}
2: {MenuID: 100400, Access: "d"}
3: {MenuID: 101000, Access: "e"}
```

### Tip 8

Always in onClick function if you want to run a specific function do it just like this:

```js
function = () => {
  // Do Something
}
```

```js
onClick={this.function}    // Correct
```

```js
onClick={() => this.function}    // InCorrect
```


### Tip 9

To handle Checkbox States in Javascript if it is checked or not and update the state based on that checked state Do this method in componentDidUpdate:

```js
if (this.state.createCheckbox === true) {
            if (this.state.GroupRelMenuAccess_Set_StateUpdated === false) {
                this.setState({
                    GroupRelMenuAccess_Set_State: [this.state.GroupRelMenuId, 'c'],
                    GroupRelMenuAccess_Set_StateUpdated: true
                })
            }
        } else if (this.state.createCheckbox === false) {
            if (this.state.GroupRelMenuAccess_Set_StateUpdated === true) {
                this.setState({
                    GroupRelMenuAccess_Set_State: [null, null],
                    GroupRelMenuAccess_Set_StateUpdated: false
                })
            }
        }
```

### Tip 10

Always in redux when you do CRUD works you have to call the getData as quickly as possible to update the page.

And to do that you have to write these actions reducers in the same place as getData reducer just like this. And if you want to get MsgId of the action you have to write another reducer to get those data.

Example:

For example I have a list of users and I am doing CRUD applications on them.

This is the Reducer format of users.js

```js
import {
    GET_USERS_LIST_REQUEST,
    GET_USERS_LIST_SUCCESS,
    GET_USERS_LIST_FAILURE,
    DELETE_USER,
    ADD_USER,
    EDIT_USER
} from '../../../../actions/dashboard/types';

const initialState = {
    users: null,
    loading: false,
    error: ''
}

export const users = (state = initialState, action) => {
    switch (action.type) {
        case GET_USERS_LIST_REQUEST:
            return {
                loading: true
            }
            break;
        case GET_USERS_LIST_SUCCESS:
            return {
                loading: false,
                users: action.payload
            }
            break;
        case GET_USERS_LIST_FAILURE:
            return {
                loading: false,
                error: action.payload
            }
            break;
        case DELETE_USER: {
            return {
                ...state,
                ...action.payload
            }
        }
        case ADD_USER: {
            return {
                ...state,
                ...action.payload
            }
        }
        case EDIT_USER: {
            return {
                ...state,
                ...action.payload
            }
        }
        default: return state
    }
}
```

As you can see I have written DELETE_USER, ADD_USERT and EDIT_USER action reducers inside my main Users Reducer.


And then to use the MsgId return from the CRUD actions you have to define another reducers for all of them:

addUser.js (reducer)

```js
import {
    ADD_USER
} from '../../../../actions/dashboard/types';

const initialState = null;

export const addUser = (state = initialState, action) => {
    switch (action.type) {
        case ADD_USER: return action.payload
        default: return state
    }
}
```

editUser.js (reducer)

```js
import {
    EDIT_USER
} from '../../../../actions/dashboard/types';

const initialState = null

export const editUser = (state = initialState, action) => {
    switch (action.type) {
        case EDIT_USER: return action.payload
        default: return state
    }
}
```

deleteUser.js (reducer)

```js
import {
    DELETE_USER
} from '../../../../actions/dashboard/types';

const initialState = null;

export const deleteUser = (state = initialState, action) => {
    switch (action.type) {
        case DELETE_USER: return action.payload
        default: return state
    }
}
```

### Tip 11

in some cases you need to change the state of the parent component form child component.

For example I want to change the state of the showAlert state from false to true in parent component. I want to do this when submitting the form from child component.

first define a function parent component:

Parent.js

```js
changeState = () => {
  this.setState({ showAlert: true })
}
```

then pass this function through props to your child component:

Child.js

```js
handleSubmit = () => {
  this.props.changeState()
}
```

### Tip 12

in some cases you need to run several actions in one onClick. Do this:

```js
onClose={() => this.props.closeModal() || this.setState({ userModalUpdated: false, groupRelMenuUpdated: false, userRelMenuUpdated: false })}
```

Here I closeModal and setState userModalUpdated, groupRelMenuUpdated and userRelMenuUpdated back to false. in one `onClick`

### Tip 13

**Why does `CORS Error` happen?**

because, when you send request from `localhost:3000` to the `WebService`, the `WebService` should allow the `localhost:3000` to send the request. whenever this error happened, you just need to talk to the backend developer to give you a `PORT NUMBER` which you can send the `Request` to the `Server`. for example the PORT 5001 is open and you can send the request. So you have to change the `PORT NUMBER` of your application inseted of the `3000`.

How?

Go to `package.json`:

```js
"scripts": {
    "start": "set PORT=5001 && react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
 },
```

As you can see I set the `start` to PORT, 5001  by `set PORT=5001 && react-scripts start`.

That's it You solved the `CORS` issue.


### Tip 14

always to render the `key` of an object dynamically, you have to put the variable inside `[]`

example:

```js
{
  [head1Field]: '14',
  [head2Field]: 'گروه دوم',
  rowNumber: 2
},
```

**as you can see I have put the `head1Field` and `head2Field` dynamically from props**


### Tip 15

always remember when you assign default state, **Don't** set it to **null**, if you have a string, set to **empty string**, or if you have an array of lists, set it to **empty array**

INCORRECT:
```js
const state = {
  visitors: null
}
```

CORRECT:
```js
const state = {
  visitors: []
}
```

### Tip 16

Imagine You want to operate several `functions` inside one onChange and also you want to `map` through an `array` and also you want to `setState` depending on a condition which occurs during `map`

example: I have a `Input.Search` tag which users types something in it and the `search` input has a `search icon button` which we want to show different content to the user, according to the condition and `state` which we `setState` inside `onSearch` of the input tag

**We want to show different content to the user depending on the state of the `genericCodeFound`**

**Notice that `onSearch` will be called after user `clicked` on `search` icon**

```js
const [genericCodeFound, setGenericCodeFound] = useState(null)
const [genericCodeSearchLoading, setGenericCodeSearchLoading] = useState(false)

<Input.Search
  enterButton
  type="text"
  value={genericCode}
  onChange={(e) => setGenericCode(e.target.value)}
  placeholder="کد ژنریک"
  loading={genericCodeSearchLoading}
  onSearch={() => {
    setGenericCodeSearchLoading(true)
    genericCodes.map((item) => {
      if (item.genericCode === genericCode) {
        return setGenericCodeFound(true)
      }
      return setGenericCodeFound(false)
    })
    setTimeout(() => {
      setGenericCodeSearchLoading(false)
    }, 2000)
  }}
/>
```

### Tip 17

**The correct way of using `.then` and `.catch` in `axios` to use the correct `status` and `message` got from `backend`**

**The important thing here is that you have to remember handle `error` inside `.catch` and use `err.response` not just `err`**

```js
axios
  .delete(
    `${API.PRODUCT_CATEGORY_DELETE}/${oldData.categoryId}`,
    config,
    data
  )
  .then((res) => {
    if (res.status === 200 || res.status === 204) {
      productCategoryDispatch({ type: DELETE_PRODUCT_CATEGORY })
      EventDispatch({ type: SUCCESS, message: 'رکورد با موفقیت حذف شد' })
    }
  })
  .catch((err) => {
    EventDispatch({
      type: ERROR,
      message: err.response.data.errors[0].error
    })
  })
```

### Tip 18

**always remember when you use `modal` in your component, absolutely the modal has a parameter called `open or visible` and you use that to show or not show the modal, `But` another thing you have to notice is that you also have to specify a `conditional statement` for `render or not render` the modal**

`why we do this?`

because if you don't do this, the content of the modal will be rendered in the first time you render the page, `this is not good`, we only want to render the content of the modal, when the modal is being rendering.

So: Do this

```js
{openModal ?
<Modal
  openModal={openModal}
/>
: null
}
```


### Tip 19 (DropDown Pagination)

**here I created a dropdown select of antd using `pagination` to load more data**

```js
const [loading, setLoading] = useState(false)

const onScroll = (event) => {
    const target = event.target
    const scrollTop = target.scrollTop
    const offsetHeight = target.offsetHeight
    const scrollHeight = target.scrollHeight
    if (scrollTop + offsetHeight === scrollHeight) {
      setLoading(true)
      target.scrollTo(0, target.scrollHeight)
      setPageNumber((pageNumber) => pageNumber + 1)
      const config = {
        headers: {
          Authorization: `bearer ${token}`
        }
      }
      setTimeout(() => {
        const loadMoreProducts = async () => {
          try {
            const response = await axios.get(
              `${API.PRODUCT_GET}?PageNumber=${pageNumber}`,
              config
            )
            response.data.lists.forEach((item) => products.push(item))
            setLoading(false)
          } catch (err) {
            EventDispatch({
              type: ERROR,
              message: err.response.data.errors[0].error
            })
          }
        }

        loadMoreProducts()
      }, 500)
    }
  }
  
  
  <Select
    virtual
    showSearch
    className={classes.input}
    placeholder="کالا"
    optionFilterProp="children"
    allowClear
    filterOption={(input, option) =>
      option.children.toLowerCase().indexOf(input.toLowerCase()) >=
      0
    }
    getPopupContainer={(trigger) => trigger.parentNode}
    notFoundContent="داده ای یافت نشد"
    value={productId}
    onChange={(val) => setProductId(val)}
    onPopupScroll={onScroll}
  >
    {loading ? (
      <Option>Loading...</Option>
    ) : (
      products.map((product) => (
        <Option value={product.productId}>
          {product.productName}
        </Option>
      ))
    )}
  </Select>
```


### add remote url

`git remote add NameOfRepo HTTPS`

`git remote add Custom-Fetch-Hook-With-React-Hooks 'https://github.com/Arghun-dev/Custom-Fetch-Hook-with-React-Hooks.git'`


# JavaScript: How can I change property names of objects in an array?

use `map` method

for example in here, I'm going to change the property names of the objects in an array from `name` to `label` and from `id` to `value`.

```js
tags.map((item) => ({ label: item.name, value: item.id }));
```

```js
```

# text-overflow: 'ellipsis'

To make some `text-overflow` ellipsis, write this code below:

```js
.text {
   overflow: hidden;
   white-space: nowrap;
   text-overflow: ellipsis;
}
```


# Access origin URL in React

`$. window.location.origin`


# double exclamation mark meaning in JS

it's short way to cast a variable to be a boolean (true or false) value.

JavaScript is not a static language, rather it is a dynamic language. That means that a variable can reference or hold a value of any type, and further, that type can be changed at any point. Whether you prefer a static or dynamic language is for you to decide.

But, we can certainly have a notion of a type in JavaScript. Here is a quick list of the various data types in JavaScript:

1. Boolean
2. String
3. Number
4. Object

A boolean data type is the simplest of all data types as it is a simple bit value: 0 (false) or 1 (true).

We can set a variable to a boolean value and use it when evaluating an if-statement. Here's our simply example.

```js
function() {
  var thisIsTrue = true;
  if (thisIsTrue) {
    window.alert('It certainly is!');
  }
}
```

When the function above is executed we will get the alert It certainly is! because the variable thisIsTrue is being set to the boolean value of true.


Now, let's look at how JavaScript can evaluate a value that is not a boolean to be casted to a boolean.

```js
function() {
  var nothing = '';
  if (nothing) {
    window.alert('Nothing');
  } else {
    window.alert('Huh?');
  }
}
```

When the function above is executed we will get the alert `Huh`? because the value of the variable `nothing` is being evaluated to be `false`. This is what is commonly referred to as `truthy` versus `falsey`.

```js
const name = 'arghun';
console.log(!!name); // true

const newName = '';
console.log(!!newName); // false
```

**The following values are considered by JavaScript to be `falseys`**

`Empty string: ""`

`0`

`null`

`undefined`

`NaN`

**The following values are considered by JavaScript to be `truthys`**

`Object: {}`

`Array: []`

`Not empty string: "anything"`

`Number other than zero: 3.14`

`Date: new Date()`

### So, why double exclamation marks?

In some cases you may want to cast a variable to be explicitly boolean. Why? Well, the number one reason is that most of time developers do not use type safe comparison operators.

When using the type safe comparison operators you are both checking that the values are equal (or unequal) and that their type is the same. Without the type safe comparison operators you are allowing the JavaScript engine the freedom to coerce your variables to true or false based on the truthy/falsey logic.


# URI, encodeURIComponent(), decodeURIComponent()

**URI**

A URI (Uniform Resource Identifier) is a string that refers to a resource. The most common are URLs, which identify the resource by giving its location on the Web. URNs, by contrast, refer to a resource by a name, in a given namespace, such as the ISBN of a book.

### encodeURIComponent()

The encodeURIComponent() function encodes a URI by replacing each instance of certain characters by one, two, three, or four escape sequences representing the UTF-8 encoding of the character (will only be four escape sequences for characters composed of two "surrogate" characters).

```js
// encodes characters such as ?,=,/,&,:
console.log(`?x=${encodeURIComponent('test?')}`);
// expected output: "?x=test%3F"

console.log(`?x=${encodeURIComponent('шеллы')}`);
// expected output: "?x=%D1%88%D0%B5%D0%BB%D0%BB%D1%8B"
```

**Syntax**

```js
encodeURIComponent(str);
```

Paramaters:

`str`

Return Value:

A new string representing the provided string encoded as a URI component.


### decodeURIComponent()

The `decodeURIComponent()` function decodes a Uniform Resource Identifier (URI) component previously created by `encodeURIComponent` or by a similar routine.


# what is async and sync javascript ?

**synchronous:** So to recap, `synchronous` code is executed in sequense - each statement waits for the previous statement to finish before executing.

**Asynchronous:** Asynchronous code doesn't have to wait your program can continue to run. You do this to keep your site or app responsive, reducing waiting time for the user.

## What does `Async` mean ?

Async is short for “asynchronous”. It’s easier to understand async if you first understand what “synchronous”, the opposite, means.

In programming, we can simplify the definition of synchronous code as “a bunch of statements in sequence”; so each statement in your code is executed one after the other. This means each statement has to wait for the previous one to finish executing.

```js
console.log('First');
console.log('Second');
console.log('Third');
```

The statements above will execute in order, outputting “First”, “Second”, “Third” to the console. That’s because it’s written synchronously.

Asynchronous code takes statements outside of the main program flow, allowing the code after the asynchronous call to be executed immediately without waiting. You’ve probably used asynchronous programming before with jQuery.ajax or similar:

```js
console.log('First');
jQuery.get('page.html', function (data) {
    console.log("Second");
});
console.log('Third');
```

In the example above, the output will be different: “First”, “Third”, “Second”. This is because the function passed into jQuery.get is not called immediately – it has to wait for jQuery to fetch the page you asked for before it can execute.

“So why the hell use asynchronous code instead of synchronous code?" I hear you ask! I’ll explain.

## Why Asynchronous ?

When JavaScript is executed, synchronous code has the potential to block further execution until it has finished what it’s doing. In English, long-running JavaScript functions can make the UI or server unresponsive until the function has returned. Obviously this can result in a terrible user-experience.

For example: if you want to load your latest tweets onto a web page, and you do this synchronously, then a visitor to your site won’t be able to do anything until those tweets are loaded. This could cause a long delay before they even get to see the content of your site! I’ve illustrated the problem below:

```js
tweets = loadTweetsSync();
// ... Wait
// ... Do something with the tweets
doSomeOtherImportantThings();
```

```js
loadTweetsAsync(function () {
    // ... Wait
    // ... Do something with the tweets
});
doSomeOtherImportantThings();
```

In the second example, `doSomeOtherImportantThings` doesn’t have to wait for the tweets to load.

## How To Program Asynchronously ?

More often than not, this is done for you by browser/server APIs (XMLHttpRequest, Node fs module) or third-party libraries (jQuery.ajax). Most of the time, this is as far as you need to go – you wouldn’t asynchronize everything, as this can actually lead to less performant (and very complex) code.

As a general rule of thumb, you use `asynchronous` code when performing `expensive` and `time-consuming` operations. You wouldn’t use it to change a CSS class on an element, for example.

For when you need them, there are plenty of libraries which aid you in writing asynchronous code; Async.js is an excellent example.


## How to log for example cu event of an input in react ?

```js
<input
  value={state}
  onChange={(e) => setState(e.target.value)}
  onCut={console.log}
/>
```

## React NODE_ENV=development

`NODE_ENV=development`

React already has a lot of developer conviniences build into it out of the box. What's better is that they are automatically stripe it out when you compile your code for production.

So how do you get the debugging conveninces then? Well if you're using parcel.js, it will compile your development server with an environment variable of `NODE_ENV=development` and then when you run `parcel build`, it will automatically change that to `NODE_ENV=production` which is how all the extra weight gets stripped out.

why is it imporrtant that we debug stripe out? the dev bundle of React is quite a bit bigger and quite a bit slower than the production build. Make sure you're compiling with the correct environmental variables or your users will suffer.


## React Strict Mode

It's basically gonna throw stronger errors at you, and it's gonna enforce more opinions on you.

There's bunch of methods here called like `UNSAFE_componentWillMount` and there's other kinds of things that they are deprecated, and they don't want you to use them. And `Strict Mode` basically doesn't allow you to use them.

**is `stric mode` make my bundle size bigger?** The answer is no, It's just stripped out in production build.


## Error Boundaries

Frequently there's errors with APIs with malformatted or otherwise weird data. Let's be defensive about this, because we still want to use this API, but we can't control when we get errors. we're going to use a feature called `componentDidCatch` to handle this. this is something you can't do with `hooks`. so if you needed this sort of functionality. you'd have to use a `class` component.

a component can only catch errors in its children. so that's important to keep in mind. it cannot catch it's own errors. Let's go make a wrapper to use on `Detail.js`. Make a new file called `ErrorBoundary.js`


ErrorBoundary.js

```js
import { Component } from 'react';
import { Link } from 'react-router-dom';

class ErrorBoundary extends Component {
  state = { hasError: false };
  static getDerivedStateFromError() {
    return { hasError: true }
  }
  componentDidCatch(error, info) {
    // I log this to sentry, Azure Monitor, New Relic, TrackJS
    console.error('ErrorBoundary caught an error', error, info);
  }
  
  render() {
    if (this.state.hasError) {
      return (
        <h1>This listing has an error <Link to="/">Click here</Link> to go back to the home page</h1>
      )
    }
    
    return this.props.children;
  }
}
```

Now inside my `Details.js` component if ever there is an error I would like to catch that error and we wanna do something about it.

`ErrorBoundary` has to exist above it. It'll only catch things in components underneath it. So an easy way to do that is to use `Higher Order Components`

`Higher Order Component`, It's a component that adds functionality, but doesn't add display. That's a higher order component. Which is just a fancy name for things that don't display

Details.js

```js
import { Component } from 'react';
import ErrorBoundary from './ErrorBoundary';

class Details extends Component {
  componentDidMount() {
    // request to an api to get some data
  }
  
  redner() {
    // return some UI
  }
}
```

```js
export default function DetailsWithErrorBoundary() {
  return (
    <ErrorBoundary>
      <Details />
    </ErrorBoundary>
  )
}
```

## Redirect on Error

```js
import { Component } from 'react';
import { Link } from 'react-router-dom';

class ErrorBoundary extends Component {
  state = { hasError: false, redirect: false };
  
  static getDerivedStateFromError() {
    return { hasError: true }
  }
  
  componentDidCatch(error, info) {
    // I log this to sentry, Azure Monitor, New Relic, TrackJS
    console.error('ErrorBoundary caught an error', error, info);
    setTimeout(() => this.setState({ redirect: true }), 5000)
  }
  
  render() {
    if (this.state.redirect) {
      return <Redirect to='/' />
    } else if (this.state.hasError) {
      return (
        <h1>This listing has an error <Link to="/">Click here</Link> to go back to the home page</h1>
      )
    }
    
    return this.props.children;
  }
}
```

## CSS Children Selector

to select all the children elements of css use this selector:

```js
.content > * {
  ...
}
```

## React 18 Updates

**1. Automatic Batching**

Batching is React groups several state updates into single re-render for better performance.

For example, if you have two state updates inside of the same click event, React has always batched these into one re-render. If you run the following code, you’ll see that every time you click, React only performs a single render although you set the state twice:

```js
import { useState } from 'react';

const App = () => {
  const [count, setCount] = useState(0);
  const [flag, setFlag] = useState(false);
  
  const handleClick = () => {
    setCount(count + 1); // Does not re-render yet
    setFlag(!flag); // Does not re-render yet
    // React only one re-render compoent here
  }
}
```

**What if I don't want to batch?**

in this case you will have to use `flushSync`

```js
const handleClick = () => {
  flushSync(() => {
    setCount(count + 1)
  });
  
  flushSync(() => {
    setFlag(!flag)
  })
}
```

**2. SSR Support for Suspense**

**3. Transition**

This is an incredible feature going to be released. It lets users solve the issue of frequent updates on large screens. For example, consider typing in an input field that filters a list of data. You need to store the value of the field in state so that you can filter the data and control the value of that input field. Your code may look something like this:

```js
// Update the input value and search results
setSearchQuery(input);
```

Here, whenever the user types a character, we update the input value and use the new value to search the list and show the results. For large-screen updates, this can cause lag on the page while everything renders, making typing or other interactions feel slow and unresponsive. Even if the list is not too long, the list items themselves may be complex and different on every keystroke, and there may be no clear way to optimize their rendering.

Conceptually, the issue is that there are two different updates that need to happen. The first update is an urgent update, to change the value of the input field and, potentially, some UI around it. The second is a less urgent update to show the results of the search.

```js
// Urgent: Show what was typed
setInputValue(input);

// Not urgent: Show the results
setSearchQuery(input);
```

The new `startTransition` API solves this issue by giving you the ability to mark updates as “transitions”:

```js
import { startTransition } from 'react';

// Urgent: Show what was typed
setInputValue(input);

// Mark any state updates inside as transitions
startTransition(() => {
  // Transition: Show the results
  setSearchQuery(input);
});
```


## Issue

I recently hit an issue annoyed me so much. which made me to decide to migrate to linux from windows :))

Problem: I tried to run an old react project which using some packages windows needs to install some build tools and so on... The error was:

```js
npm ERR! code 1
npm ERR! path C:\kian\kian-web-mc\kiandigital-web-mc\node_modules\sqlite3
npm ERR! command failed
npm ERR! command C:\Windows\system32\cmd.exe /d /s /c node-pre-gyp install --fallback-to-build
npm ERR! Failed to execute 'C:\Program Files\nodejs\node.exe C:\Users\KIAN\AppData\Roaming\npm\node_modules\npm\node_modules\node-gyp\bin\node-gyp.js configure --fallback-to-build --module=C:\kian\kian-web-mc\kiandigital-web-mc\node_modules\sqlite3\lib\binding\node-v83-win32-x64\node_sqlite3.node --module_name=node_sqlite3 --module_path=C:\kian\kian-web-mc\kiandigital-web-mc\node_modules\sqlite3\lib\binding\node-v83-win32-x64 --napi_version=8 --node_abi_napi=napi --napi_build_version=0 --node_napi_label=node-v83 --msvs_version=2017' (1)       
npm ERR! node-pre-gyp info it worked if it ends with ok
npm ERR! node-pre-gyp info using node-pre-gyp@0.11.0
npm ERR! node-pre-gyp info using node@14.17.1 | win32 | x64
npm ERR! node-pre-gyp WARN Using request for node-pre-gyp https download
npm ERR! node-pre-gyp info check checked for "C:\kian\kian-web-mc\kiandigital-web-mc\node_modules\sqlite3\lib\binding\node-v83-win32-x64\node_sqlite3.node" (not found)
npm ERR! node-pre-gyp http GET https://mapbox-node-binary.s3.amazonaws.com/sqlite3/v4.2.0/node-v83-win32-x64.tar.gz
npm ERR! node-pre-gyp http 403 https://mapbox-node-binary.s3.amazonaws.com/sqlite3/v4.2.0/node-v83-win32-x64.tar.gz
npm ERR! node-pre-gyp WARN Tried to download(403): https://mapbox-node-binary.s3.amazonaws.com/sqlite3/v4.2.0/node-v83-win32-x64.tar.gz
npm ERR! node-pre-gyp WARN Pre-built binaries not found for sqlite3@4.2.0 and node@14.17.1 (node-v83 ABI, unknown) (falling back to source compile with node-gyp)
npm ERR! node-pre-gyp http 403 status code downloading tarball https://mapbox-node-binary.s3.amazonaws.com/sqlite3/v4.2.0/node-v83-win32-x64.tar.gz
npm ERR! gyp info it worked if it ends with ok
npm ERR! gyp info using node-gyp@7.1.2
npm ERR! gyp info using node@14.17.1 | win32 | x64
npm ERR! gyp info ok
npm ERR! gyp info it worked if it ends with ok
npm ERR! gyp info using node-gyp@7.1.2
npm ERR! gyp info using node@14.17.1 | win32 | x64
npm ERR! gyp info find Python using Python version 2.7.15 found at "C:\Users\KIAN\.windows-build-tools\python27\python.exe"
npm ERR! gyp ERR! find VS
npm ERR! gyp ERR! find VS msvs_version was set from command line or npm config
npm ERR! gyp ERR! find VS - looking for Visual Studio version 2017
npm ERR! gyp ERR! find VS VCINSTALLDIR not set, not running in VS Command Prompt
npm ERR! gyp ERR! find VS checking VS2017 (15.9.28307.1525) found at:
npm ERR! gyp ERR! find VS "C:\Program Files (x86)\Microsoft Visual Studio\2017\BuildTools"
npm ERR! gyp ERR! find VS - found "Visual Studio C++ core features"
npm ERR! gyp ERR! find VS - missing any VC++ toolset
npm ERR! gyp ERR! find VS could not find a version of Visual Studio 2017 or newer to use
npm ERR! gyp ERR! find VS looking for Visual Studio 2015
npm ERR! gyp ERR! find VS - not found
npm ERR! gyp ERR! find VS not looking for VS2013 as it is only supported up to Node.js 8
npm ERR! gyp ERR! find VS
npm ERR! gyp ERR! find VS valid versions for msvs_version:
npm ERR! gyp ERR! find VS
npm ERR! gyp ERR! find VS **************************************************************
npm ERR! gyp ERR! find VS You need to install the latest version of Visual Studio
npm ERR! gyp ERR! find VS including the "Desktop development with C++" workload.
npm ERR! gyp ERR! find VS For more information consult the documentation at:
npm ERR! gyp ERR! find VS https://github.com/nodejs/node-gyp#on-windows
npm ERR! gyp ERR! find VS **************************************************************
npm ERR! gyp ERR! find VS
npm ERR! gyp ERR! configure error
npm ERR! gyp ERR! stack Error: Could not find any Visual Studio installation to use
npm ERR! gyp ERR! stack     at VisualStudioFinder.fail (C:\Users\KIAN\AppData\Roaming\npm\node_modules\npm\node_modules\node-gyp\lib\find-visualstudio.js:121:47)
npm ERR! gyp ERR! stack     at C:\Users\KIAN\AppData\Roaming\npm\node_modules\npm\node_modules\node-gyp\lib\find-visualstudio.js:74:16
npm ERR! gyp ERR! stack     at VisualStudioFinder.findVisualStudio2013 (C:\Users\KIAN\AppData\Roaming\npm\node_modules\npm\node_modules\node-gyp\lib\find-visualstudio.js:351:14)      
npm ERR! gyp ERR! stack     at C:\Users\KIAN\AppData\Roaming\npm\node_modules\npm\node_modules\node-gyp\lib\find-visualstudio.js:70:14
npm ERR! gyp ERR! stack     at C:\Users\KIAN\AppData\Roaming\npm\node_modules\npm\node_modules\node-gyp\lib\find-visualstudio.js:372:16
npm ERR! gyp ERR! stack     at C:\Users\KIAN\AppData\Roaming\npm\node_modules\npm\node_modules\node-gyp\lib\util.js:54:7
npm ERR! gyp ERR! stack     at C:\Users\KIAN\AppData\Roaming\npm\node_modules\npm\node_modules\node-gyp\lib\util.js:33:16
npm ERR! gyp ERR! stack     at ChildProcess.exithandler (child_process.js:326:5)
npm ERR! gyp ERR! stack     at ChildProcess.emit (events.js:375:28)
npm ERR! gyp ERR! stack     at maybeClose (internal/child_process.js:1055:16)
npm ERR! gyp ERR! System Windows_NT 10.0.19042
npm ERR! gyp ERR! command "C:\\Program Files\\nodejs\\node.exe" "C:\\Users\\KIAN\\AppData\\Roaming\\npm\\node_modules\\npm\\node_modules\\node-gyp\\bin\\node-gyp.js" "configure" "--fallback-to-build" "--module=C:\\kian\\kian-web-mc\\kiandigital-web-mc\\node_modules\\sqlite3\\lib\\binding\\node-v83-win32-x64\\node_sqlite3.node" "--module_name=node_sqlite3" "--module_path=C:\\kian\\kian-web-mc\\kiandigital-web-mc\\node_modules\\sqlite3\\lib\\binding\\node-v83-win32-x64" "--napi_version=8" "--node_abi_napi=napi" "--napi_build_version=0" "--node_napi_label=node-v83" "--msvs_version=2017"
npm ERR! gyp ERR! cwd C:\kian\kian-web-mc\kiandigital-web-mc\node_modules\sqlite3
npm ERR! gyp ERR! node -v v14.17.1
npm ERR! gyp ERR! node-gyp -v v7.1.2
npm ERR! gyp ERR! not ok
npm ERR! node-pre-gyp ERR! build error
npm ERR! node-pre-gyp ERR! stack Error: Failed to execute 'C:\Program Files\nodejs\node.exe C:\Users\KIAN\AppData\Roaming\npm\node_modules\npm\node_modules\node-gyp\bin\node-gyp.js configure --fallback-to-build --module=C:\kian\kian-web-mc\kiandigital-web-mc\node_modules\sqlite3\lib\binding\node-v83-win32-x64\node_sqlite3.node --module_name=node_sqlite3 --module_path=C:\kian\kian-web-mc\kiandigital-web-mc\node_modules\sqlite3\lib\binding\node-v83-win32-x64 --napi_version=8 --node_abi_napi=napi --napi_build_version=0 --node_napi_label=node-v83 
--msvs_version=2017' (1)
npm ERR! node-pre-gyp ERR! stack     at ChildProcess.<anonymous> (C:\kian\kian-web-mc\kiandigital-web-mc\node_modules\node-pre-gyp\lib\util\compile.js:83:29)
npm ERR! node-pre-gyp ERR! stack     at ChildProcess.emit (events.js:375:28)
npm ERR! node-pre-gyp ERR! stack     at maybeClose (internal/child_process.js:1055:16)
npm ERR! node-pre-gyp ERR! stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:288:5)
npm ERR! node-pre-gyp ERR! System Windows_NT 10.0.19042
npm ERR! node-pre-gyp ERR! command "C:\\Program Files\\nodejs\\node.exe" "C:\\kian\\kian-web-mc\\kiandigital-web-mc\\node_modules\\node-pre-gyp\\bin\\node-pre-gyp" "install" "--fallback-to-build"
npm ERR! node-pre-gyp ERR! cwd C:\kian\kian-web-mc\kiandigital-web-mc\node_modules\sqlite3
npm ERR! node-pre-gyp ERR! node -v v14.17.1
npm ERR! node-pre-gyp ERR! node-pre-gyp -v v0.11.0
npm ERR! node-pre-gyp ERR! not ok
```

SOLUTION:

```js
> 1. `npm i -g rimraf`
> 2. `npm i -D sqlite3`
> 3. `rimraf node_modules`
> 4. `npm i`
`


## Nullish Coalescing Operator in JavaScript

In JavaScript, if a certain value is falsy (like null, undefined, 0, '', NaN), we can use the or (||) conditional to provide a fallback value.

For example, if we have a product page component and we want to display a given product's price, you can use a || conditional to either show the price or show the text "Product is unavailable".

```js
export default function ProductPage({ product }) {    
  return (
     <>
       <ProductDetails />
       <span>{product.price || "Product is unavailable"} // if price is 0, we will see "Product is unavailable"
     </>
  );
}
```

However, there's a small error with our existing code.

If the price has the value of `0`, which is falsy, instead of showing the price itself, we're going to show the text "Product is unavailable" – which is not what we want.

We need a more precise operator to only return the right hand side of our expression if the left hand side is `null` or `undefined` instead of any falsy value.

This is available now in the `nullish coalescing operator`. It will return its right hand operand when its left hand operand is `null` or `undefined`. Otherwise it will return its left hand side operand:

```js
null ?? 'fallback';
// "fallback"

0 ?? 42;
// 0
```

The way to fix our code that we have above is simply to replace the or conditional with the nullish coalescing operator to show the correct price of 0.

```js
export default function ProductPage({ product }) {    
  return (
     <>
       <ProductDetails />
       <span>{product.price ?? "Product is unavailable"}
     </>
  );
}
```


## onChange Example

App.js
```js
import { useState } from "react";
import Input from "./input";
import "./styles.css";

export default function App() {
  const [formValues, setFormValues] = useState({
    username: "",
    email: "",
    password: ""
  });

  function handleSubmit(event) {
    event.preventDefault();
    console.log(formValues);
  }

  function handleChange(event) {
    const { name, value } = event.target;
    setFormValues({ ...formValues, [name]: value });
  }

  return (
    <div className="App">
      <form
        onSubmit={handleSubmit}
        style={{
          display: "flex",
          flexDirection: "column",
          textAlign: "left"
        }}
      >
        <Input name="username" handleChange={handleChange} type="text" />
        <Input name="password" handleChange={handleChange} type="password" />
        <Input name="email" handleChange={handleChange} type="email" />
        <Input type="submit" />
      </form>
    </div>
  );
}
```

input.js
```js
const Input = ({ name, handleChange, type }) => {
  return (
    <div style={{ marginBottom: 16 }}>
      <label htmlFor={name} style={{ marginRight: 8 }}>
        {name}
      </label>
      <input name={name} onChange={handleChange} id={name} type={type} />
    </div>
  );
};

export default Input;
```

## Use prevState to change the state based on previous state

```js
const initialState = {
  count: 0,
  totalCount: 120
}

const [clapState, setClapState] = useState(initialState);

const handleClapClick = () => {
  setClapState(prevState => ({
    count: prevState.count + 1,
    totalCount: prevState.count + 1
  }))
}
```


## useEffect witout dependency

If you write useEffect like this, without any dependency

It's implicitly saying, run this whenever anything changes.

```js
useEffect(() => {
  ...
})
```


## useMemo

you know every time I change a state in a component it's going to re-run all the functions inside of it. so, imagine every time I change the isGreen state, whole the component re-renders and it causes to fib function to re-run every time. And it makes the component slower.

But, now if I use call the `fib` function inside `useMemo` with a dependency, it's going to re-run just when the dependency changes, not in every re-render of the component.

It is amazing yeah :)) So, the comoponent will be fast, even if i change the state for the `isGreen` it will not re-run the function of fib in every re-render.

**This is amazing, but => Do not go and abuse to use it in everywhere. Because, what happens if later I start expecting this to re-render?, why is the fibonacci is not re-rendering? This makes no sense. The states updating, things are happening, why is that not re-rendering. And they have to go and track down => ohhh this person use useMemo here. That's why this is not re-rendering. You run into this new class of bugs, why is my state not changing when I expected to => It just, it's not obvious right?**

So, have a performace problems first, before you solve a performance problems. 

Because, almost always you will solve a wrong problem, you're thinking this is gonna be a problem. But, you're creating a new problem for yourself.

So, only use, useMemo whenever you have a performance problem.

```js
import { useState, useMemo } from "react";

const fibonacci = (n) => {
  if (n <= 1) {
    return 1;
  }

  return fibonacci(n - 1) + fibonacci(n - 2);
};

const UseMemo = () => {
  const [isGreen, setIsGreen] = useState(true);
  const [num, setNum] = useState(26);

  const fib = useMemo(() => fibonacci(num), [num]);

  return (
    <div>
      <h3
        onClick={() => setIsGreen(!isGreen)}
        style={{ color: isGreen ? "green" : "red" }}
      >
        Header Color
      </h3>
      <button onClick={() => setNum(num + 1)}>+</button>
      result: {fib}
    </div>
  );
};

export default UseMemo;
```


## postcss

postcss is like babel, but for css. It takes one set of css and outputs other type of css.


## When and Where to setState

In the case of something **doesn't** involve user interaction, you want to use **useEffect**, if it's something involves user interaction, you're gonna use **useCallback**


- useEffect without dependency Array is going to call every single time whenever the component re-renders.


## Easiest way to setup Husky and lint-staged

`npx mrm@2 lint-staged`


## conditional import

![1688645856492](https://github.com/Arghun-dev/React-Knowledge/assets/53907570/b41dce57-246a-4883-b88c-dfe9f5ad516e)

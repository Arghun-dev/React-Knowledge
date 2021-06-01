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

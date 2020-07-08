# React-Knowledge

### Tip 1

Always remember when submitting a form to set the input states back to null, always setState the inputs to empty string.

```js
this.setState({
  name: '',
  password: ''
})
```


### Tip 2 (Calling 2 different actions from the same onClick event)

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


### Tip3

To remove [] from an array =>>>  ["100", "102"]   =>>> "100", "102"

```js
array.toString()
```


### Tip4

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


### Tip 5

To split a string by comma and setState to an array use this method:

```js
const string = 'arghun, sahand, shahla';

const seperateByComma = string.split(',');

console.log(seperateByComma);

["arghun", " sahand", " shahla"]
```

### Tip 6

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

### Tip 7

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


### Tip 8

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

### Tip 9

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

### Tip 10

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

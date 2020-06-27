# React-Knowledge

### Tip 1

Always remember when submitting a form to set the input states back to null, always setState the inputs to empty string.

```
this.setState({
  name: '',
  password: ''
})
```

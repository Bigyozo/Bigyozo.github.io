# 高阶函数实现 React 组件事件处理函数复用

通过高阶函数实现了仅使用 editForm 一个函数完成对多个属性的赋值

```jsx
class Login extends React.Component {
  state = {
    username: "",
    password: "",
  };

  render() {
    return (
      <form>
        Username: <input type="text" onChange={this.editForm("username")} />
        <br />
        Password: <input type="password" onChange={this.editForm("password")} />
        <br />
        <button>Login In</button>
      </form>
    );
  }

  editForm = (type) => {
    return (event) => this.setState({ [type]: event.target.value });
  };
}
```

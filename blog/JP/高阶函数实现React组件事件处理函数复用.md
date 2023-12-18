# 高階関数による React のイベントメソッドの再利用

高階関数を利用して、editForm 関数だけで複数のプロパティの値設定を実現

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

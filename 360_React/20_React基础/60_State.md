

# 八、this.state

React 把组件看成是一个状态机（State Machines）。
通过与用户的交互，实现不同状态，
然后渲染 UI，让用户界面和数据保持一致。 

React 里，只需更新组件的 state，然后根据新的 state 重新渲染用户界面（不要操作 DOM）。

由于 this.props 和 this.state 都用于描述组件的特性，可能会产生混淆。
一个简单的区分方法是，this.props 表示那些一旦定义，就不再改变的特性，
而 this.state 是会随着用户互动而产生变化的特性。


# 示例

```
<!DOCTYPE html>
<html>
  <head>
    <script src="../build/react.js"></script>
    <script src="../build/react-dom.js"></script>
    <script src="../build/browser.min.js"></script>
  </head>
  <body>
    <div id="example"></div>
    <script type="text/babel">
var LikeButton = React.createClass({
  getInitialState: function() {
    return {liked: false};
  },
  handleClick: function(event) {
    this.setState({liked: !this.state.liked});
  },
  render: function() {
    var text = this.state.liked ? 'like' : 'haven\'t liked';
    return (
      <p onClick={this.handleClick}>
        You {text} this. Click to toggle.
      </p>
    );
  }
});
ReactDOM.render(
  <LikeButton />,
  document.getElementById('example')
);
    </script>
  </body>
</html>
```

上面代码是一个 LikeButton 组件，
它的 getInitialState 方法用于定义初始状态，也就是一个对象，这个对象可以通过 this.state 属性读取。
当用户点击组件，导致状态变化，this.setState 方法就修改状态值，
每次修改以后，自动调用 this.render 方法，再次渲染组件。


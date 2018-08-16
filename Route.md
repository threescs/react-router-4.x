<Route>

Route 它最基本的职责就是当页面访问地址与Route上的path匹配时,就渲染出对应的ui界面
它自带 三个method 和 三个props

render methods 分别是：

<Route component>
<Route render>
<Route children>

props 分别是：

match
location
history
----------------------------------------------------------------------------------------------------------------------
component
只有当访问地址和路由匹配时，一个 React component 才会被渲染，此时此组件接受 route props (match, location, history)。
当使用 component 时，router 将使用 React.createElement 根据给定的 component 创建一个新的 React 元素。这意味着如果你使用内联函数（inline function）传值给 component 将会产生不必要的重复装载。对于内联渲染（inline rendering）, 建议使用 render prop。

<Route path="/user/:username" component={User} />
const User = ({ match }) => {
  return <h1>Hello {match.params.username}!</h1>
}
----------------------------------------------------------------------------------------------------------------------
render: func
此方法适用于内联渲染，而且不会产生上文说的重复装载问题。
// 内联渲染
<Route path="/home" render={() => <h1>Home</h1} />

// 包装 组合
const FadingRoute = ({ component: Component, ...rest }) => (
  <Route {...rest} render={props => (
    <FadeIn>
      <Component {...props} />
    </FaseIn>
  )} />
)

<FadingRoute path="/cool" component={Something} />
----------------------------------------------------------------------------------------------------------------------
children: func
有时候你可能只想知道访问地址是否被匹配，然后改变下别的东西，而不仅仅是对应的页面。
<ul>
  <ListItemLink to="/somewhere" />
  <ListItemLink to="/somewhere-ele" />
</ul>

const ListItemLink = ({ to, ...rest }) => (
  <Route path={to} children={({ match }) => (
    <li className={match ? 'active' : ''}>
      <Link to={to} {...rest} />
    </li>
  )}
)
----------------------------------------------------------------------------------------------------------------------
path: string
任何可以被 path-to-regexp解析的有效 URL 路径
----------------------------------------------------------------------------------------------------------------------
exact: bool
如果为 true，path 为 '/one' 的路由将不能匹配 '/one/two'，反之，亦然。
----------------------------------------------------------------------------------------------------------------------
strict: bool
对路径末尾斜杠的匹配。如果为 true。path 为 '/one/' 将不能匹配 '/one' 但可以匹配 '/one/two'。

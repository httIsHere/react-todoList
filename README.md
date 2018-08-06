> create-react-app创建react项目：<br>
npm install -g create-react-app <br>
create-react-app xxx(项目名称) <br>

package.json->脚手架配置文件；

//https协议的服务器 PWA
import registerServiceWorker from './registerServiceWorker';

> 属性校验 propTypes <br>
属性默认值 defaultProps

props、state、render：<br>
页面加载时render自动执行，props或者state发生改变，render函数重新被执行；<br>
父组件的render被执行，子组件的render也会被执行；

虚拟DOM：在数据发生改变时生成新的DOM，并于原始DOM对比，替换更新的部分；<br>
diff算法：原始虚拟DOM和新虚拟DOM的差异；

ajax请求：异步请求获取数据；<br>
使用模块：axios

react 动画：<br>
css过渡动画 -> react-transation-group

#### Redux（= Reducer + Flux）：
> 所有组件共享store，通过操作store进行组件更新，无需传递参数；
React Components：页面上的所有组件；<br>

Store：数据的公共区域；

1、创建store：<br>
```
import { createStore } from 'redux';
const store = createStore();
```
2、创建记录本（所有数据，接收到上一次的state和操作，在reducer内进行过操作返回新的state）：<br>
```
const defaultState = {}
export default (state = defaultState, action) => {
    return state;
}
```
在reducer内进行操作时不能直接操作state，需要先深度拷贝state在进行操作；<br>
```
const newState = JSON.parse(JSON.stringify(state));
```
3、告知数据：<br>
```
import reducer from './reducer'
const store = createStore(reducer);
```
4、获得数据：<br>
```
store.getState();
```
5、在页面内对store的变化进行监听：<br>
```
//对store进行订阅
store.subscribe(this.handleStoreChange);
```
在定义action的type时方便查错可以将其提取出作为常量进行引用；<br>
6、`actionCreator` 同一管理action：<br>
将需要使用到的action全部写在一个文件，在需要使用的页面进行引入；<br>
PS：想要在chrome使用redux查看工具，需要在创建store时传入参数`window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()`
即：
```
const store = createStore(
    reducer,
    window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
);
```
> redux重要点：<br>
1）state是唯一的，只用自己能操作自己，其他不能直接操作state；<br>
2）reducer必须要是纯函数（给定固定输入，一定有固定输出，且不会有任何副作用，输入和输出一一映射，不会对其他数据产生影响）；<br>
3）createStore - store.dispatch（派发） - store.getState - store.subscribe（订阅）;<br>

7、redux-thunk中间件：<br>
引入`applyMiddleware`，在创建store时使用`applyMiddleWare`引入需要的中间件；<br>
```
const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ? window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({}) : compose;
const enhancer = composeEnhancers(
    applyMiddleware(ReduxThunk)
);
const store = createStore(reducer, enhancer);
```
创建一个action，返回一个异步调用的函数，该函数可接收dispatch参数（只有只用使用thunk，action才可以返回一个函数）；<br>

##### redux中间件：
action和store之间，相当于对dispatch方法的封装升级，dispatch的参数可以是函数，如果参数是对象则直接传给store，如果参数是函数则操作函数，如果有需要对store进行操作再进行操作。<br>
8、`redux-saga`中间件：
sagas.js也可以接收到action；<br>
在容器组件派发action，sagas函数接收该action执行文件内相应的异步请求方法，使用saga的put；<br>


> react-redux的使用：<br>
1、安装react-redux；<br>
2、创建store文件：<br>
`
import { createStore } from 'redux';
import reducer from './reducer'
const store = createStore(reducer);
`<br>
3、创建reducer文件，并传入store：<br>
`
export default (state, action) => {
return state;
}
`<br>
4、在容器组件内引入store，并初始化容器的state；<br>
5、Provider组件，用于连接组件，将Provider与store做连接，Provider内部组件都能使用store的内容，在组件内部使用connect将容器组件与store做连接；<br>
在页面内渲染数据就可以用this.props

UI组件和容器组件：<br>
UI组件负责页面渲染，容器组件负责逻辑；<br>
在绑定函数需要传入参数时不能直接传入，可以采用箭头函数的方法：
`onClick={(index) => (this.props.handleDelete(index))}`

无状态组件：当一个组件只有render函数可以用无状态组件代替；<br>
无状态组件只是一个函数而非类，没有生命周期，所以性能比普通组件高；<br>

react ui框架：antd

--------------------------------------------------------------------------------------
> 生命周期：<br>
getDefaultProps<br>
getInitialState<br>
componentWillMount<br>
render<br>
componentDidMount：在组件挂载到页面上，放置接口请求比较合适；<br>
componentWillUnmount<br>
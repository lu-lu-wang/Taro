# 说明

## Taro包组成

### NPM包

NPM包 | 描述
---- | ----
@tarojs/taro | Taro运行时框架
@tarojs/taro-h5 | Taro H5运行时框架
@tarojs/taro-rn | Taro React-Native运行时框架
@tarojs/taro-weapp | Taro 微信小程序运行时框架
@tarojs/taro-swan | Taro百度智能小程序运行时框架
@tarojs/taro-tt | Taro字节跳动小程序运行时框架
@tarojs/taro-alipay | Taro支付宝小程序运行时框架
@tarojs/redux | Taro小程序Redux支持
@tarojs/redux-h5 | Taro H5 Redux支持
@tarojs/redux-rn | Taro React Native Redux支持
@tarojs/mobx-common | Taro Mobx 公共模块
@tarojs/mobx | Taro 小程序Mobx支持
@tarojs/mobx-h5 | Taro H5 Mobx支持
@tarojs/mobx-rn | Taro React Native Mobx支持
@tarojs/router | Taro H5路由
@tarojs/async-await | 支持使用async/await语法
@tarojs/cli | Taro开发工具
@tarojs/transformer-wx | Taro小程序转换器
@tarojs/taroize | Taro小程序编译器
@tarojs/taro-rn-runner | Taro React Native打包编译工具
@tarojs/webpack-runner | Taro H5端Webpack打包编译工具
@tarojs/components | Taro标准组件库，H5版
@tarojs/components-rn | Taro标准组件库，React Native版
@tarojs/plugin-babel | Taro Babel编译插件
@tarojs/plugin-sass | Taro Sass 编译插件
@tarojs/plugin-less | Taro Less 编译插件
@tarojs/plugin-stylus | Taro Stylus编译插件
@tarojs/plugin-csso | Taro css压缩插件
@tarojs/plugin-uglifyjs | Taro JS压缩插件
@tarojs/config-taro | Taro ESLint规则
eslint-plugin-taro | Taro ESLint插件

###项目配置

####各类小程序平台均有自己的项目配置文件，例如

* 微信小程序，project.config.json
* 百度只能小程序，project.sawn.json
* 头条小程序，project.config.json大部分字段与微信小程序一致
* 支付宝小程序暂无

## 路由跳转

### 配置下入口文件的config即可
#### 跳转

新页面打开 `Taro.navigateTo({url: '/pages/my/index'})`

当前页面打开 `Taro.redirectTo({url: ‘/page/orderPay/index’})`

### 路由传参
我们可以通过在所有跳转url后面添加查询字符串参数进行跳转传参

```
	// 传入参数 id=2&type=test  
	Taro.navigateTo({ url: ‘/pages/orderPay/index?id=2&type=test’})
	
	// 在跳转成功的目标页的生命周期方法里通过this.$router.params获取到传入的参数，
	class C extends Taro.Component {
		componentWillMount () {
			console.log(this.$router.params) // 输出{id: ‘2’,type:’test’}
		}
	}
	
	
```
## 事件处理
###阻止事件冒泡
`e.stopPropagation()`
###向事件处理程序传递参数
通常我们会为事件处理程序传递额外的参数

`<button onClick={this.delate.bind(this,id)}>delete</button>`

当你通过bind方式向监听函数传参，在类组件中定义的监听函数，事件对象e要排在所传递参数的后面。

```
	class Popper extends Component {
		constructor(){
			super(...arguments)
			this.state = {name: ‘hello Tarojs!’}
		}
		preventPop(name, test, e){
			e.stopProPagation()
		}
		render () {
			return 
			<Button 
				onClick={this.preventPop.bind(this,this.state.name,’test’)}> 				delete
			</Button>
		}
	}
```
### 使用匿名函数
> **注意：在各个小程序端，使用匿名函数，尤其是在循环中使用匿名函数，比使用bind进行事件传参占用更大的内存，速度也会更慢。除了bind之外，事件参数也可以使用匿名函数进行传参。直接写匿名函数不会打乱原有监听函数的参数顺序：**

``` 
	class Popper extends Component {
		constructor(){
			super(...arguments)
			this.state = {name: ‘hello Tarojs!’}
		}
		render () {
			const name = ‘test’
			return 
			<Button 
				onClick={(e)=>{e.stopPropagation; this.setState({name})}}			>{this.state.name}
			</Button>
		}
	}
```
###任何组件的事件传递都要以on开头
## 条件渲染（按需渲染）
###元素变量
你可以使用变量来存储元素。它可以帮助你有条件的渲染组件的一部分，而输出的其他部分不会改变。

```
	class LoginStatus extends Component {
		render () {
			const isLoggedIn = this.props.isLoggedIn
			let status = null;// 避免报错
			if(isLoggedIn){
				status = <Text>已登录</Text>
			} else {
				status = <Text>未登录</Text>
			}
			return (
				<View>{status}</View>
			)
		}
	}
```
### 逻辑运算符 &&
你可以通过用花括号包裹代码在JSX中嵌入几乎任何表达式，也包括js的逻辑与&&，它可以方便地条件渲染一个元素。

```
	class LoginStatus extends Component {
		render () {
			const isLoggedIn = this.props.isLoggedIn
			return (
				<View>
					{isLoggedIn && <Text>已登录</Text>}
					{!isLoggedIn && <Text>未登录</Text>}
				</View>
			)
		}
	}
```
**其逻辑算法均和React一致**
***
## 列表渲染
### map（）
得到一个数组
### 渲染多个组件
下面我们使用js中的map()方法遍历numbers数组。对数组中的每个元素返回<Text>标签，最后我们得到一个数组listItems:

```
	const numbers = [...Array(100).keys()] // [0,1,2,...,99]
	const listItems = numbers.map(item=>{
		return <Text>第 { item + 1 } 个</Text>
	})
```
### Keys
当循环数组时需要提供一个keys，keys可以在DOM中的某些元素被增加或删除时帮助程序识别那些元素发生了变化。即key就是数组中每个元素的标识。

```
	const numbers = [...Array(100).keys()] // [0,1,2,...,99]
	const listItem = numbers.map(item=>{
		return <Text key={String(item)}>{ item + 1 }</Text>
	})
```
### key的取值

1. 稳定
2. 可预测
3. 唯一

### 与React的不同
在React中，JSX是会编译成普通的JS执行，每个JSX元素，其实会通过createElement函数创建成一个js对象，因此实际上你可以这样写代码react也是完全能渲染的。

```
	const list = this.state.list.map(l=>{
		if(l.selected){
			return <li>{l.text}</li>
		}
	}).filter(React.isValidElement)
	
```
在Taro中，JSX会编译成微信小程序模板字符串，因此你不能把map函数生成的模板当做一个数组来处理，当你要这样处理时，应先处理需要循环的数组，再用处理好的数组来调用map函数。

```
	const list = this.state.list
		.filter(l=>l.selected)
		.map(l=>{
			return <li>{l.text}</li>
		})
```
## Children与组合
> **经测试，由于无法再微信小程序中使用<slot />因此Children和组合在微信小程序中也无法在循环中使用。百度小程序、支付宝小程序、H5、RN都可以在循环中使用此功能**

###Children
当我们设计组件时，有些组件是不知道自己的子组件会有什么内容，这种情况可以使用this.props.children来传递子元素

```js 
	class Dialog extends Component {
		render () {
			return (
				<View>
					<View>Welcome!</View>
					<View>{this.props.children}</View>
				</View>
			)
		}
	}
```
**注意事项：** 请不要对this.props.children进行任何操作。Taro在小程序中实现这个功能使用的是小程序的slot功能，也就是说你可以把this.props.children理解为slot的语法糖，this.props.children在Taro中并不是React的ReactElement对象
###组合
有些时候我们不仅仅需要传递一个子组件，可能会需要很多个占位符。例如下面这个Dialog组件中，你不仅需要自定义它的body，你希望它的header和footer都是给Dialog组件的使用者自由定制。

```
	class Dialog extends Component {
		render () {
			return (
				<View>
					<View>{this.props.renderHeader}</View>
					<View>{this.props.children}</View>
					<View>{this.props.renderFooter}</View>
				</View>
			)
		}
	}
	
	class App extends Component {
		render (){
			<View>
				<Dialog
					renderHeader={
						<View>header</View>
					}
					renderFooter={
						<View>footer</View>
					}
				>
					<View>children</View>
				</Dialog>
			</View>
		}
	
	}
```
在这里声明Dialog组件时，header和footer部分我们分别增加了this.props.renderHeader和this.props.renderFooter作为占位符。相应的我们在使用组件时，可以给renderHeader和renderFooter传入JSX元素，这两个分别传入的JSX元素将会填充它们在Dialog组件中的位置--就像Dialog JSX标签里写入的内容，会填充到this.props.children的位置一样。

###**注意事项**
**组件的组合需要遵守this.props.children的所有规则**组合这个功能和this.props.children一样是通过slot实现的，也就是说this.props.children的限制对于组件组合也同样适用。所有组合需用render开头，且遵循驼峰式命名法。

## Refs引用
> Refs提供了一种访问在render方法中创建的DOM节点，在常规Taro数据流中，props是父组件与子组件交互的唯一方式。要修改子元素，你需要用新的props去重新渲染子元素。然而，在少数情况下，你需要在常规数据流外强制修改子元素。被修改的子元素可以是Taro组件实例，或者是一个DOM元素。

###不要过度使用Refs
你可能首先想到在你的应用程序中使用refs来更新组件。
### 创建Refs
Taro支持使用字符串和函数两种方式创建ref
###使用字符串创建ref
通过字符串创建ref只需要把一个字符串的名称赋给```ref```prop。然后就通过this.refs访问到被定义的组件实例或DOM元素。在微信小程序中，如果ref的是小程序原生组件，那么相当于使用createSelectorQuery取到小程序原生组件，如果是在H5环境中，访问到的是@tarojs/components对应组件的实例。

```
	class MyComponent extends Component {
		componentDidMount(){
			// 如果ref的是小程序原生组件，那只有在didMount生命周期之后才能通过this.refs.input访问到小程序原生组件
			if(process.env.TARO_ENV === ‘weapp’){
				// 这里this.refs.input访问的时候通过wx.createSelectQuery取到小程序原生组件
			} else {
				// 这里this.refs.input访问到的是@tarojs/components的Input组件实例
			}
		}
		render () {
			return <Input ref=‘input’/>
		}
	}
```
###通过函数创建ref
你也可以通过传递一个函数创建ref，在函数中被引用的组件会作为函数的第一个参数传递，如果被引用的组件是自定义组件，那么可以在任何生命周期访问引用。

*不管在任何情况下，Taro都推荐使用函数的方式创建ref*

```
	class MyComponent extends Component {
		roar(){
			this.cat.miao()
		}
		
		refCat = (node)=>this.cat = node // this.cat会变成cat组件实例的应用
		render(){
			return <Cat ref={this.refCat}/>
		}
	}
	class Cat extends Component {
		miao(){
			console.log(‘miao miao~~’)
		}
		render(){
			return <View />
		}
	}
	
```
##跨平台开发
Taro的设计初衷就是为了统一跨平台的开发方式，并且已经尽力通过运行时框架、组件、API去抹平多端差异。
###内置环境变量
> 注意：环境变量在代码中的使用方式

**process.env.TARO_ENV**用于判断当前编译类型，目前有weapp/swan/alipay/h5/rn/tt六个值，可以通过这个变量来书写对应一些不同环境下的代码，在编译时会将不属于当前编译类型的代码去掉，只保留当前编译类型下的代码。

```
	if(process.env.TARO_ENV === ‘weapp’){
		require(‘path/to/weapp/name’)
	} else if(process.env.TARO_ENV ===‘h5’){
		require(‘path/to/h5/name’)
	}
```
同时也可以在JSX中使用，决定不同端要加载的组件

```
	render () {
		return (
			<View>
				{process.env.TARO_ENV === ‘weapp’ && <ScrollViewWeapp/>}
				{process.env.TARO_ENV === ‘h5’ && <ScrollViewH5/>}
			</View>
		)
	}
```
###统一接口的多端文件












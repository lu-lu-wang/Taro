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
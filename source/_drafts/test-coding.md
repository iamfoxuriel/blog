---
title: test-coding
date: 2017-09-12 16:36:11
tags:
clearReading: true
thumbnailImage: unsplash2.jpg
thumbnailImagePosition: bottom
autoThumbnailImage: yes
metaAlignment: center
coverCaption: "A beautiful sunrise"
coverMeta: out
coverSize: full
coverImage: unsplash2.jpg
---
#Small Business Back Office  
_[Switch English version](./README.md)_  
>* 构建工具为webpack, 允许库为ReactJs Redux  
>* 代码风格为ES6, 打包时候经Babel转换  
>* 用ESLint作为代码风格检查, 目前使用eslint-config-airbnb规则, 并去掉少许  
>* 使用express作为模拟API, 这样开发前端时可以不必有服务端依赖

 <!-- excerpt -->
 {% blockquote David Levithan, Wide Awake http://rabears.com  rabears.com%}
 Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque hendrerit lacus ut purus iaculis feugiat. Sed nec tempor elit, quis aliquam neque. Curabitur sed diam eget dolor fermentum semper at eu lorem.
 {% endblockquote %}
 
 {% alert success %}
 success
 {% endalert %} 
 
 {% alert danger no-icon %}
 Here is a danger alert without icon
 {% endalert %}
 
 
{% tabbed_codeblock example http://example.com%}
   <!-- tab js -->
       var test = 'test';
   <!-- endtab -->
   <!-- tab css -->
       .btn {
           color: red;
       }
   <!-- endtab -->
{% endtabbed_codeblock %}
    
    
## 如何构建项目
### 开发环境准备
node version >=4.3.0 <5.0.0 || >=5.10  
平时习惯使用的JavascriptIDE

###构建命令
1. `npm install`  
2. build `npm run build` or `npm run build-windows`  
3. build dll `npm run build:dll` and `npm start`  (目前 npm start 有可能会遇到mockapi oom, 建议npm run mock-api 和npm run dev 分别启动)  
4. test`npm test` or `npm run test-watch`   
5. to commit change `npm run precommit` or `npm run precommit-windows`   
test single file`npm test -- baseAPIService.spec.js`   
update snapshot`npm test -- -u`   

##ReactJS
[官方网站](https://facebook.github.io/react/)  
[函数式Component](https://github.com/kriasoft/react-starter-kit/blob/master/docs/react-style-guide.md#prefer-using-functional-components)      
```
class Navigation extends Component {  
  static propTypes = { items: PropTypes.array.isRequired };  
  render() {  
    return <nav><ul>{this.props.items.map(x => <li>{x.text}</li>)}</ul></nav>;  
  }  
}  
  
// Better  
function Navigation({ items }) {  
  return (  
    <nav><ul>{items.map(x => <li>{x.text}</li>)}</ul></nav>;  
  );  
}  
Navigation.propTypes = { items: PropTypes.array.isRequired };  
```

###VirtualDom

###RectRouter
[官方网站](https://reacttraining.com/react-router/)   
目前使用3.x 版本


##Redux
[中文文档](http://cn.redux.js.org/) 
###Store
任何时候Store中保存完整的State树
###Reducer
reducer里面是纯函数, 必须返回immutable对象.  
对于有限层级的对象可以使用Object.assign().
###Action Creator
export 创建Action object的函数以及Action name常量  
###middleWare

###Dumb Component && Smart Component
**只通过porps处理外部响应事件, 没有依赖方法, 与状态隔绝**  
Smart Component, 我们赋予State管理的能力  
Dumb Component, 只允许管理组件自身状态, 不允许修改props


##Babel
###配置文件
传送门: [.babelrc](./.babelrc)  
插件 https://github.com/gaearon/react-transform-hmr  


##Mock-api
用express构建在本地环境 mock api

##ESLint
>ESLint是目标是提供一个插件化的javascript代码检测工具.  
项目将其作为静态代码对于编码风格的检测工具.
###配置文件
延伸信息 [ESLint rules](http://eslint.cn/docs/rules/)  
传送门: [.eslintrc](./.eslintrc)  
config code: 0 = 关闭, 1 = 警告, 2 = 报错

###IntelliJ/Webstorm 配置 Javascript styles
>Settings 路径: `Editor->Code Style-> JavaScript`  
`Tab and Indents` 设置成 4  
`Spaces`  enable ES6 `import/export braces`  
如果你是使用IntelliJ/Webstorm, 只要在IDE中使用EditorConfig插件即可 

##Styles
###全局样式
通过index.js引入index.less
###模块级别的样式
通过在dumb组件import相应的样式, 以做到模块之间的样式隔离
```javascript
import styles from './Module.lss';  
export default Module extends Component {  
    render() {  
        return (  
            <div className={styles['module-container']+' clearfix'>  
                Test  
            </div>  
        )  
    }  
}  
```
##代码结构
```
.
├── app                             #开发目录
|   |   
|   ├──config                       #配置文件
|   |   
|   ├──moudles                      #容器组件,主页
|   |   |   
|   |   ├──main                     #Main 
|   |   |
|   |   |——module1                  #路由文件，容器组件
|   |   |   |
|   |   |   |——action-creator       #action创建器
|   |   |   |
|   |   |   |——components           #Dumb组件
|   |   |   |
|   |   |   |——containers           #Smart组件
|   |   |   |
|   |   |   |——styles               #模块独立样式
|   |   |   |
|   |   |   |——reducers             #reducers文件
|   |   |   |
|   |   |   |——module-info.js       #模块定义文件
|   |   |
|   |   |——module2
|   |   |
|   |   ...
|   |    
|   └──index.js                     #入口文件
|      
├── build                           #发布目录
├── test                            #测试目录
├── node_modules                    #包文件夹
├── .editorconfig     
├── .gitignore     
├── .babelrc     
├── .eslintrc      
├── webpack.config.js               #webpack配置文件
├── package.json                    #环境配置
```


##Writing Test
###Component test
DOM Testing  
https://facebook.github.io/jest/docs/tutorial-react.html  
Snapshot Testing  
https://facebook.github.io/jest/blog/2016/07/27/jest-14.html

###Reducer test
纯函数测试, 定义好入参, 断言返回

###Action creator test
判断在方法调用之后能够dispatch相应的action

##Tips
- RHL@1.x 会导致 @autobind 不可用，所以采用 2.x
- 除了 decorator,还可以使用操作符 `::` 来实现 autobind,参见：http://babeljs.io/blog/2015/05/14/function-bind/
- 开发时不应使用 extract-text-webpack-plugin, 因为它让 CSS 无法热替换
- 开发时应尽量通过`className={style.className}`来设定类名, 使得CSS可以模块化
- Object.assign 与 Array.slice 都不是深拷贝
- 错误的统一处理。错误的统一处理建议采用`middleware`的方式处理。
- 依赖版本检查`npm  outdated`

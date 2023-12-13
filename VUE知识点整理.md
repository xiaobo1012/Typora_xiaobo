# VUE知识点整理

### vue脚手架的安装

> 1. 全局安装@vue/lic	`npm install -g @vue/cli`
> 2. 到创建的目录里面开始框架的创建  `vue create [项目名称]`



### 内置指令语法

#### v-bind

> 单向数据绑定，数据只能从data流向页面

- v-bind 可以简写为 “ ：”，用于去绑定data中的数据
- 主要用于html与data中的**动态数据**的绑定

<img src="https://cdn.jsdelivr.net/gh/xiaobo1012/imgPicGo/imgs/202312121117999.png" alt="img" style="zoom: 67%;" />



#### v-model

> 双向数据绑定，可以将html标签的value值与data中的某个值双向绑定，由于mvvm模型，实现了数据与视图的改变

<img src="https://cdn.jsdelivr.net/gh/xiaobo1012/imgPicGo/imgs/202312121122402.png" alt="img" style="zoom: 67%;" />

- 这种写法是错误的，因为v-model只能用于绑定value值，由于h2没有value值则会报错



#### v-if  / v-else

- 与 if 和else的用法相同，并且可以用于控制元素是否存在（被渲染出来），if为false时页面不会加载html的结构



#### v-for

- ![img](https://cdn.jsdelivr.net/gh/xiaobo1012/imgPicGo/imgs/202312121133675.png)

![img](https://cdn.jsdelivr.net/gh/xiaobo1012/imgPicGo/imgs/202312121130601.png)persons可以是任何可便利的数据，例如数组，对象



- 需要添加一个 :key=’xxx‘  ,在虚拟dom渲染的时候可以快速找到对应的数据进行判断
- key必须是**唯一值**

<img src="https://cdn.jsdelivr.net/gh/xiaobo1012/imgPicGo/imgs/202312121131068.png" alt="img" style="zoom: 67%;" />



#### 其他不常用的指令

- v-text
- v-html
- v-cloak
- v-once
- v-pre



###  自定义指令





### 路由（Vue-router）

#### 路由的创建

> 1. 安装vue-router 	
>    1. [vue3—安装 router4  vue2—安装 router3]
>    2. `npm i vue-router@3`
> 2. 在main.js中引入和应用路由



#### 路由跳转

```js
this.$router.push({
	// 跳转页面的name（因为这里使用了params所以不能使用path进行跳转）
	name: "search",
	// 这里要看有没有query参数（也就是下拉列表是否被选中了），如果有的话就要在搜索的时候把query参数一起带上（这里有没有都带上了，淦）
	query: this.$route.query,
	params: {
		keyword: this.keyWord || undefined || ""
    },
});
```


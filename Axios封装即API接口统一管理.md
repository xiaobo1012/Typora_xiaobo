# axios的安装

[axios请求流程图](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/38a6dc88edd149c6ab5e9c08b15a3416~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

##安装axios

``` js
npm install axios;
```



## 创建文件夹用于保存对应代码

- 项目的src目录中，新建一个utils文件夹，然后在里面新建一个request.js用于封装axios

【注】：utils文件夹保存的是自己定义的第三方的工具



## 封装axios

- 切换不同的环境【就是在不同的服务器中去获取数据】
  - 开发环境
  - 测试环境
  - 生产环境

```js
import axios from 'axios'	//引入axios


// 创建一个axios实例
const requestjs = axios.create({
  baseURL: process.env.VUE_APP_BASE_API, // url = base url + request url
  // withCredentials: true, // send cookies when cross-domain requests
  timeout: 5000 // request timeout
})

// 设置请求拦截器
requestjs.interceptors.request.use(
  config => {
    // do something before request is sent
	// [请求拦截器主题代码] 
    config.headers['token'] = getToken   // 可以设置请求头
    return config
  },
  error => {
    // do something with request error
    console.log(error) // for debug
    return Promise.reject(error)
  }
)

// 设置响应拦截器
requestjs.interceptors.response.use(
  response => {
    const res = response.data

    // if the custom code is not 20000, it is judged as an error.
    if (res.code !== 20000 && res.code !== 200) {
      Message({
        message: res.message || 'Error',
        type: 'error',
        duration: 5 * 1000
      })

      // 50008: Illegal token; 50012: Other clients logged in; 50014: Token expired;
      if (res.code === 50008 || res.code === 50012 || res.code === 50014) {
        // to re-login
        MessageBox.confirm('You have been logged out, you can cancel to stay on this page, or log in again', 'Confirm logout', {
          confirmButtonText: 'Re-Login',
          cancelButtonText: 'Cancel',
          type: 'warning'
        }).then(() => {
          store.dispatch('user/resetToken').then(() => {
            location.reload()
          })
        })
      }
      return Promise.reject(new Error(res.message || 'Error'))
    } else {
      return res
    }
  },
    error => {
    console.log('err' + error) // for debug
    Message({
      message: error.message,
      type: 'error',
      duration: 5 * 1000
    })
    return Promise.reject(error)
  }
)    

export default requestjs  // 将封装好的对象暴露出去
```





# API接口的统一管理

- 创建一个api文件夹用于保存api接口代码
- 将不同组件的请求接口分开存放，便于区分

## 引入封装好的axios文件request.js

```js
import requestjs from '@/xxxxx/request'	


// 暴露出去写好的接口
export function login(data,token) {
  return requestjs({
    url: '/admin/acl/index/login',
    method: 'post',
    data:data,
    params: { token }
  })
}
```



## 将不同组件的接口统一暴露出去并绑定到Vue原型上

```js
// 先引入模块
import * as tradeMark from './product/tradeMark'
import * as attr from './product/attr'
import * as sku from './product/sku'
import * as spu from './product/spu'

// 对外暴露出去
export default {
  tradeMark,
  attr,
  sku,
  spu
}
```

 在main.js中绑定

```js
// 将api接口绑定到原型上
import api from '@/api/index'

Vue.prototype.$API = api
```

这样可以在**任何组件**中采用`this.$API.xxxx`进行接口的访问





# 跨域代理

### 基本配置

```js
// 开启代理服务器
devServer:{
    proxy:{
        // /api——>请求前缀，自行定义
        // 浏览器在精选数据请求的时候，如果有这个前缀则说明需要代理跨域，没有这个前缀则说明请求的是本地数据
        '/api':{
            target:'http://xxxxxxxxxx',	//这里是目标服务器的地址
            //路径重写（将/api的前缀替换为空，因为这个前缀只是伪类区别是否进行代理跨域，服务器里面的文件是没有这个前				缀的，没有替换前缀服务器就找不到对应的文件）
            pathRewrite:{'^/api':''},	
            //ws:true,	// 用于支持websocket
            //changeOrigin:true	// 用户控制请求头中的host值（为true时，host就和目标服务器的host相同）
        },
        '/xxxx': {   // 这里是第二个代理跨域的请求
        	target: 'http://gmall-h5-api.atguigu.cn',
        	pathRewrite: { '^/xxxx': '' }
     	 }
    }
}
```


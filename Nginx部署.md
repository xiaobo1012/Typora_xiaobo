> nginx部署前端服务器，这样浏览器就可以访问你这个**前端服务器**来获取页面，前端服务器到后端服务器获取数据



## [nginx服务器概念](https://baike.baidu.com/item/nginx/3817705?fr=ge_ala)

- NGINX 是一个开源的 Web 服务器和反向代理服务器，它使用 Nginx 作为 Web 服务器和反向代理服务器的原因是它拥有高性能、可扩展性和可靠性。它可以处理大量的并发连接，并且可以缓存 HTTP 请求以提高性能。
  - 我们前端提供的是静态资源（需要去后端拿去数据才是动态展示的），nginx在处理静态资源时非常高效
- 可以实现反向代理
- 具有非常优秀的并发性能



## 文件说明

1. conf——存放nginx的配置
2. html——存放打包后的静态资源





## 安装

1. 下载nignx
2. 下载完成后，解压缩，运行cmd，使用命令进行操作，**不要直接双击nginx.exe**，一定要在dos窗口启动，不要直接双击nginx.exe，这样会导致修改配置后重启、停止nginx无效，需要手动关闭任务管理器内的所有nginx进程，再启动才可以。
3. 启动nginx服务，启动时会一闪而过是正常的

```js
start nginx
```

4. 查看任务进程是否存在，dos或打开任务管理器都行

```js
tasklist /fi "imagename eq nginx.exe"
```

5. 安装成功后可以在浏览器访问**127.0.0.1**进行服务器访问



## 部署项目

1. 将项目打包`npm run build`（具体按照package.json里面的配置来打包）
2. 将dist里面的文件**复制**到nginx目录下的**html**文件夹中

3. 启动nginx

```js
start nginx
```

3. 访问浏览器地址

```JS
127.0.0.1:80	// 80为默认端口，可以省略
```



## 配置nginx反向代理

> 前端获取接口数据的两种方式：
>
> 1. 直接访问后端服务第接口地址（前提：后端必须配置跨域）
> 2. 前端先访问nginx服务器，检测到路径上的指定符号，通过**nginix反向代理**，从而将请求转发到后端服务器

1. 找到nginx中conf文件夹下的`nginx.conf`

2. 找到http对象

   <p style="color:red;">表示监听了80端口</p>

   ![image-20231215164747535](https://cdn.jsdelivr.net/gh/xiaobo1012/imgPicGo/imgs/202312151647622.png) 

3. <p style="color:blue;font-weight:bold">开始配置代理跨域		(这些配置直接写死，以后只需要改一下跨域的名称)</p>

   ```js
   // 在location对象下面再创建一个location对象
   location /api/ {	// api是项目中的代理跨域的路径
       rewrite ^.+api/?(.*)$ /$1 break;	// 重写路径，将/api换为空
   	proxy_pass http://gmall-h5-api.atguigu.cn;	// 后端服务器的地址
   	proxy_redirect off;
   	proxy_set_header Host $host;
   	proxy_set_header X-Real-IP $remote_addr;
   	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
   }
   ```

   



## 常用命令

```js
start nginx			启动nginx服务
tasklist /fi "imagename eq nginx.exe"		查看任务进程是否存在
nginx -s stop       快速关闭Nginx，可能不保存相关信息，并迅速终止web服务。
nginx -s quit       平稳关闭Nginx，保存相关信息，有安排的结束web服务。
nginx -s reload     因改变了Nginx相关配置，需要重新加载配置而重载。
nginx -s reopen     重新打开日志文件。
nginx -c filename   为 Nginx 指定一个配置文件，来代替缺省的。
nginx -t            不运行，而仅仅测试配置文件。nginx 将检查配置文件的语法的正确性，并尝试打开配置文件中所引用到的文件。
nginx -v            显示 nginx 的版本。
nginx -V            显示 nginx 的版本，编译器版本和配置参数。

```



## 报错处理（启动nginx服务器时提示：没有运行的任务匹配指定标准）

1. 查看logs文件夹的error.log日志（端口错误）

2. 在cmd通过命令查找某一特定端口

   ```js
   netstat -ano | findstr 443		// 在所有全部开放的端口下查找443特定端口
   ```

3. 查看应用程序对应PID的对应的服务名称

   ```js
   tasklist | findstr “7500”		//查看系统中所有的进程的特定PID：7500的信息。
   ```

4. 通过以上步骤找到了占用端口的程序后，关闭程序
5. 重启Nginx服务器
**HTTP（Hypertext Transfer Protocol)超文本传输协议**

**对浏览器和服务器之间的通信进行约束**

# HTTP请求报文

![image-20230429215933910](C:\Users\QJD\AppData\Roaming\Typora\typora-user-images\image-20230429215933910.png)

请求头和请求体之间有一个空行



### 请求行

![image-20230429220023371](C:\Users\QJD\AppData\Roaming\Typora\typora-user-images\image-20230429220023371.png)

#### **请求方法**

![image-20230429220046210](C:\Users\QJD\AppData\Roaming\Typora\typora-user-images\image-20230429220046210.png)

HEAD/OPTIONS/CONNECTION用的相对较少

#### URL

![image-20230429220314461](C:\Users\QJD\AppData\Roaming\Typora\typora-user-images\image-20230429220314461.png)

### 请求头

![image-20230502015721930](C:\Users\QJD\AppData\Roaming\Typora\typora-user-images\image-20230502015721930.png)

- 记录浏览器当前所支持的一些信息

- 记录一些交互行为

### 请求体

## HTTP响应报文

### 相应行

![image-20230502020513979](C:\Users\QJD\AppData\Roaming\Typora\typora-user-images\image-20230502020513979.png)

#### 响应状态码

![image-20230502020424805](C:\Users\QJD\AppData\Roaming\Typora\typora-user-images\image-20230502020424805.png)

![image-20230502020444056](C:\Users\QJD\AppData\Roaming\Typora\typora-user-images\image-20230502020444056.png)

### 响应头

![image-20230502021018370](C:\Users\QJD\AppData\Roaming\Typora\typora-user-images\image-20230502021018370.png)

- 记录了与服务器相关的内容
- 记录了与响应体相关的内容

### 响应体

![image-20230502021348508](C:\Users\QJD\AppData\Roaming\Typora\typora-user-images\image-20230502021348508.png)

# 搭建HTTP服务

### 端口

端口是应用程序的数字标识

端口的**主要作用**：实现不同主机应用程序之间的通信

## 创建HTTP服务

```js
//导入http模块
const http=require('http')

//创建服务对象
const server=http.createServer((request,response)=>{
    response.end('hello http server')//设置响应体并结束响应
})

//监听端口，启动服务，监听9000端口
server.listen(9000,()=>{
	console.log(' start success')
})
```

### http的注意事项

1.命令行`ctrl+C`结束，在terminal使用

2.当服务启动后，更新代码必须**重启服务**才能生效

3.响应内容中文乱码的解决方法

```js
response.setHeader('content-type','text/html;chartset=utf-8')
```

4.端口号被占用

`Error:listen EADDRINUSE: address already in usr :::9000`

​	1)关闭当前正在运行监听端口的服务(`使用较多`)

​	2)修改其他端口号

5.http默认端口是80，http服务开发常用端口有3000，8080，8090，9000等

如果**端口被占用**，可以使用**资源监视器找到占用端口的程序**，然后使用**任务管理器关闭对应的程序**

## 提取http请求报文

| 含义          | 语法                                                         | 重点掌握 |
| ------------- | ------------------------------------------------------------ | -------- |
| 请求方法      | request.method                                               | T        |
| 请求版本      | request.httpVersion                                          |          |
| 请求路径      | request.url                                                  | T        |
| URL路径       | require('url').parse(request.url).pathname                   | T        |
| URL查询字符串 | require('url').parse(request.url,true).query                 | T        |
| 请求头        | request.headers                                              | T        |
| 请求体        | request.on(data,function(chunk)=>{})<br />request.on('end,function(){}') |          |


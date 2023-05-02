引入fs

```javascript
let fs=require('fs')
```

# 1.文件写入

### writeFile()异步写入

```js
/*新建一个文件，txt格式*/
/*写入文件*/

//若路径无文件则自动新建一个文件
/*(path,msg,callback())*/

fs.writeFile('qjd.txt','msg',err=>{
    //err写入失败：错误对象 
    //写入成功 null
    if(err){
        console.log(err)
        return
	}
    else{
        console.log('写入成功')
	}
})
```

### writeFileSync()同步写入

```js
//sync为同步简写
fs.writeFileSync('./qjd.txt','msg')
```

## 1.1追加写入

### appendFile/appendFileSync追加写入

```js
//引入fs模块
const fs= require('fs')

/*
path:它是一个字符串，缓冲区，URL或数字，表示将附加到其后的源文件名或文件描述符。
data:它是一个字符串或缓冲区，表示必须附加的数据。
options:它是一个字符串或对象，可用于指定将影响输出的可选参数。它具有三个可选参数：
	encoding:它是一个字符串，它指定文件的编码。默认值为“ 	utf8”。
	mode:它是一个整数，指定文件模式。默认值为“ 0o666”。
	flag:它是一个字符串，它指定附加到文件时使用的标志。默认	  值为‘a’。
callback:该方法执行时将调用该函数。
	err:如果方法失败，将引发错误。
*/
fs.appendFile('./qjd.txt','msg',err=>{
    if(err){
        console.log('写入失败')
        return 
	}
    console.log('追加写入成功')
})
```

同步写入

```javascript
const fs= require('fs')
fs.appendFileSync('./qjd.txt','msg',{flag:'a'},callback)
\\可在msg中写入'\n'进行换行
```

写日志的时候，记录用户访问时间以及相关信息等可使用appendFile方法

## 1.2fs流式写入

### createWriteStream流式写入

参数说明：

- path文件路径

- potions配置选项（可选）

  ```js
  const fs=require('fs')
  //创建写入流对象
  const ws=fs.createWriteStream('./qjd,txt')
  
  ws.write('举头望明月')
  ws.write('低头思故乡')
  
  //关闭通道
  ws.close();
  ```

  写入后的结果

  ```
  举头望明月低头思故乡
  ```

  程序打开一个文件是需要消耗资源的，流式写入可以减少打开文件的次数，流式写入适用于**大文件写入或者频繁写入**的场景，**writeFile适用于写入频率较低的场景**

## 1.3文件写入的应用场景

文件写入在计算机中是很常见的操作，例如

- 下载文件
- 安装软件
- 保存程序日志，如Git
- 编辑器保存文件
- 屏幕录制

想要**持久化保存数据**的时候应该使用**文件写入**

# 2.文件读取

### 异步读取

```js
const fs=require('fs')

//异步读取
/*
	path文件路径
	options选项配置(可选)
	callback回调函数
*/
fs.readFile('./',(err,data)=>{
    if(err){
        console.log('读取失败')
        return
    }
    //data为buffer字符串使用toString转化
    console.log(data.toString())
})
```

### 同步读取

```js
//获取文件data
let data=fs.readFileSync('./')
//输出data
console.log(data.toString())
```

- ## 2.1读取文件应用场景

- 电脑开机

- 程序运行

- 编辑器打开文件

- 查看图片

- 播放视频

- 播放音乐

- Git查看日志

- 上传文件

- 查看聊天记录

## 2.2流式读取文件

流式读取为一块一块的读取文件内容

```js
const fs=require('fs')
//创建读取流对象
const rs=fs.createReadStream('./')

//绑定一个data事件  chunk（块儿，大块儿）
rs.on('data',chunk=>{
	console.log(chunk)//输出内容为buffer格式
    console.log(chunk.length)//65536  （字节=>64kb）意味着每次读取64kb的内容
    console.log(chunk.toString())//若为视频文件，会造成乱码
})

//end可选事件
rs.on('end',()=>{
	console.log('读取完成')
})
```



# 3.文件操作

## 3.1复制文件

### 3.1.1read方式

```js
const fs=require('fs')
const process=require('process')
//读取文件内容
let data=fs.readFile('./qjd.mp4')
//写入文件
fs.writeFileSync('./qjd2.mp4',data)
console.log(process.memoryUsage())//输出对象为Object，rss为字节数，可查看内存占用

```

### 3.2.2流式操作

```js
const fs=require('fs')
const process=require('process')
//创建读取流对象
const rs=fs.createReadStream('./qjd.mp4')
//创建写入流对象
const ws=fs.createWriteStream('./qjd3.mp4')


//绑定data事件
rs.on('data',chunk=>{
    ws.write(chunk)
})
rs.on('end',()=>{
	console.log(process.memoryUsage())//输出对象为Object，rss为字节数，可查看内存占用
})
ws.close()
```

经过比对可发现，流式操作**所占资源更少**，则更好一些，但是**小文件使用流式操作内存占用较高**

```js
const fs=require('fs')
const process=require('process')
//创建读取流对象
const rs=fs.createReadStream('./qjd.mp4')
//创建写入流对象
const ws=fs.createWriteStream('./qjd3.mp4')

rs.pipe(ws)
```

可实现上述同样操作

## 3.2重命名

### 3.1.1重命名

```js
const fs=require('fs')
//调用重命名方法
/*
	oldpath：旧路径
	newpath：新路径
	callback：err
*/
fs.rename('./','./',err=>{
	if(err){
        console.log('err')
        return
    }
    console.log('success')
})

```

### 3.2.2文件移动

```js
const fs=require('fs')
//调用重命名方法
/*
	oldpath：旧路径
	newpath：新路径
	callback：err
*/
//移动文件
fs.rename('./','../',err=>{
	if(err){
        console.log('err')
        return
    }
    console.log('success')
})
```

新路径和旧路径的**父文件夹**做一改正

## 3.3文件删除

```js
const fs=require('fs')
//调用unlink方法 同步方法unlinkSync
fs.unlink('./',err=>{
    if(err){
        console.log('err')
        return
    }
    console.log('success')
})

//调用rm方法  同步方法rmSync
fs.rm('./',err=>{
	if(err){
        console.log('err')
        return
    }
    console.log('success')
})
```

## 3.4文件夹操作

### 3.4.1创建文件夹

```js
const fs=require('fs')

//创建文件夹.
/*
	path:文件夹路径
	options:配置选项（可选）
	callback：回调函数
*/
fs.mkdir('./qjd',err=>{
	if(err){
        console.log('err')
        return 
    }
    console.log('success')
})

 
//递归创建  recursive递归
fs.mkdir('./qjd/qjd',{recursive:true},err=>{
	if(err){
        console.log('err')
        return 
    }
    console.log('success')
})//直接创建则创建失败，需添加option
```

### 3.4.2读取文件夹

```js
const fs=require('fs')
//读取文件夹 read读取
fs.readdir('./',(err,data)=>{
	if(err){
		console.log('err')
		return
	}
	console.log(data)//输出结果为Array，元素为文件列表
})
```



### 3.4.3删除文件夹

```js
const fs=require('fs')

fs.rmdir('./',err=>{
	if(err){
        console.log('err')
        return 
    }
    console.log('success')
})

//递归删除
fs.rmdir('./',{recursive:true},err=>{
	if(err){
        console.log('err')
        return 
    }
    console.log('success')
})//建议使用rm进行删除
```

# 4.others

## 4.1查看资源状态

```js
const fs=require('fs')

fs.stat('./',(err,data)=>{
	if(err){
		console.log('err')
		return
	}
    console.log(data)
    console.log(data.isFile())//bool查看是否为文件
    console.log(data.isDirectory())//bool查看是否为文件夹
})
```

输出结果

![image-20230429022216027](C:\Users\QJD\AppData\Roaming\Typora\typora-user-images\image-20230429022216027.png)

## 4.2_dirname文件路径

绝对路径

- D:/qjd.html    D盘下的文件，写入C盘权限不够
- /qjd.html         此文件的根目录下进行创建

```js
const fs=require('fs')
//绝对路径全局变量，始终保存所在文件的所在目录的绝对路径，即父文件夹的绝对路径
console.log(_dirname)

console.log(_dirname+'/qjd.html')
//即为所需要文件的绝对路径
```

## 4.3批量重命名

```js
const fs=require('fs')

//读取code文件夹
const files=fs.readdirSync('./code')
console.log(files)//输出为Array格式

//遍历数组
files.forEach(item=>{
	//拆分文件名
    let data=item.split('-')
    let [num,name]=data
    //判断
    if(Number(num)<10){
        num='0'+num
    }
    //创建新的文件名
    let newName=num+'-'+name
    //重命名
    fs.renameSync('./${item}','./${newName}')
})
```


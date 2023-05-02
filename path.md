**![image-20230429144900062](C:\Users\QJD\AppData\Roaming\Typora\typora-user-images\image-20230429144900062.png)**

## path.soleve()

```js
const fs=require('fs')

const path=require('path')

//resolve解决
console.log(path.resolve(_dirname,'./index.html'))//输出结果为绝对路径
```

## sep

```js
const fs=require('fs')

const path=require('path')
console.log(path.sep)//输出分隔符
```

## parse

```js
const fs=require('fs')

const path=require('path')

console.log(__filename)//文件的绝对路径，输出为Object
```

## basename，dirname,extname

```js
const fs=require('fs')

const path=require('path')
//获取文件名
console。log(path.basename('./'))

//获取文件夹名
console。log(path.dirname('./'))

//获取文件扩展名
console。log(path.extname('./'))
```


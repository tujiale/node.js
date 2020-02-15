Node全名是Node.js，但它不是一个js文件，而是一个**软件**

Node.js是一个基于Chrome V8引擎的ECMAScript的运行环境，在这个环境中可以执行js代码

Node.js是一个容器，可以在里面运行ECMAScript代码。

它提供了很多的模块，可以很方便地实现web服务器的功能（让我们具备写后端代码的能力，例如写接口）

nodejs和javascript的区别

1.nodejs是一个容器（**不是一个新语言**），ECAMScript程序可以在这个容器中运行。

​      不能在nodejs使用window对象，也不能在nodejs使用dom操作。因为nodejs中并不包含这个对象。

2.javascript是由三个部分组成：ECAMScrtipt,BOM,DOM

###   模块化

一个js文件中可以引入其他的js文件，能使用引入的js文件的中的变量、数据，这种特性就称为模块化。

每个模块都是一个独立的文件。每个模块都可以完成特定的功能，我们需要时就去引入它们，并调用。不需要时也不需要管它。（理解于浏览器的js中的Math对象）

#### nodejs模块的分类

在nodejs中模块分成三类：

- 核心模块
- 自定义模块
- 第三方模块

核心模块:就是nodejs自带的模块，在安装完nodejs之后，就可以随意使用啦。相当于学习js时使用的Array对象。

自定义模块:程序员自己写的模块。就相当于我们在学习js时的自定义函数。

第三方模块:其他程序员写好的模块。nodejs生态提供了一个专门的工具npm来管理第三方模块

### 核心模块

- 核心模块就是 Node 内置的模块，需要通过唯一的标识名称来进行获取。

- 每一个核心模块基本上都是暴露了一个对象，里面包含一些方法供我们使用

- 一般在加载核心模块的时候，变量（或常量）的起名最好就和核心模块的标识名同名

  `const fs = require('fs')`

  示例

  ```javascript
  const fs = require('fs');
  let htmlStr = fs.readFileSync( 'index.html')).toString();
  console.log(htmlStr)
  ```

  ### 第三方模块

  所谓第三方模块，顾名思义，就是别人写的模块（不是自己写的，也不是nodejs自带的）。这一点和在浏览器环境中使用使用第三方函数或者是库非常类似 。统一由npm来管理

### fs模块

fs模块是nodejs用来进行文件操作的模块。fs是 FileSystem的简写。它属于**核心模块**

核心模块的使用步骤：

1.引入模块

const fs = require('fs');

2.调用api实现自己的要求

fs.apiName()

fs模块中操作文件(或者文件夹)的方法，大多都提供了两种选择：同步版本的 （方法名上有Sync）,**异步版本**的

### 文件内容读取 - readFile

(1)异步格式

功能：读出文件内容

格式:fs.readFile('路径',utf8,(err,data)=>{ })

要点：

- 不写utf8,得到是Buffer,如果要转字符串，直接.toString()

- 写了utf8,得到就是字符串

- 如果有错误，则err有值，

- 如果没有错误，则data有值

  (2)同步格式

  功能：读出文件内容

  与异步格式不同在于：

  1.api的名字后面有Sync（async是异步的，sync表示同步的）

  2.不是通过回调函数来获取值，而是像一个普通的函数调用一样，直接获取返回值

  格式: let rs = fs.readFile('路径',utf8)

   如果遇到了错误了，需要额外使用 try {   } catch(err) { } 结构来处理错误。

  ### 文件写入

  #### 覆盖写入 writeFile

  功能：向指定文件中写入字符串， 如果没有该文件则尝试创建该文件。它是覆盖写入：会把文件中的内容全部删除，再填入新的内容。

  格式：

  ```javascript
  fs.writeFile(pathName, content, option, callback);
  参数1: 要写入的文件路径 --- 相对路径和绝对路径均可，推荐使用绝对路径
  参数2: 要写入文件的内容
  参数3: 配置项，设置写入的字符集，默认utf-8
  参数4: 写入完成后触发的回调函数，有一个参数 --- err （错误对象）
  ```

  示例

  ```javascript
  const fs = require('fs')
  fs.writeFile('./a.txt', 'hello world niahi \n 换一行', err => {
    if (err) {
      console.info(err)
      throw err
    }
  })
  ```

  #### 文件追加 appendFile

  功能 ：向指定文件中写入字符串（追加写入）， 如果没有该文件则尝试创建该文件。

  格式：

  ```javascript
  fs.appendFile(pathName, content, option, callback);
  参数1: 要写入的文件路径 --- 相对路径和绝对路径均可，推荐使用绝对路径
  参数2: 要写入文件的字符串
  参数3: 配置项，设置写入的字符集，默认utf-8
  参数4: 写入完成后触发的回调函数，有一个参数 --- err （错误对象）
  ```

  示例：

  ```javascript
  const fs = require('fs')
  
  fs.appendFile('./a.txt', '\n 为天地立命', err => {
    if (err) {
      console.info(err)
      throw err
    }
  })
  ```

  ### 路径问题

  相对地址有问题： 问题出在这这个相对地址是以当前在哪个目录运行node命令为准。

  解决方法: 就是在操作文件时，使用**绝对路径**来定位文件。

  #### \__dirname __filename 获取绝对路径

  nodejs中提供了两个全局变量来获取获取绝对路径：

  - __dirname：获取当前被执行的文件的文件夹所处的绝对路径
  - __filename：获取当前被执行的文件的绝对路径

  全局变量的含义是：

  - 变量：它们的值是变化的。在不同的文件中值就不同。
  - 全局：在任意地方都可以直接使用

### path模块

它是也是node中的核心模块，作用是用来处理路径问题：拼接，分析，取后缀名等等。

 使用步骤：

1. 引入模块。

   ```javascript
   const path = require('path')
   ```

2,使用模块。

常用的api。

- path.basename（） ：此方法返回 `path` 的最后一部分。一般可用来获取路径中的文件名。

- path.join() ：路径拼接。

- path.parse(path) ：把一个路径转成一个对象

  ### 实操

  > 通过fs模块，实现对json文件的操作。

  message.json

  ```javascript
  [{"id":1,"name":"test","content":"测试发言内容","dt":1581171783658}]
  ```

  实现功能：

  - 读出。从message.json文件中读出内容，以js数组格式返回。

  - 添加。传入name及content即可，id是自动增长的，dt是时间戳。

  - 删除。传入id，实现删除。

    ### 自定义模块

    ##### 定义模块

    所谓定义模块，就是新建一个js文件。文件取名时，要注意一下：

    - 一般会用模块名给它命名。类比于核心模块，例如，你的模块叫myModule，则这个js文件最好叫myModule.js
    - 不要与核心模块的名字重复了。就像我们定义变量**不要与核心关键字重名**，你自己定义的模块也不要叫fs.js,因为nodejs有一个核心模块就叫fs.js。
    - 要记得**导出模块**

    
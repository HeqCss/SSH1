# node.js



## 1. fs文件系统模块

**如果要在 JavaScript 代码中，使用 fs 模块来操作文件，则需要使用如下的方式先导入它：**

​	<font color='red' fontsize='20px'>**const  fs =  require (fs);**</font>

1. **fs.readFile()** 方法，用来读取指定文件中的内容

   ![image-20220403150322110](C:\Users\Heq\AppData\Roaming\Typora\typora-user-images\image-20220403150322110.png)

- 第一个参数 path **必**  字符串，表述文件的**路径**。
- 第二个参数 options **可选参数**  表示以什么**编码格式**来读取文件 一般默认 utf8/utf-8  两种格式都识别。
- 第三个参数 callback   **必** **回调函数** 即为文件读取完成后，通过回调函数拿到读取的结果。
  - ​    回调函数中有两个参数 err(失败) 与 dataStr (成功)
  - 其中 若读取**成功**，则 err 的为 null。 dataStr为数据
  - 若读取**失败** 识别 err 的值为 错误对象  dataStr的值为underfined

- 例1<img src="C:\Users\Heq\AppData\Roaming\Typora\typora-user-images\image-20220403145253593.png" alt="image-20220403145253593" style="zoom:90%;" />



2. -**fs.writeFile()**方法，用来向指定文件中写入内容。

![image-20220403150033700](C:\Users\Heq\AppData\Roaming\Typora\typora-user-images\image-20220403150033700.png)

- 第一个参数path **必**  字符串，表述文件的路径。
- 第二个参数data **必 ** 表示要写入的内容。
- 第三个参数 options **可** 选参数 一般默认 utf8/utf-8  两种格式都识别。
- 第四个参数 callback  **必** 回调函数 即为文件读取完成后，通过回调函数拿到读取的结果。
- <img src="C:\Users\Heq\AppData\Roaming\Typora\typora-user-images\image-20220403150454580.png" alt="image-20220403150454580" style="zoom:90%;" />

## 2.path路径模块

- path 模块是 Node.js 官方提供的、用来处理路径的模块。它提供了一系列的方法和属性，用来满足用户对路径的处理需求。

### 1. 使用方式

- 导入 **const path = require('path')**

### 2.路径拼接

path.join( [ ...paths ] )

- ...paths <string> 路径片段的序列。

- 返回值: <string>。

- 例
  
  <script>
      const pathStr = path.join('/a', '/b/c', '../../', './d', 'e')
      console.log(pathStr)  // \a\b\d\e
      fs.readFile(__dirname + '/files/1.txt')
  </script>

- **其中 __dirname 为表示当前执行脚本所在的目录**

- **/a /b/c表示路径**    

- **一个'../'表示无视掉前方一个/x** 

- 实例

- <script>
      fs.readFile(path.join(__dirname, './files/1.txt'), 'utf8', function(err, dataStr) {
        if (err) {
          return console.log(err.message)
        }	
        console.log(dataStr)
  	})
  </script>

### 3.获取路径中的文件名

#### 1.path.basename()

- 使用 path.basename() 方法，可以从一个文件路径中，获取到文件的名称部分。

-  path.basename( path[, ext ] )
  - path <string> 必选参数，表示一个路径的字符串。
  - ext <string> 可选参数，表示文件扩展名。
  - 返回: <string> 表示路径中的最后一部分。

#### 2.path.extname() 

- path.extname() 方法，可以获取路径中的扩展名部分。（例.html  .js   .css .jpg ）

- path.extname( path ) 
  - path <string>必选参数，表示一个路径的字符串。
  - 返回: <string> 返回得到的扩展名字符串。





## 3.http模块

### 1.http模块

- 在网络节点中，负责消费资源的电脑，叫做客户端；负责对外提供网络资源的电脑，叫做服务器。
- http 模块是 Node.js 官方提供的、用来创建 web 服务器的模块。通过 http 模块提供的 **http.createServer()** 方法，就能方便的把一台普通的电脑，变成一台 Web 服务器，从而对外提供 Web 资源服务。

### 2.创建Web服务器的基本步骤四步

#### 1. **导入http 模块**

- <script>
      const http = require('http')
  </script>

#### 2. **调用http.createServer()方法** 

- <script>
      const server = http.createServer()
  </script>

#### 3. **为服务器绑定 request 事件**

<script>
        server.on('request',(req,res) => {
        console.log('Someone visit our web server')
   		})
</script>

##### 1.**req 请求对象**

- **req 是请求对象，他包涵了与客户端相关的数据和属性。**
  
  - **req.url 是客户端请求的 url 地址**
  - **req.method 是客户端的 method 请求类型（GET POST )**
  
- 只要服务器接收到了客户端的请求，就会调用通过 server.on() 为服务器绑定的 request 事件处理函数。

- 如果想在事件处理函数中，访问与客户端相关的数据或属性，可以使用如下的方式：

- <script>
          server.on('request',(req) => {
          const str = 'Your request is ${req.url},and request method is ${req.method}'
          console.log(str)
      	})
      </script>

##### 2. res 响应对象

- 包含了与服务器想的数据和属性。

- 例如：要发送到客户端的字符串。

- res.end( str )     //str  是自己定义的字符串

  - res.end( ) 向客户端发送指定的内容，并结束这次请求的处理过程。

- 解决中文乱码的问题 当调用ser.end方法后，向客户端发送中文内容时，会出现乱码现象，此时需要手动设置内容的编码格式。

  - **res.setHeader('Content-Tyoe', 'text/html; charset=utf-8')**

  - <script>
        server.on('request', (req, res) => {
          // 定义一个字符串，包含中文的内容
          const str = `您请求的 URL 地址是 ${req.url}，请求的 method 类型为 ${req.method}`
          // 调用 res.setHeader() 方法，设置 Content-Type 响应头，解决中文乱码的问题
          res.setHeader('Content-Type', 'text/html; charset=utf-8')
          // res.end() 将内容响应给客户端
          res.end(str)
        })
    </script>



#### 4.**启动服务器**

- <script>
      server.listen(80,() => {
          console.log('httpnbbbbbbbbbbbbb')
      })
  </script>
  - 80：代表端口号

## 4.模块化

编程领域中的模块化，就是遵守固定的规则，把一个大文件拆成独立并互相依赖的多个小模块。

### 1.模块化的分类

- **内置模块**（内置模块是由 Node.js 官方提供的，例如 fs、path、http 等）。
- **自定义模块**（用户创建的每个 .js 文件，都是自定义模块）。
- **第三方模块**（由第三方开发出来的模块，并非官方提供的内置模块，也不是用户创建的自定义模块，使用前需要先下载）。

### 2.加载模块

- 使用强大的 **require()** 方法，可以加载需要的内置模块、用户自定义模块、第三方模块进行使用。

- 内置模块 

  - <script>
        cont fs = requeire('fs')
    </script>

- 加载用户的自定义模块

  - <script>
        const curstom = require('./custom.js')
    </script>

- 加载第三方模块

  - <script>
        const moment = require ('moment')
    </script>

### 3.模块作用域

- 和函数作用域类似，在自定义模块中定义的变量、方法等成员，只能在当前模块内被访问，这种模块级别的访问限制，叫做模块作用域。
  - 优点：防止了全局变量的污染问题。

#### 1.向外共享模块作用域中的成员

- module 对象。

  - 在每个 .js 自定义模块中都有一个 module 对象，它里面存储了和当前模块有关的信息
  - 打印如下![image-20220406192348185](C:\Users\Heq\AppData\Roaming\Typora\typora-user-images\image-20220406192348185.png)
- module.exports 对象

  - 在自定义模块中，可以使用 module.exports 对象，将模块内的成员共享出去，供外界使用。
    外界用 require() 方法导入自定义模块时，得到的就是 module.exports 所指向的对象。
  - exports 对象 与 module.exports 对象 相同 是module.exports 的简写形式 在默认情况下 两者指向同一个对象
    - 但是当两者同时出现 **且冲突时** module.exports 的优先级更高。
    - **注意：为了防止混乱，建议大家不要在同一个模块中同时使用 exports 和 module.exports**
- 安装指定版本的包
  - pm install 命令安装包的时候，会自动安装最新版本的包。如果需要安装指定版本的包，可以在包名之后，通过 **@ 符号指定具体的版本**
  - **npm i moment@2.22.2**


## 5.包

- Node.js 中的第三方模块又叫做包。

- 从npm上下载搜索包

  ### 1.如何下载包

  - 在 https://registry.npmjs.org/ 上下载包
  - npm, Inc. 公司提供了一个包管理工具，我们可以使用这个包管理工具，这个包管理工具的名字叫做 **Node Package Manager（简称 npm 包管理工具）**，这个包管理工具随着 Node.js 的安装包一起被安装到了用户的电脑上。
    - 初次装包完成后，在项目文件夹下多两个文件夹，为 **node_modules** 的文件夹和 **package-lock.json** 的配置文件。
      其中：
      - **node_modules 文件夹用来存放所有已安装到项目中的包。**require() 导入第三方包时，就是从这个目录中查找并加载包。
      - **package-lock.json 配置文件用来记录 node_modules 目录下的每一个包的下载信息，**例如包的名字、版本号、下载地址等。
  - 包的语义化版本规范
    - 包的版本号是以“点分十进制”形式进行定义的，总共有三位数字，例如 2.24.0
      其中每一位数字所代表的的含义如下：
      - 第1位数字：大版本
      - 第2位数字：功能版本
      - 第3位数字：Bug修复版本
      - 版本号提升的规则：只要前面的版本号增长了，则后面的版本号归零。

  ### 2.如何在电脑上运行包

  - 在终端中输入 npm -v 来查看自己电脑上 按照的 npm 包管理工具的版本号。

  - <script>
        const moment require('moment')
    </script>

  - 在终端运行 **npm install 包的完整名称.**

    - **简写**： npm i 包的完整名称

  
























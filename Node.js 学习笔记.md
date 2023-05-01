# Node.js 学习笔记

## 基础理论

我们的Node.js是更偏向与Java的一种JavaScript，它不同于常规的JavaScript，它是首先是不存在window和document两个全局变量的。因为它主要负责的是后台处理的功能，也就是逻辑运算，而非图像渲染。但是相应的，它相较于传统的JavaScript是额外获得的更多的和获取操作系统信息的功能。譬如，Node.js可以获取计算机的信息，操作文件系统等功能

## 全局变量:

```javascript
//__dirname		-	/*当前的文件夹地址*/
//__filename	-	/*文件的名称*/
//require		-	/*用以使用module的函数*/
//module		-	/*当前的module（文件）*/
//process		-	/*返回当前程序运行的环境信息*/
```

 ## Module(组件)的使用

在Node.js中包含了CommonJS，其在默认情况下将每一个文件都视作是一个modlue，而一个module也就可以理解为是实现某一个功能模块的代码集，我们通过module可以将不同的功能模块进行分类以便我们更好的管理代码。当我们在使用module时，我们需要使用到module和require这两个关键字。其中的**module**中存在了一个**exports**对象，该对象便是我们实现module的导出和引入功能的一个桥梁，因为当我们我们可以在其他文件中使用**require(被引入的module的文件名)**来引入被依赖的module的exports对象:

```javascript
/*para.js*/
/*Private*/
const private = 'private';
/*Shared*/
const tom = 'tom';
const jerry = 'jerry';
/*此时我们将exports对象赋值为{ tom, jerry }*/
module.exports = { tom, jerry };
```

```javascript
/*fn.js*/
function sayHi(name) {
  console.log(`Hi, I am ${name}`);
}
/*此时我们将exports对象变为了sayHi的引用，这是不推荐使用的，如果这里存在多个导出的对象或者方法时*/
module.exports = sayHi;

```

```javascript
/*obj.js*/
/*此时我们为exports对象添加了一个items属性，并且该属性的值为['item1', 'item2', 'item3']*/
module.exports.items = ['item1', 'item2', 'item3'];
const person = {
  name: 'rongxin',
};
function add(a, b) {
  return a + b;
}
/*我们为exports对象添加了两个属性，分别为一个对象和方法引用*/
module.exports.singlePerson = person;
module.exports.fn = add;

```

```javascript
/*main.js*/
const names = require('./para');
const sayHello = require('./fn');
const obj = require('./obj');
console.log(names);
sayHello(names.tom);
sayHello(names.jerry);
console.log('<--divide line-->');
console.log(obj.items);
console.log(obj.fn(1, 2));
console.log(obj.singlePerson);
	
```

当我们使用require时，其被导入的module中的函数同样会被调用:

```javascript
/*demo.js*/
const v1=5;
const v2=3;
function add(){
    console.log(`result is ${v1+v2}`);
}
```

```javascript
/*main.js*/
require("./demo")
```

此时我们将会得到结果:

```
result is 8
```

这也就说明了当我们使用了require时，其被引入的module中的函数调用将会被执行，这也就为我们初始化module提供了可能性。

## Built-in modules(预设组件)

这些组件也就是Node.js内置的mudule，它们时我们可以直接调用的,其常用的包含如下:

* **OS**:		获取系统信息
* **PATH**:   创建系统路径 
* **FS**:         文件操作
* **HTTP**:    http请求

### OS使用案例

```javascript
const os = require('os');
const userInfo = os.userInfo();
/*获取当前用户信息*/
console.log(userInfo);

const systemInfo = {
  name: os.type(),
  release: os.release(),
  totalMem: os.totalmem(),
  freeMem: os.freemem(),
};
/*获取当前系统信息*/
console.log(systemInfo);
/*获取当前的系统启动时间长度(单位为秒)*/
console.log(`system uptime is ${os.uptime()}`);
```

### PATH使用案例

```javascript
const path = require('path');
/*获取连接路径:parent\child\test.txt*/
const concatPath = path.join('parent', 'child', 'test.txt');
console.log(`concat path is ${concatPath}`);
/*获取绝对路径:D:\WorkStation\Nodejs\alpha\parent\child\test.txt*/
const absulutePath = path.resolve('parent', 'child', 'test.txt');
console.log(`absulte path is ${absulutePath}`);
```

### FS使用案例

对于FS我们常用的包含了其中的两个方法**(readFileSync<同步读>，writeFileSync<同步写>)**,其中读主要包含了两个参数:(**文件路径，字符集格式**):

```javascript
const { readFileSync, writeFileSync } = require('fs');
/*此时被读取到的文件内容将会被赋予first*/
const first = readFileSync('./parent/first.txt', 'utf8');
```

而对于writeFileSync来说，这里一般可以存在三个参数**(文件路径,写入内容,可选项)**:

```javascript
const { readFileSync, writeFileSync } = require('fs');
/*此时第二个参数的内容将会被写入第一个参数的路径下的文件中，而可选项中的参数表示如果文件，新写入的内容将会被增添到原内容之后*/
writeFileSync(
  './parent/created.txt',
  `This is the content that will be writen into the file`,
  {
    flag: 'a',
  }
);
```

#### flag的参数值：

| 参数值 | 意义                                                         |
| ------ | ------------------------------------------------------------ |
| w      | 默认值，表示在写入过程中，若文件不存在则创建，否则首先将其长度变为0(清空)，而后再写入 |
| wx     | 与默认值类似，但是如果文件已存在，则失败                     |
| w+     | 表示在写入过程中，若文件不存在则创建，否则首先将其长度变为0(清空)，而后再写入 |
| wx+    | 与w+类似，同样不支持文件已存在，则失败                       |
| a      | 该参数将会在文件已存在的情况下，将新的内容追加值已存在内容之后 |
| ax     | 该参数情况下，文件若存在，则失败                             |

### **fs**的**同步读写(sync read&write)**和**异步读写(async read&write)**：

之前我们有使用到**readFileSync和writeFileSync**，它们时同步的方法，这也就意味着在使用它们进行读写操作时，它们将会阻塞JavaScript的进程，其后续的指令将不会被执行，那么在实现多并发的情况下，如果文件读写消耗时间过长则会影响到其他功能的使用，而为了避免这一情况的发生，我们使用异步的方式来实现读写功能的操作,实际上也就是将Sync去除即可，但是与同步方法不同的是，在使用异步读写时，我们可以为之添加一个回调函数，该回调函数将会在文件读写成功或者失败后被执行:

```javascript
const { readFile, writeFile } = require('fs');

console.log('start reading');
readFile('./parent/first.txt', 'utf8', (err, data) => {
  if (err) {
    console.log(err);
    return;
  }
  const first = data;
  readFile('./parent/second.txt', 'utf8', (err, data) => {
    if (err) {
      console.log(err);
      return;
    }
    const second = data;
    writeFile(
      './parent/result-async.txt',
      `The combination is :${first}${second}`,
      (err) => {
        if (err) {
          console.log(err);
          return;
        }
        console.log('done!');
      }
    );
  });
});
console.log('start handling other tasks');
```

此时我们将会得到结果：

```javascript
start reading
start handling other tasks
done!
```

由此我们注意到我们的done是最后执行的，这也就佐证了异步方法将不会影响到其他功能的正常实现，而使用同步方法时，后续指令将会在同步方法执行后才会被执行。

### createReadStream&createWriteStream的使用

顾名思义，这两者是用以**创建读写流**的方法，通过createWriteStream我们可以创建一个文件，即使这个文件并不存在:

```javascript
const {createReadStream,createWriteStream} = require('fs');
/*在src目录下创建一个gama.txt文件。而后该方法将会返回一个写的流对象*/
const writer=createWriteStream('./src/gama.txt');
/*通过writer对象进行属于的写入操作*/
writer.write('hello world!\n');
writer.write('hello rongxin!\n');
writer.end('hello tt');
```

与此同时我们也可以使用createReadStream方法来创建读的流对象,而后我们可以使用创建的对象来进行事件的绑定:

```javascript
/*创建一个读的文件流对象，默认情况下，一次读取的buffer大小是64kb，我们也
可以通过添加参数HighWaterMark进行手动配置，除此之外我们也可以使用encoding
进行字符集的设置*/
const read = createReadStream('./src/beta.txt'[,{highWaterMark:90000,encoding:utf8}]);
/*使用读的文件流对象来绑定一个data事件，该事件将可以读取文件的内容，并置于参数中
，并实现循环读取，直至文件流被读取完成，这也就解决了仅凭一个字符串对象无法装载过长的文件
的问题*/
read.on('data', (result) => {
  console.log(result);
});
```



### HTTP使用案例

我们可以通过http对象来接收http请求以及返回响应：

```javascript
/*获取http的对象*/
const http = require('http');
/*同http对象创建一个server，其中的回调函数包含了请求和响应两个对象*/
const server = http.createServer((req, res) => {
  /*我们通过对于请求的地址判断该执行那一段响应，注意:该回调函数将会一直执行，为了避免一个请求被响应多次，我们应当在每段响应结束后进行return，亦或者我们可以通过使用if-else的方法进行操作*/
  if (req.url === '/') res.end('This is the home page!');
  else if (req.url === '/about') res.end('This is the about page!');
  else
    res.end(`
  <h1>Error Page!</h1>
  <span>Opps! We could not find this page!</span>
  `);
});
/*我们让服务监听指定的端口*/
server.listen(8964);
```

## NPM(Node Package Manager)的使用

### NPM基本命令

```shell
# 在指定项目中安装该依赖包
npm i <packageName> -D #加上参数D表示将该依赖安装并设置为开发依赖，该依赖将只会在开发时生效，这里的依赖将会被置于devDependencies之中
# 在全局的所有项目中安装该依赖包
npm install -g <packegeName>
# 配置package.json(用以储存项目和报的详细信息的文件)文件
npm init -y #加上参数y表示所有配置按默认值进行
# 安装依赖，此时将会扫描packeage.json中的dependencies，继而获取该项目中的所需的依赖
npm install
# 卸载依赖
npm uninstall <packageName>
```

### package.json中的scrpts对象

当我们希望添加一些测试的指令时，可以在该对象中添加:

```json
"scipts":{
    "start": "node main.js", /*我们规定测试的命令*/
    "dev":   "nodemon app.js"/*使用nodemon进行实时测试，我们需要首先安装nodemon依赖*/
}
```

## Event Loop(事件循环)

### JavaScript的执行逻辑

我们的JavaScript是一个**单线程的不阻塞异步的并发语言（single-threaded non-blocking asychronous concurrent language)**。Javascript的执行依靠一个调用栈，每当一个方法被调用后，那么被调用的方法将会被推入栈中，直至该方法未再调用其他的方法时停止，而后，最后一个方法将会被执行，此后，方法将会被依次执行，譬如这里的一段代码:

```javascript
function multiply(a, b) {
  return a * b;
}
function square(n) {
  console.log("Hello world")
  return multiply(n, n);
}
function printSquare(n) {
  let squared = square(n);
  console.log(squared);
}

printSquare(2);
```

首先printSquare会首先加入到栈中，而后printSquare会将square置于栈的顶部，最后square会将myltiply置于栈的顶部。最后按照multiply-->square-->printSquare的顺序上而下进行执行调用者的后续的代码.**这里需要注意的是，我们的square方法中的log将会再myltiply之前被执行，因为JavaScript在执行同步代码时将会依序执行**。

### Event Loop

由于JavaScript的单线程特性，如果说我们需要实现高并发的操作，需要借助异步操作来实现，而调用异步操作的循环我们便可以看作是**Event Loop**:

![image](https://res.cloudinary.com/diqqf3eq2/image/upload/v1613666858/course%20slides/event-loop_gohdkk.png)

## 使用Promise

如果我们需要使用将一个方法变为一个异步的方法，除了**返回Promise对象**和添加**async**修饰符以外我们还可以引入util依赖来实现:

```javascript
const util=require("util");
const someFnPromed=util.promisify(someFn)
function someFn(){
    ...
}
/*此时我们使用util的方法promisify将someFn函数变成了一个异步的函数并返回*/
```

除此之外我们也可以直接通过调用promises属性来将依赖的方法直接变为异步的方法:

```javascript
const {readFile,writeFile}=require("fs").promises  /*此时readFile和writeFile就成为了Promise方法*/
```

## 事件驱动的语言

我们的JavaScript是一个事件驱动的语言，因此我们也可以自定义事件:

```javascript
/*引入依赖*/
const EventEmitter=rquire("events")
/*创建Emiter对象*/
const emiter=new EventEmitter()
/*添加response事件,并为之添加触发所需执行的回调函数*/
emiter.on('reponse',([...parameters])=>{
    console.log("Hello World!")
})
/*触发response事件*/
emiter.emit('reponse'[,...parameters])
```

### fs和http联合使用以返回前端响应

当我们向前端发送文件时，我们可以这样实现:

```javascript
const http = require('http');
const fs = require('fs');
const server = http.createServer((req, resp) => {
  const content = fs.readFileSync('./src/beta.txt', 'utf8');
  resp.end(content)
});
server.listen(8964);
```

但是使用该种方法说实现的结果是我们将会将整个文件直接发往前端，此时是不够合理的，尤其是同时向多个用户传输较大的文件时。为了解决这个问题我们可以将其分块发送:

```javascript
const http = require('http');
const fs = require('fs');
const server = http.createServer((req, resp) => {
  /*这里需要注意的一点是我们需要在createServer内调用createReadStream方法，否则是无效的stream*/
  const stream = fs.createReadStream('./src/beta.txt', 'utf8');
  stream.on('open', () => {
    stream.pipe(resp);
  });
  stream.on('error', (error) => {
    resp.end(error);
  });
});
server.listen(8964);
```

## Express 框架

和Java类似，Java是J2EE框架用以实现网络APP的构建，在JavaScript中我们也可以使用Express框架来构建，安装Express框架首先需要下载依赖也就是:

```shell
npm i express --save
```

而后我们便可以创建对象:

```javascript
/*引入依赖Express，并获取构造函数*/
const Express = require('express');
/*创建Express对象*/
const app = new Express();
```

当我们创建对象后我们便可以调用其方法进行请求的接收和响应...

```javascript
/*接收get请求，请求地址为'/'，当接收到请求后将会执行回调函数*/
app.get('/', (req, res) => {
  /*设置响应码为200返回指定值*/
  res.status(200).send(`<h1>Home Page</h1>`);
});
/*接收get请求，请求地址为'/about'，当接收到请求后将会执行回调函数*/
app.get('/about', (req, res) => {
  res.status(200).send(`<h1>About Page</h1>`);
});
/*接收所有请求，请求地址为任意地址，当接收到请求后将会执行回调函数*/
app.all('*', (req, res) => {
  res.status(200).send(`<h1>Page Not Found!</h1>`);
});
/*监听8964，并执行回调函数*/
app.listen(8964, () => {
  console.log('Listening on port 8964');
});
```

### 指定静态资源

我们可以向前端发送一个文件**(sendFile)**。如我们需要发送一个页面或者文件便可以使用该方法发送，但是需要注意的是如果是需要依赖到静态资源时，如我们的html，我们如果使用的相对路径，那么可能存在找不到文件的错误，此时我们可以将所需的公共资源统一置于一个文件夹下，如public文件夹:

```javascript
const express=require("express")
const path=require("path")
const app=new express()
/*将public文件夹设置为app的静态文件夹*/
app.use(express.static("./public"))
app.get("/",(req,res)=>{
    /*此时该index.html中所依赖的相对路径的'./...'将会被替换为'./public/...'*/
    res.sendFile(path.resolve(__dirname,'./src/index.html'))
})
```

<h3 style="display:inline;margin-right:1rem;color:red;">注</h3>:<span>需要注意的是，当我们指定了静态资源目录后，我们静态目录下的index.html将会自动视为网站的主页，则此时我们进入网站便会直接打开index.html</span>

### 返回json数据

我们如果希望返回前端JSON数据，则我们可以调用res的json方法:

```javascript
const Express = require('express');
const app = new Express();
app.get('/object', (req, res) => {
  /*此时我们返回至前端的即为json形式的对象 */
  res.json({
    code: 200,
    msg: 'success',
    data: [1, 2, 3, 4],
  });
});
```

### 请求参数(route params)

当我们请求指定页面时，如果同时存在多个同类型但是不同内容的页面，则我们需要使用参数的不同来区分各个页面，这个参数也就是我们的请求参数，和Vue中一致，我们是通过"**:id**"的方式来识别的:

```javascript
/*请求地址: http://localhost:8080/request/1/previews/3*/
...
app.get("/request/:productId/previews/previewId",(req,res)=>{
    console.log(req.params)
    /*此时获得的结果为{	productId:'1',previewId:'3'}*/
})
```

### URL query参数

当我们需要为指定页面传递参数时我们可以使用**URL Query**,一个URL Query实际上就是我们使用"**?**"所标识的部分，它们实际上并非属于URL，只是URL的参数，而route params则是**属于URL**的参数。这里的概念和Vue中的概念是一致的。当我们希望使用URL Query参数时:

```javascript
/*请求参数：http://localhost:8080/request?query=a&param=1*/ 
...
app.get("/request",(req,res)=>{
    console.log(req.query)
    /*此时获得的结果为{	query:'a',param:'3'}*/
})
```

**这里我们注意到，无论是query还是params，我们获取到的值都是<a style="color:red;font-style:italic;">字符串</a>类型的，因此如果我们需要获取数字则需要进行转换**

### 中间件(Middleware)

 当我们在每一次接收请求返回请求之间都插入一段指定的代码(逻辑)，那么很显然我们使用复制粘贴的手段是不够现实且低效的，因此我们可以在请求和响应之间添加一个中间件(**middleware**),这个中间件实际上就是一个函数，这个函数将会在请求被**<u>接受后</u>**及**<u>响应发生前</u>**被执行:

```javascript
const express=require("express")
const app=express()
/*此时我们定义的这个函数就可以作为一个中间件了，其第一个参数和第二个参数即为我们的请求和响应对象，而第三个参数也就是和Vue中的next是相同的功能:即仅在next被调用后，我们才会进入实际的响应代码也就是设置的回调函数*/
const logger=(req,res,next)=>{
    const method=req.method
    const url=req.url
    const time=new Date()
    concole.log(method,url,time)
    next()
}
/*注意这里我们仅需将中间件的引用传递即可，express将会自动为我们添加参数的，此时仅在logger的next被调用后，回调函数才会执行*/
app.get('/',logger,(req,res)=>{
    res.send(...)
})
```

但是单单这样使用，那么也就意味着我们需要为每一个请求都添加一个中间件参数，这样相对而言还是比较繁琐的，为此我们可以使用**app.use**来为每一个请求都应用一个中间件，则此时我们只用调用一次即可:

```javascript
const express=require("express")
const app=express()
/*此时我们定义的这个函数就可以作为一个中间件了，其第一个参数和第二个参数即为我们的请求和响应对象，而第三个参数也就是和Vue中的next是相同的功能:即仅在next被调用后，我们才会进入实际的响应代码也就是设置的回调函数*/
const logger=(req,res,next)=>{
    const method=req.method
    const url=req.url
    const time=new Date()
    concole.log(method,url,time)
    next()
}
/*此时下面的所有请求都会被添加上该中间件*/
app.use(logger)
/*注意这里我们仅需将中间件的引用传递即可，express将会自动为我们添加参数的，此时仅在logger的next被调用后，回调函数才会执行*/
app.get('/a...',(req,res)=>{
    res.send(...)
})
app.delete('/b...',(req,res)=>{
    res.send(...)
})
app.post('/c...',(req,res)=>{
    res.send(...)
})
...
```

前面我们提到了我们可以通过app.use来为每一个请求添加中间件，但是如果我们希望为不同的类型的请求添加不同的中间件我们则可以为其指定特定的地址即可:

```javascript
...
/*此时仅有地址以'/a'开头的请求会被添加上该中间件，譬如这里的第三个请求就不会添加该中间件*/
app.use("/a",logger)
app.get('/a/a...',(req,res)=>{
    res.send(...)
})
app.delete('/a/b...',(req,res)=>{
    res.send(...)
})
app.post('/c...',(req,res)=>{
    res.send(...)
})
...
```

实际上，我们可以以数组的形式为请求添加多个中间件:

```javascript
...
/*此时我们为所有请求都添加了两个中间件，而这两个中间件将按照数组的顺序来执行，这里也就是首先会先执行authorize而后执行logger。除此之外，我们因为可以在中间见中调用req和res，所以我们可以为它们添加额外的，属性，当我们定义了额外的属性，我们则可以在后来的代码中调用该属性*/
function authorize(req,res,next){
    ...
    req.user=[name:'rongxin',id;1]
    ...
}
function logger(req,res,next){
    ...
    /*因为logger将会在authorize后执行，因此我们可以获得req.user属性，当然我们在请求的回调函数中亦可调用*/
    console.log(req.user)
    ...
}
app.use([authorize,logger])
...
```

### POST Method

当我们使用post方法接收请求时，如果希望接收表单数据，也就是**form-data**类型的请求数据，则此时我们需要使用中间件**express.urlencoded**，**它会将form-data类型的数据封装至req.body中**

```javascript
const express = require('express');
const app = new express();
app.use(express.static('./methods-public'));
/*我们在这里使用插件，以便我们可以直接方法表单发送的数据*/
app.use(express.urlencoded({ extended: false }));

app.post('/login', (req, res) => {
  /*当我们使用了插件后，那么我们便可以调用body属性来获取一个表单的对象*/
  const { name } = req.body;
  if (!name) res.end(`<h1>Please privide a credential!</h1>`);
  else if (name === 'rongxin') res.end(`<h1>Welcome back ${name}!</h1>`);
  else res.end(`<h1>User ${name} does not exist!</h1>`);
});

app.listen(5000, () => {
  console.log('Listening on port 5000');
});
```

类似的，如果我们从前端传递的数据格式是**json**的请求，那么我们即可使用**express.json中间件，该中间件与前面的urlencoded类似，它会将请求来的json格式的数据装载至req.body内**

```javascript
const express = require('express');
const app = new express();
/*装载json请求中间件*/
app.use(express.json())
...
```

### Node.js 的 router

在Java中我们可以使用**Controller类**来接收前端的请求，而在Node.js中，我们也可以使用router来实现同样的功能，不过**在Node.js中，我们将它分为了router和controller这两个部分**。我们可以将同类型的router置于同一个router下，并将其单独置于一个文件中，如: '**/api/get/people**','**/api/get/people/:id**','**/api/get/people/:id/info**'它们就可以作为同一个router:

```javascript
/*getPeople.js*/
const express=require('express')
const router=express.Router()
...
/*注意在调用router时我们会有基本的跟router，所以这里无需添加根router,如这里的'/'就等价于 /api/get/people*/
router.get('/',(req,res)=>{...})
router.post('/',(req,res)=>{...})
router.get('/:id',(req,res)=>{...})
router.get('/:id/info',(req,res)=>{...})
...
module.exports=router
```

当我们定义了router后我们便可以在app中进行调用

```javascript
/*main.js*/
const express=require("express")
const app=new express();
const getPeople=require("getPeople")
...
app.use("/api/get/people",getPeople)
...
```

之前我们提到我们除了一个router以外还存在一个controller，我们使用router尽管简化了main.js，但时router.js页面依旧很庞杂，为此，我们可以将回调函数也提取出来以单独创建一个controller层:

```javascript
/*controller.js*/
function getPeople(req,res){
 ...   
}
function addPerson(req,res){
 ...
}
function getPeopleById(req,res){
 ...   
}
function getPeopleInfo(req,res){
 ...   
}
 ...
module.exports={
     getPeople,
     addPerson,
     getPeopleById,
     getPeopleInfo
 }
```

现在我们的getPeople.js可以写作:

```javascript
const express=require('express')
const router=express.Router()
const {
     getPeople,
     addPerson,
     getPeopleById,
     getPeopleInfo
}=require('controller')
...
/*注意在调用router时我们会有基本的跟router，所以这里无需添加根router,如这里的'/'就等价于 /api/get/people*/
router.get('/',getPeople)
router.post('/',addPerson)
router.get('/:id',getPeopleById)
router.get('/:id/info',getPeopleInfo)
...
module.exports=router
```

除此之外我们还可以使用链式调用的方案进一步简化:

```javascript
router.route('/').get(getPeople).post(addPerson)
router.route("/:id").get(getPeopleById)
router.route("/:id/info").get(getPeopleInfo)
```

## MongoDB

当我们使用Node.js时，我们可以使用MongoDB进行数据的持久化，因为它是使用json格式进行的存储的，这与我们的JavaScript时更加契合的。

### 连接MongoDB：

#### 下载连接依赖mongoose

```shell
npm install mongoose
```

#### 创建配置文件

```javascript
/*database.js*/
const mongoose = require('mongoose');
const connection =
  'mongodb+srv://rongxinyang:TXTbirth0912@rongxinbase.csuha3r.mongodb.net/taskManager?retryWrites=true&w=majority';
mongoose
  .connect(connection, {
    useCreateIndex: true,
    useNewUrlParser: true,
    useUnifiedTopology: true,
    useFindAndModify: false,
  })
  .then(() => console.log('Connected to the server!'))
  .catch((error) => console.log(`Fail to connect with error-> ${error}`));

```

```javascript
/*main.js*/
/*引入数据库依赖*/
require('./db/database');
const express = require('express');
const app = new express();
...
```

前面我们可以连接到数据库，但是由于这个连接过程是异步的，所以为了避免数据库在连接失败的情况下启动了app，我们可以封装方法来保证仅在数据库连接成功的情况下启动app：

```javascript
/*database.js*/
const mongoose = require('mongoose');
const connection =
  'mongodb+srv://rongxinyang:TXTbirth0912@rongxinbase.csuha3r.mongodb.net/taskManager?retryWrites=true&w=majority';
const connectDB = () => {
  return mongoose.connect(connection, {
    useCreateIndex: true,
    useNewUrlParser: true,
    useUnifiedTopology: true,
    useFindAndModify: false,
  });
};
module.exports = connectDB;
```

```javascript
/*main.js*/
const express = require('express');
const app = new express();
const connectDB = require('./db/database');
...
const start = async () => {
  /*此时在数据库连接失败的情况下，我们的app不会被启动*/
  try {
    await connectDB();
    app.listen(port, console.log(`Server is listening on port ${port}`));
  } catch (error) {
    console.log(`start error with error ${error}`);
  }
};
start();
```

前面的代码中我们注意到我们的连接密匙是明文显示在app内的，这显然是不合理的，那么此时我们可以借助插件env来使用单独的文件来除此连接密匙了:

#### 创建.env配置文件

```javascript
/*.env配置文件*/
MONGO_URI=mongodb+srv://rongxinyang:TXTbirth0912@rongxinbase.csuha3r.mongodb.net/taskManager?retryWrites=true&w=majority
```

```javascript
/*database.js*/
const mongoose = require('mongoose');
/*此时我们通过传入参数的形式来传递uti*/
const connectDB = (url) => {
  return mongoose.connect(url, {
    useCreateIndex: true,
    useNewUrlParser: true,
    useUnifiedTopology: true,
    useFindAndModify: false,
  });
};

module.exports = connectDB;
```

```javascript
/*main.js*/
const express = require('express');
const app = new express();
const connectDB = require('./db/database');
/*加载dot.env插件*/
require('dotenv').config();
...
const start = async () => {
/*此时在数据库连接失败的情况下，我们的app不会被启动*/
  try {
    /*通过process.env.MONGO_URI来加载MONGO_URI配置文件*/
    await connectDB(process.env.MONGO_URI);
    app.listen(port, console.log(`Server is listening on port ${port}`));
  } catch (error) {
    console.log(`start error with error ${error}`);
  }
};
start();
```

### 使用mongoose进行crud

#### Schema的定义

对于Java连接数据库时，我们需要一个实体类来实现对于数据库表的操作，而在MongoDB中，我们需要定义**Model(模型)**来进行操作而Model内部需要有Schema来定义其结构:

```javascript
/*taskModel.js*/
const mongoose = require('mongoose');
/*定义一个Task的Schema*/
const TaskSchema = mongoose.Schema({
  completed:{
  type:Boolean,
  default:false,
  }
}); 
/*通过Schema来定义model并返回*/
module.exports = mongoose.model('Task', TaskSchema);
```

```javascript
/*Schema是可以定义枚举类型的，如:*/
const mongoose = require('mongoose');
const ...Schema = mongoose.Schema({
  company:{
  type: String,
  enum:['google','amazon',"microsoft",...]
  }
});
/*我们可以在以对象的形式进行定义以便我们可以添加额外的信息*/
const ...Schema = mongoose.Schema({
  company:{
  type: String,
  enum:{
     values：['google','amazon',"microsoft",...]，
     /*message将会在我们填写的数据不在范围内输出，这里的{VALUE}代表的即为我们输入的不符合条件的值*/
     message:'{VALUE} is not supported'
    }
  }
});
```

我们也可以设置日期类型:

```javascript
const mongoose = require('mongoose');
const ...Schema = mongoose.Schema({
  company:{
  type: Date,
  default:Date.now()
  }
});
```

#### 插入操作(create)

前面我们获取到了一个task的模型，此时我们可以通过这个model进行插入的操作:

```javascript
/*taskController.js*/
const Task = require('../models/taskModel');
const createTask = async (req, res) => {
  /*我们将前台传递的JSON传递即可实现插入，注意我们的JSON中间件应当置于router的之前，否则无法获取json值*/
  /*为了避免抛出异常时，前台无法正常获取数据，我们使用try-catch进行异常的捕获*/
  try{
  const saved = await Task.create(req.body);
  res.json({ saved });
  }catch(error){
      res.status(500).json({msg:error})
  }
};
```

**注：我们在生成model时使用了schema进行创建，这个schema实际上相当于model的骨架，我们在实现插入和更新等操作时，仅有在schema中定义的属性是可以被插入的。除此之外，我们在MongoDB中是无需手动添加ID的，数据库将会自动生成属性_id并为其赋值**

#### 属性的简单验证

我们除了在schema中设置基本的类型外，我们还可以添加更多额外的信息:

```javascript
const mongoose = require('mongoose');
const TaskSchema = mongoose.Schema({
  name: {
    type: String,  /*设置属性的类型*/
    required: [true, 'this property is required!'],  /*设置是否为必填项，如果为填，则抛出异常，并显示'this property is required!''*/
    trim: true,    /*标识该属性是否会被调用trim方法以去除前后的空格*/
    maxLength: [20, 'name can not be more than 20 characters'], /*标识该属性的最大长度，超出长度则抛出异常并显示'name can not be more than 20 characters'*/
  },
  completed: {
    type: Boolean,
    default: false,
  },
});

module.exports = mongoose.model('Task', TaskSchema);

```

#### 删除操作(Delete)

```javascript
/*findOneAndRemove*/
A.findOneAndRemove(conditions, options)  // return Query
A.findOneAndRemove(conditions) // returns Query
A.findOneAndRemove()           // returns Query
```

#### 查询操作(query)

<a style="color:red;font-weight:bolder;">我们在Node.js中进行的大部分操作都是异步的，mongoose中的方法亦不例外，因此我们需要在调用方法时添加await,尤其是在进行查询操作时，我们需要为之添加await才可获取到所需数据</a>

##### 查询所有

```javascript
await MyModel.find({});
```

##### 条件查询

```javascript
/*查询name为john且age大于18的所有记录*/
await MyModel.find({ name: 'john', age: { $gte: 18 } }).exec();
```

##### 模糊查询

```javascript
/*查询name类似与john的所有记录的name和friends字段*/
await MyModel.find({ name: /john/i }, 'name friends').exec();
```

##### Query查询

相较于Java和MySQL来说，Mongo DB是以json的形式保存的，而Node.js中的请求也是可以以json的形式进行数据传输的，所以两者实现条件查询就变得更加简单了，而我们请求中的query即可直接作为Mongo DB的查询条件:

```javascript
/*前端的请求:''...?name=rongxin&age=23'*/
...
const result=await MyModel.find(req.query)
...
```

##### 查询操作符(query operators)

| Name      | Description                                                  |
| --------- | ------------------------------------------------------------ |
| $eq       | 匹配与之相等的对象：{ field: { $eq: value } }                |
| $gt       | 匹配大于指定值的对象                                         |
| $gte      | 匹配大于等于指定值的对象                                     |
| $in       | 匹配对应值属于指定的数组的对象:{ field: { $in: [value1, value2, ... valueN ] } } |
| $lt       | 匹配值小于指定值的对象                                       |
| $lte      | 匹配值小于等于指定值的对象                                   |
| $nin      | 匹配指定值不在指定数组内的对象                               |
| $ne       | 匹配非等于指定值的对象                                       |
| $and      | 用于连接两个不同的表达式以建立一个and并列关系：<br/>db.example.find( {<br/>   $and: [<br/>      { x: { $ne: 0 } },<br/>      { $expr: { $eq: [ { $divide: [ 1, "$x" ] }, 3 ] } }<br/>   ]<br/>} ) |
| $nor      | 用于创建与$and相反的语句                                     |
| $not      | 用于创建反转语句：<br/>db.inventory.find( { price: { $not: { $gt: 1.99 } } } ) |
| $or       | 用于or语句的多条件语句                                       |
| $expr     | 允许使用**聚合表达式**（aggregation expression）:<code>db.monthlyBudget.find( { $expr: { $gt: [ "$spent" , "$budget" ] } } )</code> |
| $add      | 使用加法运算：<code>const users=User.find({$expr:{$eq:[{$subtract:['$age',1]},30]}})</code> (查询所有**年龄-1**中等于30的对象，需要注意的是当我们希望在一个表达式中嵌入一个表达式时我们***需要**以对象的形式进行添加*) |
| $subtract | 使用减法运算                                                 |
| $divide   | 使用除法运算                                                 |
| $multiply | 使用乘法运算                                                 |
| $mod      | 使用取模运算                                                 |
| $regex    | 允许内部使用**正则表达式**(regular expression)：<code>{ name: { $regex: "(?i)a(?-i)cme" } }</code> |
| $text     | 描述文本的要求:{<br/>  $text:<br/>    {<br/>      $search: <string>,（所需查询的文本所需包含的短语）<br/>      $language: <string>,（指定查询文本的语言）<br/>      $caseSensitive: <boolean>（是否采用大小写敏感）,<br/>      $diacriticSensitive: <boolean>（是否采用音节敏感）<br/>    }<br/>}<br/>db.articles.find( { $text: { $search: "coffee" } } ) （查询包含coffee的文本）<br/>db.articles.find( { $text: { $search: "bake coffee cake" } } )（查询包含bake或coffee或cake的文本）<br/>db.articles.find( { $text: { $search: "\\"coffee shop\\"" } } ) (查询指定的短语)<br/>db.articles.find( { $text: { $search: "coffee  -shop" } } ) （查询包含coffee但是不包含shop的文本）<br/>db.articles.find(<br/>   { $text: { $search: "leche", $language: "es" } }<br/>)（规定文本的语言） |
| $all      | 以简化$and运算符:<code>{ $and: [ { tags: "ssl" }, { tags: "security" } ] }</code><br/>**等价于**:{ tags: { $all: [ "ssl" , "security" ] } } |

### 正则表达式(Regular Expression)

当我们从字符串中匹配到指定的片段，或者判断目标字符串是否满足所需要的要求，我们可以使用**正则表达式**来实现。

##### 正则表达式的定义

**使用两个斜线来创建：**

```javascript
const regEx = /ab+c/;
```

**创建对象的方式创建:**

```javascript
const regEx = new RegExp("ab+c");
```

**基本的正则表达式使用:**

```javascript
const regEx= /abc/;
/*该表达式将会匹配目标字符串中的abc字段*/
/*此时我们匹配字符串：*/ const txt='This is abc news!'
const result=txt.match(regEx)
/*此时result将会返回['abc']*/ //注意这里将会返回一个数组，如果是一个字符串中存在多个匹配值，那么这些值将全部被置于该数组中:
const text='This is abc news, can you spell abc?'
/*此时将会返回['abc','abc']*/
/*如果说一个字符串中并不包含指定的值，则返回一个null值*/
```

**\*占位符:**

当我们希望表示0个或者多个字符时，我们可以使用*来代表:

```javascript
const regEx=/ab*C/
/*该表达式将会匹配到诸如:'abc','abbc','abasdbc','abdssc'等,这也就意味着*分别代表了'','b','asd','dss'*/
```

##### 字符类型：

|      字符      | 含义                                                         |
| :------------: | ------------------------------------------------------------ |
|  [abc] [a-c]   | 其表示字符串将会匹配a到c之间的任意一个字符，如匹配**'big'**中的**b**,或者cancel中的**c**和**a**，即返回**['c','a','c']** |
| [^abc] [ ^a-c] | ^表示反义，这里也就意味着，该表达式将**不会**匹配a-c中的任意一个字符，如前面的cancel将会返回**[ 'n', 'e', 'l' ]** |
|     **.**      | 这里表示一个字符，而前面提到的*子表示0到多个字符             |
|       \d       | 这里表示匹配**[0-9]**的所有**<u>单个</u>阿拉伯数字**，如**'cancel233'**将会返回**[ '2', '3', '3' ]** |
|       \D       | 匹配所有的**非阿拉伯数字**的字符                             |
|       \w       | 表示匹配所有文本,相当于[a-zA-Z0-9_],如 'Émanuel123._AI'将会得到 [<br/>  'm', 'a', 'n', 'u',<br/>  'e', 'l', '1', '2',<br/>  '3', '_', 'A', 'I'<br/>] |
|       \W       | 相当于前者的取反                                             |
|       \s       | 匹配一个空格，如'**foo bar**'**，/\s\w*/g**将会匹配到**bar** |
|       \S       | 前者的取反，但是需要注意'**foo bar**'，**/\S\w*/g**将会匹配 **[ 'foo', 'bar' ]** |
|       \t       | 匹配一个水平制表符                                           |
|       \r       | 匹配一个回车                                                 |
|       \n       | 匹配一个换行                                                 |
|       \0       | 匹配一个NUL字符                                              |
|      x\|y      | 匹配x或者y,如**/green\|red/**，将可以匹配**'green apple'**中的**'green'**和**'red apple'**中的**'red**' |

##### 断言:

|  符号  | 含义                                                         |
| :----: | ------------------------------------------------------------ |
|   ^    | 表示表达式的开始，如果启用了多行，那么亦可以表示一个换行符，除此之外其在字符类型中也可以表示反义的含义，实际案例:<br/>**/^A/**将**不会**匹配 'an A',但是**可以**匹配'An A'中的第一个A |
|   $    | 其表示表达式的结束，如果启用了多行，其也可以表示一个换行符,案例:<br/>**/t$/**将会匹配**"eat"**中的**t**，但是不会匹配**"eater"**中的**t** |
|   \b   | 其代表的是字符的边界，我们可以将其置于字符的前面或者后面，置于前面则匹配前面无字符的字符，后面则匹配其后无字符的字符:<br/>**/\w\b/g**将会匹配**‘foo b ar e’**中的**[ 'o', 'b', 'r', 'e' ]**。而**/\b\w/g**将得到**[ 'f', 'b', 'a', 'e' ]** |
|   \B   | 其是前者的反义，**/\B\w/g**匹配**‘foo b ar e’**将会得到 **[ 'o', 'o', 'r' ]** |
| x(?=y) | 其将匹配被x后接y的字段，如:  **/Jack(?= Sprat)/**将会匹配**'Hello Jack Sprat'**，不过匹配的结果又些许不同，其返回结果为:<br/>**cont result=[ 'Jack', index: 6, input: 'Hello Jack Sprat', groups: undefined ]**,result[0]将会获取**'Jack'，即为我们的x**,而result.index将会返回匹配项(input)中查询的**x**的起始的索引值，而input将**包含匹配项和被匹配两者的融合** |
| x(?!y) | 前者的取反                                                   |
| (?=y)x | 前者是比较目标与后文的，而该匹配的对象是后者相较于前者       |
| (?!y)x | 前置的取反                                                   |

|    符号     | 含义                                                         |
| :---------: | ------------------------------------------------------------ |
|     (x)     | 实际上我们的一个regular expression中可以存在多个分组，而这些分组就依赖于**''()"**. |
| (?\<Name>x) | 我们在为regular expression分组后同样也可以为止进行命名，如:<br/>const imageDescription = 'This image has a resolution of 1440x900 pixels.';<br/>const regexpSize = /(?\<alpha>[0-9]+)x(?\<beta>[0-9]+)/;<br/>const match = imageDescription.match(regexpSize);<br/>console.log(`Width: ${match.groups.alpha} / Height: ${match.groups.beta}.`); |

##### 标识符

|                             符号                             | 含义                                                         |
| :----------------------------------------------------------: | ------------------------------------------------------------ |
|                              x*                              | 表示x出现0-n次，如 '**bo***'将可以匹配**'b','bo','boooo'**等 |
|                              x+                              | 与前者类似，表示x出现1-n次                                   |
|                              x?                              | 匹配0次或者一次，除此之外，当?使用在*,+等其他标识符后将将其变为非贪婪的 |
|                             x{n}                             | 这里的n应当是一个正整数，其表示的是x出现n次时匹配成功        |
|                            x{m,n}                            | 这里的大于等于0的整数，n则为一个正整数，且n>m，其表示的是x将出现m-n次时，匹配 |
| *x\**?<br/>` `*x*+?<br/>` `*x*??<br/>` `*x*{n}?<br/>` `*x*{n,}?<br/>` `*x*{n,m}?<br/> | 我们提到？将会将原本贪婪的标识符变为非贪婪模式，如:**'some <foo> <bar> new </bar> </foo> thing'**<br/>当我们使用**/<.*>/**将会匹配**\<foo> \<bar> new \</bar> \</foo>**<br/>当我们使用**/<.*?>/**将会匹配**\<foo>** |

#### 查询限制 

##### 查询排序(sort)

当我们需要将查询结果按照指定格式进行排序时，我们可以调用sort方法即可，此时传入所需要进行排序的字段名即可，默认是升序，我们可以通过添加**''-'**的方式来实现降序:

```javascript
/*希望按照name进行升序，age进行降序排序:*/
const users = User.find({}).sort("name -age");/*注意，不同字段之间使用的是空格*/

/*实际请求: 'uri/fetch?sort=name.-age' */
/*实际处理:*/
...
  const users = User.find({});
  const { sort } = req.query;
  if (sort) {
    const sortList = sort.split(',').join(' ');
    users.sort(sortList);
  }
  const result = await users;
...
```

##### 查询指定字段(select)

实际上select与前面的sort是类似的，其用法亦是类似:

```javascript
/*实际请求: 'uri/select?fields=name,age' */
/*实际处理:*/
...
const users = User.find({});
const { fields } = req.query;
 if (fields) {
    const fieldsList = fields.split(',').join(' ');
    users.select(fieldsList);
  }
const result = await users;
...
```

##### 分页查询

当我们希望对于mongo DB进行分页查询时，我们需要调用两个方法**limit和skip**,limit方法即限制每次查询的总记录数，而skip则可以规定从哪一条开始查询，假使这里存在十条记录，每页存在五条记录，而我们希望得到第二页的记录则可以写作:

```javascript
/*实际请求: 'uri/select?limit=5&skip=5' */
/*实际处理:*/
...
const users = User.find({});
const { limit,skip } = req.query;
 if(limit) user.limit(parseInt(limit))  /*注意limit和skip方法所接受的参数都是Number类型*/
 if(skip)  user.skip(parseInt(skip))
const result = await users;
...
```



#### 更新操作(findOneAndUpdate)

我们的更新方法有三个参数**(条件 ，内容，额外限制)**，我们可以根据条件来更新满足条件的数据，而第三选项则可以为我们添加条件:

**使用更新:**

```javascript
const updateTask=async (req,res)=>{
    try{
        ...
        /*更新__id为taskID的对象，且将其值设置为req.body,第三个选项的参数表示将要返回的值是更新后的值，否则
        将会返回未更新前的值而runValidators则表示在执行该方法时需要经过验证(默认为fasle，这也就意味着我们可
        以为一个规定了不可为空值的值赋予空值)*/
        const task=await Task.
        findOneAndUpdate(
            {__id:taskID},req.body，{
             new:true,
             runValidators:true
                        })
        ...
    }catch(error){
        console.log(error)
    }
}
```

#### put和patch的区别

当我们希望对数据进行更新时，在Java中，我们使用post实现更新和插入两种操作，但是在**rest api**规范中，我们可以拥有额外的两种操作:**put**和**patch**,它们两者都可以实现对于数据的更新，而它们的区别在于前者是全部覆盖，这也就意味着，当我们希望更新一个实体类，则前者会将所有属性进行更新，而后者只是更新局部的属性，也就是我们希望更新的数据。

### Async Wrapper

之前我们在controller中需要手动添加try-catch以进行异常的获取和处理，这也就意味着每一个方法我们都需要这样写，很显然，这样做并非是最佳的方案。因此我们可以使用一个包装方法来实现这一操作,而这个包装方法就称之为Wraper方法，又因为我们的controller中的方法是异步的所以称之为async wrapper。

```javascript
/*原方法*/
...
const updateTask=async (req,res)=>{
    try{
        .../*业务代码*/
    }catch(error){
        console.log(error)
    }
}
...
```

**封装后**:

```javascript
const asyncWraper=require("../middleware/AsyncWrapper")
...
const updateTask=asyncWraper(
	async (req,res)=>{
        .../*业务代码*/
	}
)
...
```

```javascript
/*../middleware/AsyncWrapper.js*/
const asyncWraper=async (fn)=>{
    return async (req,res,next)=>{
        try{
        await fn(req,res,next)
    }catch(error){
         console.log(error)
         /*调用error中间件*/
         next(error)
          }
    }
}

module.exports=asyncWraper
```

### 错误处理中间件(error-handler)

```javascript
/*error-handler middleware*/
const errorHandler=(err,req,res,next)=>{
    /*参数列表：（err:抛出的错误，req:请求对象，res:响应对象，next:next对象）*/
    return res.status(500).json({msg:err})
}

module.exports=errorHandler
```

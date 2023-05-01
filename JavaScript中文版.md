# JavaScript补充笔记

## 字符串的属性和方法

### toUpperCase()

将字符串转换为大写。

### toLowerCase()

将字符串转换为小写。

### charAt(index)

返回指定位置的字符。

### trim()

去除字符串两端的空格。

### starts(/ends)With(str)

判断字符串是否以指定的字符串开头或者结尾。

### indexOf(string)

返回指定字符串在字符串中首次出现的位置。

### lastIndexOf(string)

返回指定字符串在字符串中最后出现的位置。

### slice(start[, end])

返回从start到end之间的字符串。

### substring(start[, end])

返回从start到end之间的字符串。支持反向剪切

```javascript
const str = 'Rongxin Yang';
document.write(str.substring(5,3) + '<br>');
document.write(str);
document.write('<br>');
document.write(str.slice(4, 3) + '<br>');
document.write(str);
```

## 模板文本(Template Literals)

我们可以通过双引号或者单引号来包含字符串，此时，在该字符串中我们不可以添加js代码，如果需要应用某一个对象的一个属性等时需要使用加号进行拼接。而当我们使用了模板文本后(使用反引号)，我们便可以通过${}来获取对象的属性值，这样就可以实现动态的拼接字符串了。

```javascript
const name = "John";
const age = 22;
const sentence = `Hey it's ${name} and he is ${age} years old`;
const message = "Hey it's ${name} and he is ${age} years old";
console.log(sentence);
console.log(message);
function FullName({ firstName, lastName }) {
  const fullName = `${firstName} ${lastName}`;
  return fullName;
}
console.log(FullName({ lastName: "Yang", firstName: "Rongxin" }));
const add=`${2 + 2}`
console.log(add)
```

此时的结果：

```#
Hey it's John and he is 22 years old
Hey it's ${name} and he is ${age} years old
Rongxin Yang
4
```

## 数组的属性和方法

### length

返回数组的长度。

### concat(array)

将数组array拼接到当前数组后面。

### reverse()

将数组反转。

### unshift(element)

将element添加到数组的开头。

### shift()

将数组的第一个元素删除。

### push(element)

将element添加到数组的末尾。

### pop()

将数组的最后一个元素删除。

### splice(index, count[, element1[, element2[, ...[, elementN]]]])

将数组从index位置开始，删除count个元素[，并将element1, element2, ..., elementN添加到数组的index位置。]

## 引用和值的区别

当我们修改基本数据类型时，我们所对这个只是会影响当前这个值，即使我们在修改之前，将其值赋给了其他变量，那么其他变量将不会收到呀当前变量改变的影响。然而如果我们进一个对象直接俄赋予给了另一个对象，那么该对象的变化将直接多于另一个对象进行影响，因为此时它们两个对象的引用是相同的，也就意味着它们是同步的:

```javascript
//Change only the value
const number=1;
let number2=number;
number2=7;
console.log(`the first value is ${number}`);
console.log(`the second value is ${number2}`);
//Change the reference
let person={name:'bob'};
let person2=person;
person2.name="susy";
console.log(`the first person is ${person.name}`);
console.log(`the second person is ${person2.name}`);
```

结果为：

```#
the first value is 1
the second value is 7
the first person is susy
the second person is susy
```

如果说我们希望只是将这个对象的值赋给另一个对象可以将其写作

```javascript
let person={name:'bob'};
let person2={person};
//此时person和person2的引用是不同的，它们直接是互不影响的
person2.name="susy";
console.log(`the first person is ${person.name}`);
console.log(`the second person is ${person2.name}`);
```

## null和undefined

undifined的三种情况：

1.一个未被赋值的变量被调用时

2.当一个被调用时，指定的参数未被赋值，此时引用该参数的值为undefined

3.当一个对象的一个未被定义的属性被调用时

```javascript
let bye;
sayHi = function (name) {
  return `Hello there! I am ${name}`;
};
const people = {
  name: "tom",
};
console.log(bye);
console.log(people.age);
console.log(sayHi());
```

结果为：

```#
undefined
undefined
Hello there! I am undefined
```

## 真实性和错误性语句

在JavaScript中并非只有布尔类型才可以代表是和否，我们可以也可以使用其他的类型来表示真和假。这里有以下几种情况代表了假(因为代表假的远少于代表真的):

1.一个空的字符串，包含了被单引号，双引号和反引号所包含的字符串

2.当一个数字为0或者-0时

3.一个变量是一个NaN(Not a Number)时

4.一个变量为undefined或者内容是"false"时

## 数组的遍历

与Java不同的是，JavaScript拥有许多中不同的方式遍历数组：

### forEach(callback function)

forEach()方法可以遍历数组，并且可以改变数组中的元素，无返回值。

```javascript
var numArray = ['A', 'B', 'C', 'D', 'E'];
numArray.forEach(function (item, index) {
  item = `${index} ->${item} `;
  console.log(item);
});
```

结果为：

```#
 0 ->A 
 1 ->B 
 2 ->C 
 3 ->D 
 4 ->E 
```

### map(callback function)

map()方法可以遍历数组，并且可以改变数组中的元素，此外我们还可以将其获得的内容返回。

```javascript
var numArray = ['A', 'B', 'C', 'D', 'E'];
var item = numArray.forEach(function (item, index) {
  item = `${index} ->${item} `;
  return item;
});
console.log(item);
// 结果为:
//undifined

var numArray = ['A', 'B', 'C', 'D', 'E'];
var item = numArray.map(function (item, index) {
  item = `${index} ->${item} `;
  return item;
});
console.log(item);
//结果为:
//(5) ['0 ->A ', '1 ->B ', '2 ->C ', '3 ->D ', '4 ->E ']

```

### object.filter(callback function)

filter()方法可以遍历数组，并且可以将数组内容进行过滤，而后将过滤后的内容返回。

```javascript
const people = [
  { name: "Tom", age: "22", sex: "Male" },
  { name: "Bob", age: "26", sex: "Male" },
  { name: "Jucy", age: "22", sex: "Female" },
];
let person = people.filter(function (item) {
  return item.age < 25;
});
console.log(person);
```

结果为:

```#
[
    {
        "name": "Tom",
        "age": "22",
        "sex": "Male"
    },
    {
        "name": "Jucy",
        "age": "22",
        "sex": "Female"
    }
]
```

### onject.find(callback fuction)

其与前者的过滤器拥有同样的功能和用法，但是它只会返回第一个查找到的数据，因此其常用于查找一个拥有特定属性的对象

```javascript
const people = [
  { name: "Tom", age: "22", sex: "Male" },
  { name: "Bob", age: "26", sex: "Male" },
  { name: "Jucy", age: "22", sex: "Female" },
];
let person = people.find(function (item) {
  return item.name === "Jucy";
});
console.log(person);
```

结果为:

```#
{ 
    name: "Jucy",
    age: "22",
    sex: "Female"
}
```

### obj.reduce(callback function(acc,curentItem,index){},initial value)

这个方法是用以计算数组中各个元素对象的某一个属性的总和的，一般是数字属性。其包含三个参数，第一个参数是一个匿名函数，其参数列表又由两个参数所组成，其第一个参数表示的时当前的数量总和，第二个参数代表的是当前访问的元素对象。而这个reduce的第二个参数则为这个总量的初始值，第三个参数代表了当前访问元素的下标:

```javascript
const people = [
  { name: 'Tom', age: '22', sex: 'Male', sallery: 200 },
  { name: 'Bob', age: '26', sex: 'Male', sallery: 300 },
  { name: 'Jucy', age: '28', sex: 'Female', sallery: 500 },
  { name: 'Kasha', age: '32', sex: 'Female', sallery: 500 },
];
const dailySallery = people.reduce(function (total, cur, index) {
  console.log(`total: ${total}`);
  console.log(`current money:${cur.sallery}`);
  console.log(`current index: ${index}`);
  total += cur.sallery;
  return total;
}, 0);
```

结果为:

```#
total: 0
current money:200
current index: 0
total: 200
current money:300
current index: 1
total: 500
current money:500
current index: 2
total: 1000
current money:500
current index: 3
```

## 方形标记

在JavaScript中我们可以使用'.'来调用一个对象的属性，但是其有个一个缺陷，此时我们可以使用'[]'来代替，以创建动态属性:

```javascript
const subject='math'
const favoriate={}
favoriat.subject='some value'
console.log(favoriate)
//结果
{
  subject: 'some value'
}

const subject='math'
const favoriate={}
favoriate[subject]='some value'
console.log(favoriate)
//结果
{
  math: 'some value'
}
```

## JavaScript的DOM

### style

使用该属性我们可以实现对于一个document对象的css样式的设置

```javascript
object.style.propertyName = value;
```

### getElementsByTagName和querySelectorAll的区别

它们两者的区别在于其返回值的区别，前者返回的是一个动态的HTMLCollection集合，而后者返回的是一个static的NodeList集合。这也就意味着前者会随着网页的变化而变化，而后者不会，因此再遍历时，如果页面越大，元素越多，那么前者将会相对后者具备更快的执行效率。单数无论它们返回值是什么，只是们的返回值都类数组的集合无法使用foreach等数组遍历方法，若希望使用这些方法则需将其转为一个数组，这一点我们将在后文中有所提及

### childNodes和children的区别

两者均可获得其子元素的数组，但是前者的覆盖面更广，其中包含了文本节点，也就是各个子元素之间的空格，它们也同样作为一个文本元素被纳入其中，而后者只包含了元素节点，不包含文本节点

### parentElement

使用该**属性**我们可以获取当前元素的父级元素对象。除了他以外我们还可以使用parentNode来获得相同的效果。但是它们又有一点区别:前者仅仅返回父元素，其内部不包含其元素，而后者则包含了整个父元素，包含了父元素其内部的子元素

### previous(/next)sibling

此两者将会获取该元素的前或者后一个兄弟元素，这其中便包含了空格元素(文本元素)，当且仅当，它们都在一行时才不存在文本元素，如:

```html
 <li>
        <p>
          <span></span><div></div>
          <!-- 此时span与div治安不存在空格，使用nextSibling获取到的是div -->
        </p>
      </li>
```

如果我们需要跳过空格元素，则previousElementSibling和nextElementSibling，此时将跳过空格元素

### textContent和innerHTML

两者均可以获取一个元素的文本内容，但是前者只能获取文本内容，而后者可以获取元素内容，包括子元素。除此之外，它们都可以设置其内容，前者只可设置文本内容，而后者则可以设置且元素内容，总而言之，前者只可以控制和获元素的文本内容(即使是后代元素的文本内容亦可以被获取)而后者可以控制和获取元素内部的结构内容。

### classlist的概念

这是一个document元素对象属性，它可以获取该元素对象中所有**类名的一个数组**，其可以通过add和remove方法来添加和删除类名，同时也可以通过toggle来对于一个指定类名实现toggle操作，即如果该类名已经存在，则会删除，如果不存在，则会添加。

### document.createElement(tagName)

使用该方法我们可以创建一个新的元素对象，其中tagName是元素的标签名，其返回值便是所创建的元素对象

### createTextNode(text)

创建一个包含指定文本的文本节点，返回值便是所创建的文本节点

### appendChild(childElement)

向该元素对象末尾添加一个新的子节点

### insertBefore(newElement, oldElement)

我们可以将新元素插入其中一个老元素的前面继而实现插入位置的控制。注意此时老元素不可为控制，如果不希望指定老元素则可以将其设置为一个null值

### replaceChild(newElement,oldElement)

将一个老元素替换

### preapend(newElement)

向该元素对象的开头添加一个新的子节点

### createTextNode(text)和innerText

两者之差异在于前者是创建一个新的文本节点，而后者只是元素对象的一个属性，使用该属性可以获取或者设置元素对象的文本内容

### remove()和removeChild(childElement)

两者均是删除元素，而前者是删除当前元素对象，即调用者，而后者则是删除子元素对象，即调用者之子元素

### addEventListener(listener,callback)

在js中我们可以使用 "对象.onEvent=callback" 来监听事件，其中对象是一个元素对象，Event即为事件名，callback是一个函数，当事件发生时，会调用该函数.这里我们也可以使用addEventListener
实现同样的功能，第一个参数为事件名，第二个参数为回调函数，当事件发生时，会调用该函数。这里的callback也可以是一个函数引用。

### input.value

当我们使用input监听keyup等事件时，其可以使用e.target.value和input.value来获取当前的输入内容

### 回调函数的参数e，e.target和e.currentTarget*

这个e代表的时一个包含所有属性的对象，它可以使用e.target来获取实际触发当前事件的元素对象，也就是说如果当前对象的子元素中出触发了该事件，e.target就会指向该子对象，而使用this将永远只会指向当前对象;当这个回调函数是一个匿名函数时，this又将会指向Window对象，因为匿名函数中的this将会始终指向包含该对象外层的外层对象，在这里就是Window对象。为了解决这个问题，我们可以使用e.currentTarget来获取绑定了该事件且出发了该事件的当前对象，它将始终指向当前对象，而不是Window对象。此时e.target的作用将不受影响

### 事件捕获和事件冒泡

当我们触发一个事件时，首先会从Window对象向下进行捕获，而后再向上冒泡，在这个过程中包含了CAPTURING_PHASE,AT_TARGET和BUBBLING_PHASE三个阶段，CAPTURING_PHASE是捕获阶段，AT_TARGET是目标阶段，BUBBLING_PHASE是冒泡阶段，这三个阶段的顺序是从上到下的，我们可以使用e.eventPhase来获取当前的阶段，其分别由数字1，2，3来表示.当事件捕获到目标对象时，eventPhase的值为2，在此之前则为1，即为捕获阶段，之后为3称之为冒泡阶段。每一个adddEventListener都有第三个参数，这个参数用以指定该当前事件是在捕获阶段还是冒泡阶段执行，为true时即当前事件将会在事件捕获阶段被执行，如果为false(默认值)则在事件冒泡阶段被执行。不过始终都是先不获后冒泡的。

```javascript
const get = (id) => document.getElementById(id);
const $list = get('list');
const $list_item = get('list_item');
const $list_item_link = get('list_item_link');

// list 的捕獲
$list.addEventListener(
  'click',
  (e) => {
    console.log('list capturing', e.eventPhase);
  },
  true
);

// list 的冒泡
$list.addEventListener(
  'click',
  (e) => {
    console.log('list bubbling', e.eventPhase);
  },
  false
);

// list_item 的捕獲
$list_item.addEventListener(
  'click',
  (e) => {
    console.log('list_item capturing', e.eventPhase);
  },
  true
);

// list_item 的冒泡
$list_item.addEventListener(
  'click',
  (e) => {
    console.log('list_item bubbling', e.eventPhase);
  },
  false
);

// list_item_link 的捕獲
$list_item_link.addEventListener(
  'click',
  (e) => {
    console.log('list_item_link capturing', e.eventPhase);
  },
  true
);

// list_item_link 的冒泡
$list_item_link.addEventListener(
  'click',
  (e) => {
    console.log('list_item_link bubbling', e.eventPhase);
  },
  false
);
```

结果为:

```#
list capturing 1
list_item capturing 1
list_item_link capturing 2
list_item_link bubbling 2
list_item bubbling 3
list bubbling 3
```

此时我们可以在任意位置使用stopPropagation方法来阻止事件冒泡，这样当前事件就不会再向上冒泡了，而且如果当前事件是在捕获阶段执行的，那么捕获阶段也将在**该事件结束后提前结束**

```javascript
const get = (id) => document.getElementById(id);
const $list = get('list');
const $list_item = get('list_item');
const $list_item_link = get('list_item_link');

// list 的捕獲
$list.addEventListener(
  'click',
  (e) => {
    e.stopPropagation();
    console.log('list capturing', e.eventPhase);
  },
  true
);

// list 的冒泡
$list.addEventListener(
  'click',
  (e) => {
    console.log('list bubbling', e.eventPhase);
  },
  false
);

// list_item 的捕獲
$list_item.addEventListener(
  'click',
  (e) => {
    console.log('list_item capturing', e.eventPhase);
  },
  true
);

// list_item 的冒泡
$list_item.addEventListener(
  'click',
  (e) => {
    console.log('list_item bubbling', e.eventPhase);
  },
  false
);

// list_item_link 的捕獲
$list_item_link.addEventListener(
  'click',
  (e) => {
    console.log('list_item_link capturing', e.eventPhase);
  },
  true
);

// list_item_link 的冒泡
$list_item_link.addEventListener(
  'click',
  (e) => {
    console.log('list_item_link bubbling', e.eventPhase);
  },
  false
);
```

此时在第一个捕获阶段，整个捕获和冒泡都会提前结束。

那么使用冒泡机制我们可以实现什么样的功能呢？事实上当我们希望在一个容器中不断的生成某一个新的元素，而这个新的元素我们又希望它可以在创造出来时具备一个特定的事件绑定，但是这个实际上是无法直接被实现的，因为无论怎样我们必然是首先为其创建事件绑定而后才会对它进行创建的。而这也就意味着我们时在没有创建它的情况下，为它创建了事件绑定，这就会导致它的事件版和规定是无效的，因为如果我们希望为一个元素创建事件绑定，那么**它必须先存在于页面中，否则无法创建事件绑定**。而为了解决这一问题我们接可以借助事件冒泡的机制来完成了，我们通过为其父容器绑定该事件，通过事件冒泡来间接的为其创建事件绑定。

### preventDefault()

该事件一般是使用在form中的，其作用是用以阻止事件的默认行为，如表单的提交:当我们触发了一个表单的提交事件时，默认行为是提交表单，这样我们就可以通过使用preventDefault()来阻止这种默认行为。

```javascript
document.querySelector('#id-checkbox').addEventListener(
  'click',
  (event) => {
    document.getElementById('output-box').innerHTML +=
      "Sorry! <code>preventDefault()</code> won't let you check this!<br>";
    event.preventDefault();
  },
  false
);
```

```html
<p>Please click on the checkbox control.</p>

    <form>
      <label for="id-checkbox">Checkbox:</label>
      <input type="checkbox" id="id-checkbox" />
    </form>

    <div id="output-box"></div>
```

此时我们的的默认行为是勾选复选框，由于使用了阻止默认行为，因此此时复选框将不会被勾选。

### 本地存储和会话存储

localStorage和sessionStorage是两个全局变量，用于存储数据。它们均是对象，可以通过key-value的形式存储数据。其中localStorage存储的数据是永久性的(除非执行了删除命令，否则它将始终存在)，而sessionStorage存储的数据是会话性的，所以一旦刷新后，其储存的数据将会被直接清理。它们分别使用setItem(key,value),getItem(key),removeItem(key)和clear()方法来存储和获取和清理数据的。

```javascript
//本地储存
localStorage.setItem('friend1', 'TXT');
localStorage.setItem('friend2', 'WHQ');
localStorage.setItem('friend3', 'TJH');
//会话储存
sessionStorage.setItem("fruit1","Apple");
sessionStorage.setItem('fruit2',"Banana");
sessionStorage.setItem('fruit3',"Orange");
```

然而当我们希望将一个数组存储至一个本地储存或者会话储存时，如果直接进行储存，那么当我们再次取出该数组是，该数组将不再是一个数组了，因此我们在将数组储存之前，通过JSON.stringfy(object)将其转换为一个JSON字符串即可,而后我们需要使用该数组时，只需使用JSON.parse(JSON string)即可获得初始的数组对象

### setTimeout(function reference/call back,time)和setInterval(function reference/call back,time)

它们两者都是创建一个延时行为，前者只会执行一次，而后者将始终执行直到其被清除。如果希望为它们的回调函数传入参数，我们可以在时间之后写入参数列表

### scrollX 和 scrollY

该两个属性是用以获取和设置浏览器滚动条的位置。

### innerHeight,innerWidth和getBoundClientRect()

前两个方法分别代表了窗口的高度和宽度，所以它们是属于window对象的可以直接被调用。而getBoundClientRect()只一个用以获取元素对象与窗口相对位置的方法，其返回值是一个键值对数组，其内部的键值包括了x,y,width,height,top,right,bottom,left. x和left代表了该元素最左侧距离窗口左端的距离；y和top则代表了该元素顶部与window的顶部的距离；right和bottom代表了该元素的右侧和底部与window的右侧和底部之间的距离；至于宽度和高度自然是见明知意了。

### join(seperator)

对于数组使用该方法，我们便可以将数组中的元素通过指定的运算符串联起来形成一个字符串

### dataset

该属性是用以获取自定义属性的集合，我们可以使用dataset.name来获取指定的自定义属性的属性值

### call(),apply(),bind()

它们三者均是用以更改一个对象的方法的的对象的指针，它们的用法是一致的，第一个参数为所要更换的新的对象，第二个参数开始便是所需要传递的方法的参数，而它们又如下几个区别：call和bind的参数列表都是直接使用逗号进行分割的，而apply则是采用数组的方式承载参数列表的。除此之外，虽然它们三者都存在返回值，但是call和apply的返回值都是一个常规的变量，而bind则会返回一个方法引用，这也就意味着它是需要被调用才会发挥作用的，而前两者只要使用了即可发挥作用，如果说所设置的方法是一个有返回值的方法，前两者直接可以作为返回值使用，而bind必须调用才能获得方法的返回值:

```javascript
function person(name, age) {
  this.name = name;
  this.age = age;
  this.greet = function (sex) {
    return `Hello, my name is ${this.name} and my age is ${this.age}, i'm a ${sex}`;
  };
}
const jerry = new person('jerry', 20);
const tom = new person('tom', 22);
const get = jerry.greet.call(tom, 'boy');
const getMessage = jerry.greet.apply(tom, ['boy']);
const message = jerry.greet.bind(tom, 'boy');
console.log(get);
console.log(getMessage);
console.log(message());
```

## JavaScript补充点

## 即时调用函数表达 IIFE(Immediately invoked function expression)

该函数表达形式类似于Java的匿名函数，其函数主体是没用名称的，且由一对小括号进行包含，可以将其看作是一个独立的函数引用，因此若想调用它则同样需要一个小括号:

```javascript
(
  // local scope
  function ([params1,params2...]) {
   //expressions...
  }
)([params1,params2...]);
```

使用该表达式不如直接调用函数便利，但是可以保护其内部的局部域即local scope部分，使其无法被修改，甚至访问。

## 预加载 Hoisting

JavaScript代码与其他编程语言一样也是自上而下执行的，因此诸如const和let等变量是无法在未声明前被调用的，否则会返回一个错误。但是函数和var变量却拥有预加载能力，虽然我们可以先调用var变量再声明，但是我们获取的该变量也只是一个未定义的值。而函数却不同，即使先调用后声明，我们的函数功能也是可以正常使用的，其中的原因在于，无论是var还是函数它们都可以理解为再js加载时便会首先将这些函数和var变量提取到js的最开头以使其可以被直接调用，只是var变量在提前时只是提前了标识符却未提前其值，这也就导致值依旧处于未定义的状态

## 原型链

![原型链基本框架](https://img-blog.csdnimg.cn/8008480353ad4c148c9bfa5af977d601.png#pic_center)

```javascript
function Person(first, last, age, eyecolor) {
  this.firstName = first;
  this.lastName = last;
  this.age = age;
  this.eyeColor = eyecolor;
}

const myFather = new Person('John', 'Doe', 50, 'blue');
const myMother = new Person('Sally', 'Rally', 48, 'green');

console.log('1:访问构造方法');
console.log('对象访问构造方法:');
console.log(myFather.constructor);
console.log(myMother.constructor);
console.log('等价性：');
console.log(myMother.constructor === myFather.constructor);
console.log('2:访问原型');
console.log('构造方法访问原型:');
console.log(Person.prototype);
console.log('对象访问原型:');
console.log(myFather.__proto__);
console.log(myMother.__proto__);
console.log('等价性：');
console.log(myFather.__proto__ === myMother.__proto__);
console.log(myFather.__proto__ === Person.prototype);
console.log('3:访问Object原型');
console.log('Person原型访问Object原型:');
console.log(Person.prototype.__proto__);
console.log('Object对象访问Object原型:');
console.log(new Object().__proto__);
console.log('Object构造方法访问Object原型:');
console.log(new Object().constructor.prototype);
console.log('等价性：');
console.log(new Object().__proto__ === new Object().constructor.prototype);
console.log(new Object().__proto__ === Person.prototype.__proto__);
```

## Arrow Function

所谓的Arrow Function，是指用箭头函数的方式定义函数，箭头函数的语法是：

```javascript
const functionName=(params)=>{  /*params是参数,当没有参数时需要使用小括号，有一个参数时，可以省略小括号，当只有一条语句时可以省略大括号*/
  //expressions... /*当只有一条语句时，若需要返回一个值，则可以省略return;如果是一条语句，且返回值是一个对象时，这个对象需要被小括号包含*/
}
```

### Arrow function和Regular function的this的指向的差异

Arrow function的this始终指向它的包含域，而Regular function的this指向的是调用它的对象。

```javascript
//修改前
const bob = {
  firstName: 'bob',
  lastName: 'smith',
  sayName: function () {
    console.log(this);
    console.log(`Hello, my name is ${this.firstName} ${this.lastName}`);
  },
};

const anna = {
  firstName: 'anna',
  lastName: 'sanders',
  sayName: () => {
    console.log(this);
    console.log(`Hello, my name is ${this.firstName} ${this.lastName}`);
  },
};

bob.sayName();
//此时因为常规函数的this指向调用对象；所以this指向bob对象，因此bob的信息被正确解析
anna.sayName();
//此时因为箭头函数的this指向包含域，所以this指向全局域对象即window，因此anna的信息未被正确解析


//修改后
const bob = {
 firstName: 'bob',
  lastName: 'smith',
  sayName: function () {
    console.log(this);//此时this指向调用者bob对象
    [window.]setTimeout(function () {//此时该常规的this指向调用者window对象
      console.log(this);//this指向window对象，对象无法正常解析
      console.log(`Hello, my name is ${this.firstName} ${this.lastName}`);
    }, 1000);
  },
};

const anna = {
  firstName: 'anna',
  lastName: 'sanders',
  say: function () {
    console.log(this);//此时this指向调用者window
    [window.]setTimeout(() => {
      console.log(this);//此时该箭头函数的this指向包含域对象anna，因此anna信息被正常解析
      console.log(`Hello, my name is ${this.firstName} ${this.lastName}`);
    }, 0);
  },
};

bob.sayName();//见代码部分
anna.sayName();//见代码部分
```

### 箭头函数与常规函数的默认值以及一些特性

在ES6中，箭头函数和常规函数都是是可以设置默认值的。当我们设置了默认值后，即使调用函数时为传入参数，那么该函数也将使用默认值进行操作。

```javascript
function fnName(param1 = defaultValue, param2 = defaultValue) {
  //...
}
fnName();//param1和param2都使用默认值

const fnName = (param1 = defaultValue, param2 = defaultValue) => {
  //...
}
fnName();//param1和param2都使用默认值
```

我们知道JavaScript中函数是具备预解析能力的，所以配合着默认值，我们可以在任意位置调用常规函数，但是箭头函数却不能，它本身是没有名字的匿名函数，只是寄居于一个变量之中，因此遵循变量的预解析法则，因此箭头函数是不支持预解析的。

## ES6中的数组拆解

如果这里有一个数组，我们可以使用索引或者各式方法来提取其中的数据，但是实际上，ES6中的数组提取方法更加强大，更加灵活。

```javascript
const friends=['bob','anna','jane','tom'];

//数组拆解
const [first,second,third,fourth]=friends;
//访问数组元素
console.log(first);//bob
console.log(second);//anna
console.log(third);//jane
console.log(fourth);//tom
```

数组拆解的顺序是按照数组的顺序来拆解的，如果拆解元素的数量不足，那么后面的元素将被忽略。反之，如果元素过多，多出的元素将显示为undefined:

```javascript
const friends=['bob','anna','jane','tom'];

//数组拆解
const [first,second,third,fourth]=friends;
//访问数组元素
console.log(first);//bob
console.log(second);//anna
console.log(third);//jane
console.log(fourth);//tom
console.log(fifth);//undefined
```

如果希望跳过某些元素，则不为其命名即可

```javascript
const friends=['bob','anna','jane','tom'];

//数组拆解
const [first,second,,fourth]=friends;//跳过了jane
//访问数组元素
console.log(first);//bob
console.log(second);//anna
console.log(third);//undefined
console.log(fourth);//tom
console.log(fifth);//undefined
```

### 对象的交换

除了拆解数组外，这个运算符还可以用来交换对象:

```javascript
let rongxin="rongxin";
let griffin="griffin";
[rongxin,griffin]=[griffin,rongxin];
console.log(rongxin,griffin);//输出griffin,rongxin
```

### 对象的拆解

与数组的拆解不同的是，拆解对象需要使用大括号来包含,此外需要注意的是我们不可在拆解元素时使用非该对象的属性值作为拆解元素:

```javascript
const bob = {
  first: 'bob',
  last: 'sanders',
  city: 'chicago',
  siblings: {
    sister: 'jane',
  },
  ,
  sayhello: function () {
    console.log('Hello');
  },
};

const {
  first: firstName,
  last,
  city,
  zip,//对象中无该属性
  siblings: { sister: favoriteSibling },
} = bob;
console.log(firstName, last, city, zip, favoriteSibling);//输出bob sanders chicago undefined jane
sayhello();//输出hello
```

### repeat()

该方法为ES6提供了一个新的方法，用来重复一个字符串或者一个字符:

```javascript
const str = 'hello';
const str2 = str.repeat(3);
console.log(str2);//输出hello hello hello
```

## 扩展符(Spread operator)

在ES6中新增了一种扩展符，其可用于将一个字符串拆解为一个字符数组，将一个数组拆解为多个元素以合并数组集亦或者将一个数组复制

### 拆解字符串

```javascript
const udemy = 'udemy';
const letters = [...udemy];
console.log(letters);//输出u,d,e,m,y
```

### 合并数组

```javascript
const boys = ['john', 'peter', 'bob'];
const girls = ['susan', 'anna'];
const bestFriend = 'arnold';

const friends = [...boys, bestFriend, ...girls];
console.log(friends);//输出 ['john', 'peter', 'bob', 'arnold', 'susan', 'anna']
```

### 数组复制

我们知道数组也是一个对象，如果将一个数组直接赋值给另一个数组，则它们将在内存中共用一个地址引用，则这就意味着它的数据将会同步，当其中一个数组数据发生改变时，则另一个将同时发生改变，为了避免这一问题，我们可以将一个数组通过扩展符进行复制，将这个复制的对象传递给另一个数组

```javascript
const boys = ['john', 'peter', 'bob'];
const girls = ['susan', 'anna'];
const bestFriend = 'arnold';

const friends = [...boys, bestFriend, ...girls];
const newFriends = [...friends];
```

### 对象复制

除了将数组复制，亦可以将对象复制:

```javascript
const bob = {
  first: 'bob',
  last: 'sanders',
  city: 'chicago',
  siblings: {
    sister: 'jane',
  },
  sayhello: function () {
    console.log('Hello');
  },
};
const newPerson = { ...bob };
console.log(newPerson);
```

再复制对象后，新建的对象可以被扩展和内容覆盖，因为此时扩展符只是在一个对象里将另一个对象的所有属性方法扩展出来置于新对象之下，因此它只是新对象的一部分，而不是全部的属性方法。因此我们可以再为新对象增添一些新的属性方法。如果我们希望覆盖被扩展的对象的值，则仅需在新对象为其重新赋值即可:

```javascript
const bob = {
  first: 'bob',
  last: 'sanders',
  city: 'chicago',
  siblings: {
    sister: 'jane',
  },
  sayhello: function () {
    console.log('Hello');
  },
};
const newPerson = { ...bob,job: 'web developer' };//对象的扩展
const newPerson = { ...bob,first: 'john' };//属性的覆盖
console.log(newPerson);
```

### 将DOM元素转换为数组

在此之前我们提到过querysqlqctorAll和getElementsByTagName都可以选取页面的元素对象将其置于各自的NodeList和HTMLCollection对象之中，但是它们都是类数组对象因此无法直接使用数组的方法，为了解决这一问题我们便可以使用扩展符将其转换为数组，如下所示:

```javascript
const elements=document.querySelectorAll('div');
const divEle=[...elements];
```

### 剩余收集符

同样使用前面的扩展符，我们将其置于接收对象的末尾即可将其转为剩余收集符。前者是将对象拆解，而这里则是将对象的部分元素合并：

```javascript
//arrays
const fruit = ['apple', 'orange', 'lemon', 'banana', 'pear'];
const [first, second, ...fruits] = fruit;//此时我们的前两个参数first和second被分别赋予了apple和orange两个值，而其后的lemon,banana和pear则被赋予了fruits之中整合成为了一个新的fruits数组
// console.log(first, fruits);

//objects
const person = { name: 'john', lastName: 'smith', job: 'developer' };
const { job, ...rest } = person; //与数组不同的是，对象是根据属性值被拆解的，因此这里除了job被都被单独提取外
// const {  ...rest,job } = person;//注意剩余收集器须置于末尾
// console.log(job, rest);

const testScores = [78, 90, 56, 43, 99, 65];

const getAverage = (name, ...scores) => { //将第二个即之后的参数统一置于剩余收集器之中
  console.log(name);
  console.log(scores);
  let total = 0;
  for (const score of scores) {
    total += score;
  }
  console.log(`${name}'s average score is ${total / scores.length}`);
};

getAverage(person.name, 78, 90, 56, 43);
getAverage(person.name, ...testScores);

```

## Array.of和Array.from()

两者均是将指定的元素转换为一个数组，前者是将其内部的所有参数(不计类型)均置于同一个数组之中:

```javascript
const array=Array.of('Rongxin Yang',12,true);
//数组将被储存为['Rongxin Yang',12,true]此时三种不同的数据类型的数据被置于同一个数组之中
```

后者是将类数组的对象转换为数组对象，如字符串，NodeList，HTMLCollection，set等等，如下所示:

```javascript
const strArray=Array.from('Rongxin');/*返回数组:[R,o,n,g,x,i,n]*/
```

类数组的对象还有很多，例如方法中的参数列表其被arguments对象所储存，其储存方法也是域数组类似，因此也是一个类数组对象，故也可以使用Array.from()方法将其转换为数组对象。

### Array.from的扩展

Array.from除了第一个参数是用以接受转化的对象，事实上它还可以有第二个参数，也就是一个回调函数，该回调函数的参数即代表了前面的对象转换为数组后的各个元素对象，其可以返回任意值以将整个函数返回值指定为指定值,其后的回调函数相当于是该函数返回值(数组)的一个map方法：

```javascript
const p = document.querySelectorAll('p');
const result = document.getElementById('result');
const second = document.getElementById('second');

let newText = Array.from(p);
newText = newText.map(item => `<span>${item.textContent}</span>`).join(' ');//使用传统的map方法对转化后的数组进行操作

result.innerHTML = newText;

const text = Array.from(document.querySelectorAll('p'), item => {//使用回调函数作为map方法直接进行操作，其结果与前面的结果是一致的
  return `<span>${item.textContent}</span>`;
}).join(' ');

second.innerHTML = text;
```

## findIndex,every和some

与find类似，find是用以查找数组中包含指定指定特性的唯一对象，因此获取的返回值是一个对象；而findindex则是获取该对象的索引值，因此返回值是一个数字。

```javascript
const friends=[
  {
    id: 1,
    name:'Rongxin Yang',
    age:21
  },
  {
    id: 2,
    name:'Tao Xueting',
    age:20
  },
  {
    id: 3,
    name:'Wang Hanqi',
    age:20
  }
]
const result=friends.find(item=>item.id===2);//返回对象{id: 2, name: "Tao Xueting", age: 20}
const result2=friends.findIndex(item=>item.id===2);//返回索引值1
```

every是值某个数组中所有值都满足某一条件便返回true，否则返回false；some是值某个数组中任意一个值满足某一条件便返回true，否则返回false:

```javascript
const grades=['A','B','C','D','E'];
const goodeGrades=['A','B','B','A','B'];
//只要有一个满足条件即可为true
const hasBadGrades=goodGrades.some(item=>item==='C'||item==='D');//返回false
const hasGoodGrades=grades.some(item=>item==='A'||item==='B');//返回true
//必须全部满足条件才能为true
const hasGoodGrades2=goodeGrades.every(item=>item==='A'||item==='B');//返回true
const hasBadGrades2=goodeGrades.every(item=>item==='C'||item==='D');//返回false
```

## for in和for of

前者可以遍历对象和数组，但是其主要是遍历对象，不建议遍历数组因为遍历数组时我们首先返回的是索引，而后还要使用所以获取对象； 而for of 是遍历数组专用的语法，直接返回数组元素对象：

```javascript
 const person={
    id: 1,
    name:'Rongxin Yang',
    age:21
  }
  const array=[1,2,3,4,5];
  //for in遍历对象
  for(let key in person){
    console.log(key);//输出id,name,age
    log(person[key]);//输出1,Rongxin Yang,21
  }
  //for in遍历数组
  for(let key in array){
    console.log(key);//输出0,1,2,3,4
    log(array[key]);//输出1,2,3,4,5
  }
  //for of遍历数组
  for(let item of array){
    console.log(item);//输出1,2,3,4,5
  }
```

## keys,values和entries

任意一个对象的属性和值都可以通过keys,values,entries方法获取，其中keys方法返回的是一个数组，其中每个元素都是对象的属性；values方法返回的是一个数组，其中每个元素都是对象的值；entries方法返回的是一个数组，其中每个元素都是一个数组，每一个数组的第一个元素是对象的属性，第二个元素是对象的值，而且整个属性和值是相对应的

## Set

尽管JavaScript的数组已经很好用了，但是这里有一个小问题即再数组中，所有的元素都是不被保证是唯一的，这也就意味着，同一个数组内可能存在两个完全一样的元素，而为了解决这一问题。我们可以采用Set数据结构，Set数据结构是一种没有重复元素的数据结构，它类似于数组，但是成员的值都是唯一的，不过由于它是类数组，因此它只可以使用foreach，若希望使用数组方法则需首先转换为一个数组。

对于Set,它可以使用add,delete,has,size方法，其中add方法可以添加元素，delete方法可以删除元素，has方法可以判断元素是否存在,size获取其内部元素数量。

```javascript
const friends = ['bob', 'anna', 'jane', 'tom', 'lily','tom', 'lily'];
const setItem = new Set(friends);
console.log(setItem);
```

## Promise

Promises是一个对象，它代表一个异步操作，可以用then方法来指定回调函数，当异步操作完成时，会执行回调函数。它均有三种状态，Pending，Resolved，Rejected。这三个状态分别意为等待状态，成功状态，失败状态。当我们的Promise处于等待状态就意味着什么都没有发生，而当我们的Promise处于成功状态时(调用了resolve方法)，这是我们便可以使用then来执行下一步操作了；如果处于失败状态即调用了reject方法，则可以使用catch(失败状态类似于Java中的异常情况因此需要对其进行捕获)来执行下一步操作了。

为了更好的理解，这里有一个很形象的例子:当我们去餐馆吃饭时，我们并不是去了餐厅就能吃上饭的，首先我们去了之后需要点餐，而点餐之后我们需要等待，这个等待时间就相当于Pending时间；正常情况下我们可以等一段时间后就可以拿到菜了，此时这就相当于调用了reslove，这是我们就可以吃饭了；如果点餐失败了，如某种材料突然用完了或者其他各种情况无法正常做出我们需要的菜时，这是我们拿不到菜，这就相当于调用了reject，这时我们就不能吃饭了，这时我们就可以使用catch来捕获错误了。

```javascript
const heading1 = document.querySelector('.one');
const heading2 = document.querySelector('.two');
const heading3 = document.querySelector('.three');

const btn = document.querySelector('.btn');

btn.addEventListener('click', () => {});

const promise = new Promise((resolve, reject) => {//这里的两个参数即我们所需要的两个方法
  let value = false;
  if (value) {
    resolve([1, 2, 4]); //使用该方法意味着我们的Promise处于成功状态，其内部所传递的参数可以是任意对象，可以是一个对象，一个数组，一个字符串等等。then方法中的回调函数的参数将会将其接收。此时promise的状态(status)即为resolved
  } else {
    reject(`there was a error, value is false`);//使用该方法意味着我们的Promise处于失败状态，其内部所传递的参数可以是任意对象，可以是一个对象，一个数组，一个字符串等等。catch方法中的回调函数的参数将会将其接收。此时promise的状态(status)即为rejected
  }
});
promise
  .then((taco) => {
    console.log(taco);//接收到了成功(resolve)状态的参数
  })
  .catch((err) => {
    console.log(err);//接收到了失败(reject)状态的参数
  });

```

实际案例1(该案例中，我们通过点击按钮可以将一张随机图片添加至容器之中)：

```javascript
const heading1 = document.querySelector('.one');
const heading2 = document.querySelector('.two');
const heading3 = document.querySelector('.three');
const btn = document.querySelector('.btn');
const container = document.querySelector('.img-container');
const url = 'https://source.unsplash.com/random';
btn.addEventListener('click', () => {
  loadImage(url)
    .then((taco) => container.appendChild(taco)) //当我们点击按钮时，我们需要加载一张图片，如果地址正确，则将会返回正确的图片对象，而后我们将获得的图片对象添加到容器中
    .catch((err) => console.log(err));  //如果遇到网络问题或者图片地址有误则此时将会返回错误信息，此时我们可以将其捕获
});

function loadImage(url) {
  return new Promise((resolve, reject) => {
    let img = new Image();
    img.addEventListener('load', () => {  //当任务成功加载时，将会触发该事件
      resolve(img);  //将图片对象返回给then方法
    });
    img.addEventListener('error', () => { //当任务失败时，将会触发该事件
      reject(new Error(`Failed to load image from the source : ${url}`)); //将错误信息返回给catch方法
    });
    img.src = url;  //设置图片的地址
  });
}
```

实际案例2(该案例中html部分应包含三个标题，这三个标题分别被heading1/2/3所代指，当我们点击按钮之后，这三个标题将分别在1秒，2秒和1秒后添加颜色)：

```javascript
const heading1 = document.querySelector('.one');
const heading2 = document.querySelector('.four');
const heading3 = document.querySelector('.three');
const btn = document.querySelector('.btn');
btn.addEventListener('click', () => {
  addColor(1000, heading1, 'red')  //如果该链条中任意一个出现了问题，那么从该链条开始以后的所有then方法都将不再执行，继而进入catch方法；此外在该链条中只有当前一个执行完成后才会执行下一个then方法
    .then(() => addColor(2000, heading2, 'green'))
    .then(() => addColor(1000, heading3, 'blue'))
    .catch((err) => console.log(err));
});

function addColor(time, element, color) {
  return new Promise((resolve, reject) => {
    if (element) {
      setTimeout(() => {
        element.style.color = color;
        resolve();//注意这里的resolve方法，如果不调用该方法，将会导致Promise的状态始终处于Pending状态
      }, time);
    } else {
      reject(new Error(`There is no such element ${element}`));//当然这里的reject如果不被调用则Promise的状态同样将始终处于Pending状态
    }
  });
}
```

除了使用then来接收promise以外我们可以使用async 和 await来接收继而使其更加清晰明了：

```javascript
//初始态
const heading1 = document.querySelector('.one');
const heading2 = document.querySelector('.two');
const heading3 = document.querySelector('.three');
const btn = document.querySelector('.btn');
btn.addEventListener('click', async () => { //在回调函数前添加async是其用法之一，我们也可直接在函数前添加async关键字，表示该函数是一个异步函数，这样就可以使用await来接收promise
 try {  //此时捕获reject我们需要使用try catch来捕获就像Java的异常捕获一样
    await addColor(1000, heading1, 'red');//此时await相当于是then
    await addColor(1000, heading2, 'green');
    await addColor(1000, heading3, 'blue');
    console.log(first);//此时返回为undifined因为，rersolve未返回任何值
  } catch (error) {
    console.log(error);
  }
});
function addColor(time, element, color) {
  return new Promise((resolve, reject) => {
    if (element) {
      setTimeout(() => {
        element.style.color = color;
        resolve();
      }, time);
    } else {
      reject(new Error(`There is no such element ${element}`));
    }
  });
}

//修改后
const heading1 = document.querySelector('.one');
const heading2 = document.querySelector('.two');
const heading3 = document.querySelector('.three');
const btn = document.querySelector('.btn');
btn.addEventListener('click', async () => {
  const result = await displayColor();
  console.log(result);//此时返回的是我们返回的字符串hello
});

async function displayColor() { //函数前添加async关键字，表示该函数是一个异步函数，这样就可以使用await来接收promise，此时需要注意的是该函数此时默认返回值是一个promise，其value即接收到的value；而如果我们希望更改其返回值则自行添加一个返回值即可实现其覆盖
  try {
    const first = await addColor(1000, heading1, 'red');
    await addColor(1000, heading2, 'green');
    await addColor(1000, heading3, 'blue');
    console.log(first);
  } catch (error) {
    console.log(error);
  }
  return 'hello';//这个return的字符串将会覆盖默认返回值即一个promise对象
}

function addColor(time, element, color) {
  return new Promise((resolve, reject) => {
    if (element) {
      setTimeout(() => {
        element.style.color = color;
        resolve();
      }, time);
    } else {
      reject(new Error(`There is no such element ${element}`));
    }
  });
}

```

## Ajax

```javascript
const xhr = new XMLHttpRequest();  //创建一个XMLHttpRequest对象

xhr.open('GET', './api/sample.txt'); //readyState在调用了open方法后就会变为1(初始值为0表示未设置)，表示已经准备好了，可以调用send方法了；其参数列表为open(method, url[[, async][, user][, password]]),第一参数为请求方法("GET", "POST", "PUT", "DELETE")，第二个参数为请求的url，第三个参数为是否异步，默认为true，第四个参数为用户名，第五个参数为密码，后三个参数均可省略
xhr.onreadystatechange = function () {  //一个监听事件，用以在readyState发生改变时触发
  if (xhr.readyState === 4 && xhr.status === 200) {//readyState为4表示请求已完成，status为200表示请求成功
    const text = document.createElement('p');
    text.textContent = xhr.responseText;//reponseText,将返回的响应信息转换为的字符串
    document.body.appendChild(text);
  } else {
    console.log({
      status: xhr.status, //请求的状态码，包含了UNSENT: 0 OPENED: 0 LOADING: 200 DONE: 200 客户端问题:400 服务端问题:500
      text: xhr.statusText,
      state: xhr.readyState,//
    });
  }
};
xhr.send();
```

readyState的五种状态
<table>
  <thead>
    <tr>
      <th>Value</th>
      <th>State</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>0</code></td>
      <td><code>UNSENT</code></td>
      <td>Client has been created. <code>open()</code> not called yet.</td>
    </tr>
    <tr>
      <td><code>1</code></td>
      <td><code>OPENED</code></td>
      <td><code>open()</code> has been called.</td>
    </tr>
    <tr>
      <td><code>2</code></td>
      <td><code>HEADERS_RECEIVED</code></td>
      <td><code>send()</code> has been called, and headers and status are available.</td>
    </tr>
    <tr>
      <td><code>3</code></td>
      <td><code>LOADING</code></td>
      <td>Downloading; <code>responseText</code> holds partial data.</td>
    </tr>
    <tr>
      <td><code>4</code></td>
      <td><code>DONE</code></td>
      <td>The operation is complete.</td>
    </tr>
  </tbody>
</table>

**注：** XMLHttpRequest是浏览器提供的对象，正如setInterval一样，因此它们都是委托给浏览器执行的，知意javascript空闲时才会接收其返回的结果

### fetch的使用

fetch是基于Promise的，此外它也是浏览器所提供的内置方法因此我们可以直接对其进行调用。

```javascript
const url = './api/people.json';

const btn = document.querySelector('.btn');

btn.addEventListener('click', () => {
  const first = fetch(url)//调用fetch方法，返回一个Promise对象，返回值即为我们接收到的json字符串
    .then((resp) => resp.json()) //因为接收的返回值是json对象因此调用json将其转为json字符串，此时返回一个promise对象，而这个promise对象的返回值即为我们所接受的json字符串所经过json方法转换后的对象
    .then((data) => { 
      displayItems(data);//这里的data接收的即为我们所接受的json字符串所经过json方法转换后的对象
    })
    .catch((err) => console.log(err));
});

const displayItems = (items) => {
  const displayData = items
    .map((item) => {
      return `<p>${item.name}</p>`;
    })
    .join('');
  const element = document.createElement('div');
  element.innerHTML = displayData;
  document.body.appendChild(element);
};
```

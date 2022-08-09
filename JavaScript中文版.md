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

### obj.reduce(callback function(acc,curentItem){},initial value[,index])

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

两者均可以通过选择器参数获取符合其选择器参数的所有document元素对象，然而前者获取的只是一个类数组的对象，其事实上并不是一个严格意义上的数组，因此它除了可以使用一个foreach方法意外，注入map,filter等数组方法均不可以调用，而后者则是会返回一个数组集合因此可以调用所有的数组方法，当然也包括了foreach方法

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

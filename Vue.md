# Vue

### Interpolation and Data binding

Vue could be chosen to control all the UI components in the application, and also can be chosen to control just several UI components. So if we want use Vue to control these components we should make an data binding, usally, that is implemented by id, because id is unique in the application

#### create a Vue object

```javascript
const app = Vue.createApp({
  data() {
    return {
      customProperty: value,
    };
  },
});
app.mount('selector');
```

<strong>Note:<br>
1.mout()->In Vue, any vue object has the lifecyclehook:create,mount,update and destroy, when we create a Vue object, this object in fact is stored in virtual DOM memory, so at this moment, the vue object can make any change to the DOM, only we mount this vue object, it can make changes to the DOM, so this method can be called only once.<br>
2.When Vue controls an element, the nested elements are also being controlled
</strong>

After we create a vue object, we could set a data function in it, but this function name can only be "data", besides this data function should do only one thing is just return an object that contains so many different properties or objects that you can decide. These properties can be anything whatever string,number or array etc.

After we set the property in data function, we could use "{{ property }}" to bind the property in js and html in it's related component.

But when we want to bind a property to a attribute, the double curly braces won't work. For example, when we need aet a link, and we want set the property "vueLink" as the value of href attribute. We need tell the Vue we want bind this property to this href attribute, so at this time we could use "v-bind:", and we could set the property name into the value position without curly braces.

Except properties, we have the functions in javascript, and like the <strong>"data" function</strong> to express the data, we use an <strong>"methods" object</strong> to represent all of the methods in this component, and when we want to access these methods, we could invoke them in double curly braces or after "v-bind:" in the format of "functionName()"

Here's an example:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vue Basics</title>
    <link
      href="https://fonts.googleapis.com/css2?family=Jost:wght@400;700&display=swap"
      rel="stylesheet"
    />
    <link rel="stylesheet" href="styles.css" />
    <script src="https://unpkg.com/vue@next" defer></script>
    <script src="app.js" defer></script>
  </head>
  <body>
    <header>
      <h1>Vue Course Goals</h1>
    </header>
    <section id="user-goal">
      <h2>My Course Goal</h2>
      <p>{{ courseGoal(/outputGoal()) }}</p>
      <p>Learn more <a v-bind:href="vueLink">about Vue</a>!</p>
    </section>
  </body>
</html>
```

```javascript
const app = Vue.createApp({
  data() {
    return {
      courseGoal: 'Finish the course and learn Vue!',
      outputA:'Vue is awesome!',
      outputb:'Programming is fun!',
      vueLink: 'http://vuejs.org/',
    };
  },
  methods: {
    outputGoal:(){
      const randomNumber=Math.random();
      if(randomNumber<0.5){
        return this.outputA;
      }else{
        return this.outputB;
      }
    }
  },
});
app.mount('#user-goal');
```

<strong>Note:keyword "this", in Vue we could not invoke the properties of data function in methods object directly, but we could use "this" refers the Vue object so that we could invoke the properties by "this"</strong>

<strong>v-html:</strong>
In Vue, when we invoke the properties of string directly, all the tags in string we be ignored as a tag, instead they would also be considered as a string. So when we want output a html code, what should we do is just add a property "v-html", and the value is which html code you need:

```html
<p v-html="outputGoal()"></p>
```

#### Add eventListener by Vue

Like we bind the data to an attribute, we could an property to an element like "v-on:eventListener="expression" to add the related event listener on it:

```html
<button v-on:click="console.log('Hello Vue')"></button>
```

But in fact, we do not want the logic expression appear in HTML code, so we have the methods to invoke, with methods we could put our logic part into the javascript code:

```html
<!-- When we add a event listener, we could add parentheses after the function name or not, they are the same result -->
<button v-on:click="output"></button>
```

```javascript
methods:{
  output(){     //Note:With output(){...},this refers to this Vue object, but in output:()=>{...}, this refers to Window object
    console.log("Hello Vue!");
  }
}
```

Here's the thing: When we use an method, we want set multiple arguments in it, but we still want use like:"e.target", we could use "$e" to specify this argument like this:

```html
<input type="text" v-on:input="setName($e,'Yang')" />
```

```javascript
methods(e,lastName){
  this.name = e.target.value + ' ' + lastName;
}
```

#### Event modifier in Vue:

After we set an event listener, we could add a modifier so that only the listener meet this specific modifier, the event occurs or this modifier will stop some default behaviour:

```html
v-on:keyup.enter="function"
```

So now only the enter and enter key is up, the event will be triggered. Or like this:

```html
v-on:submit.prevent="function"
```

And when this form is submitted, the default behavior will not be triggered.

Sometimes, we need some element only uptdates once we could use "v-once" property in it:

```html
<p v-once>Starting Counter:{{counter}}</p>
<p>Update Counter:{{counter}}</p>
```

So assume we have a button update, each time we click this button, the counter is updated , the Update Counter will always be updated, but the Start Counter will only be updated once caused by "v-once"

With methods, we could achieve the event binding and data binding. But it has little problem: When we just use a function to return value as the value of a element, the any other value changes will cause this function operate:

HTML:

```html
<p>Your name is {{ outputFullName() }}</p>
```

Javascript:

```javascript
methods:{
  outputFullName() {
      const that = this;
      console.log('Hello, fullName is on!');
      if (that.firstName == '' || that.lastName == '') {
        that.fullName = '';
      } else {
        that.fullName = that.firstName + ' ' + that.lastName;
        return that.fullName;
      }
    },
}
```

This time, when we have any other value has changed even though it is not related to this value, this "outputFullName()" still will operate.So it will cause the waste of the memory. To prevent this problem, we could use computed property instead:

```html
<p>Your name is {{ outputFullName}}</p>
```

```javascript
computed:{
  outputFullName() {
      const that = this;
      console.log('Hello, fullName is on!');
      if (that.firstName == '' || that.lastName == '') {
        that.fullName = '';
      } else {
        that.fullName = that.firstName + ' ' + that.lastName;
        return that.fullName;
      }
    },
}
```

But please note that at this time, the outputFullName is no longer a function in component, it becomes an property, so we could not invoke it as a function in the component, so there should not contains the parentheses there.

But there is an watcher there which is an very useful object like the computed property, the firstly for similarity:Both can be used to manipulate the data. The differences: The computed property can be used directly in template, and it can be defined as any name you want, expect that, we could sort or manipulate a large group of data at the same time. However, the watcher can only be named as an exists property name, and it can not be invoked in template directly, a watcher can only lisen one data property at a time, and a watcher can operate multiple times while the computed property can only be operated once. Expect for that, a watcher can have two parameters, newValue and oldValue, so you can easily do something the computed can not or at least is difficult:

```html
<div class="counter">
  <p>Counter:{{ counter }}</p>
  <button v-on:click="add">Add 10</button>
  <button v-on:click="minus">Minus 10</button>
</div>
```

```javascript
watch: {
    counter(value, old) {
      if (value > 50) {
        const that = this;
        setTimeout(() => {
          that.counter = old;
        }, 2000);
      }
    },
  },
```

At this time, the counter is watched, so every time we change the the value of the counter, the watcher will judge if the counter exceeds 50, if the result is true, the setTimeout will work, even though the counter is 100 or more bigger(Because we set only 2 scends after the code will work), the counter will repeatedly operate until the counter is equals to 50 which the computed is hard to do.

#### Shorthand for v-on: and v-bind:

We could simply use @ instead of "v-on:", and just colon(:)to instead of "v-bind:"

### Dynamic styling in Vue

If we ant style the element in Vue, there are two steps: Bind a event listener and use "v-bind" on style attribute, then set an expression:

```html
<div
  class="demo"
  :style="{borderColor:boxAselected?'red':'#ccc'
}"
  @click="boxSelected('A')"
></div>
```

```javascript
data(){
  return{
    boxASelected:false
  }
}
methods:{
  boxSelected(box){
    if(box==='A'){
      this.boxASelected = true
    }
  }
}
```

All right, we found in that way, it's not so easily to read and manage. So we could use the most common way is to add or delete the class name from the element so that we could change the style of the element. Fo those concret class we could simply use "class='classNames...'" attribute to set, and for those dynamic classes we could use ":class='{className:boolean(from data)...}'"

```html
<div
  class="demo"
  :style="{active:boxASelected
}"
></div>
```

Note:There is still has a better shorthand(With array):

```html
<div :style="['demo',{active:boxASelected}]"></div>
```

As we saw, that in that way we still add so many logic into html code, so in order to prevent this ,we just need to add this logic to "computed property" and then invoke it.

<strong>Note: v-model:used to fetch the input value:\<input type="text" v-model="enteredValue(defined in data)"></strong>

#### Rendering Content Conditionally

With "v-if" directive we could render the content conditionally:

```html
<p v-if="Conditon expression">Content</p>
```

Now if the condition is true the content is rendered, or the condition is not, the content will be hidden.

And we see, in javascript we have "else if" and "else". So the same in Vue, we could also use these keywords too, and we will get the same result. But as a whole, they need to be the neighboring for each other, these elements can not be separated bu any other element.

Except for that, we have the "show" directive which has the same functionality. The only two differences:

<li>It is used alone</li>
<li>It won't remove the element instead it just hides them with</li>

#### for in Vue

If we have the if, we will get the for also, so there is the "v-for" here. With we for we could iterate the elements in an array with the keyword of "in":

```html
<li v-for="element in elements">{{element}}</li>
```

The variation for "v-for":

1.When we iterate the something, if we need it's index for each time, what should we do it's follow the pattern:"v-for='(value,index) in elements'", and then you get both values and index for each time

2.Except for arrays, we could also iterate the objects:"v-for='(value,key) in {object}'", the value for value and the key for the keynames. Also we could use get the index by adding an argument:"v-for='(value,key,index) in {object}'"

But in fact, here's an bug in "v-for": When we set a element with this attribute, this element and the subelement in it is not actually bind together, so when we delete this element , all the sub elements won't be deleted and they will be kept on the old position whatever the old position is beyond the current length or the new element has take the old position. In order to solve this problem, we could use ":key='property'" to bind the subelement to the element. But this property should have the unique value, but in fact the index doesn't meet . Because the index will change as the new value has added or old value has been removed.So for arrays, the values in fact can be a good choice, But when the both elements have the same value, this bug will still exist.

As we know, the Vue keep the properties in data dynamic, when we change the value of the property, the property will be changed.but in fact we know the javascript is not working like that, when we set an valueA, and set and valueB, then set valueC eaquals to valueA plus valueB. Lastly we change the value if valueA, the valueC won't be changed, however the Vue would. So what makes Vue could get this behavior. The answer is "Proxy object".

And proxy object is an special javascript object that enables you to create a proxy for another object, which can intercept and redefine fundamental operations for that object.
Here's constructor:

```javascript
const proxy = new Proxy(data, handler);
```

The data is the target object we want keep it dynamic, and the handler is the inner behavior for the values in it.

And behind the handler function we have an set method that has three arguments:targer->refers to the data;key->refers to the keys of all the values;value->refers to the values. And here's an example:

```javascript
let data = {
  value1: 'Hello',
  value2: 'world!',
  result: '',
};
data.result = data.value1 + ' ' + data.value2;
console.log(data.result);
const handler = {
  set(target, key, receiver) {
    if (key === 'value1') {
      target.result = receiver + ' ' + data.value2;
    } else if (key === 'value2') {
      target.result = target.value1 + ' ' + receiver;
    }
  },
};
const proxy = new Proxy(data, handler);
proxy.value2 = 'javascript!';
console.log(data.result);
```

Alough it's a sort of difficult, but it still shows us the secret behind the reactive data in Vue

The template is the html code in the component, we could set them in the html file, also we could set them in the javascript file with the form:

```javascript
template:` html code `,
```

Example:

```html
html...
<div id="app">
  <div
    class="demo"
    :style="{borderColor:boxAselected?'red':'#ccc'
}"
    @click="boxSelected('A')"
  ></div>
</div>
...
```

```javascript
...Vuecode
template:
`<div class="demo"
:style="{borderColor:boxAselected?'red':'#ccc'
}"
  @click="boxSelected('A')"
></div>
`,
data(){
  return{
    boxASelected:false
  }
}
methods:{
  boxSelected(box){
    if(box==='A'){
      this.boxASelected = true
    }
  }
}
...
```

#### <strong>Note:The use of ref->Like the key , ref is an attribute only understand by Vue, it can be hold by any element in the component, when we set an "ref='name'" to this element, we could use "this.$refs.name" to invoke this element directly</strong>

#### Vue Components

When we use create an app, this app will control an html component, and then we we could use this app to generate the same smaller components in the main component with "v-for". But here's an problem: When we use the same module to create the same smaller components, teh would share the same event listeners too, so there would cause so bugs:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vue Basics</title>
    <link
      href="https://fonts.googleapis.com/css2?family=Jost:wght@400;700&display=swap"
      rel="stylesheet"
    />
    <link rel="stylesheet" href="styles.css" />
    <script src="https://unpkg.com/vue@next" defer></script>
    <script src="app.js" defer></script>
  </head>
  <body>
    <header>
      <h1>FriendList</h1>
    </header>
    <section id="app">
      <ul>
        <li v-for="friend in friends" key="id">
          <h2>{{friend.name}}</h2>
          <button @click="toggleDetails">
            {{visibility?"Hide" : "Show"}} Details
          </button>
          <ul v-if="visibility">
            <li><strong>Phone:</strong>{{friend.phone}}</li>
            <li><strong>Email:</strong> {{friend.email}}</li>
          </ul>
        </li>
      </ul>
    </section>
  </body>
  <script src="https://unpkg.com/vue@3"></script>
  <script src="app.js"></script>
</html>
```

```javascript
const app = Vue.createApp({
  data() {
    return {
      visibility: false,
      friends: [
        {
          id: 'manuel',
          name: 'Manuel Lorenz',
          phone: '01234 5678 991',
          email: 'manuel@gmail.com',
        },
        {
          id: 'julie',
          name: 'Julie Jones',
          phone: '09876 543 221',
          email: 'julie@gmail.com',
        },
      ],
    };
  },
  methods: {
    toggleDetails() {
      this.visibility = !this.visibility;
    },
  },
});
app.mount('#app');
```

Because the buttons are all bind the same event listener, they will all do the same thing when any button is clicked. In order to solve this problem, we could use the smaller components:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vue Basics</title>
    <link
      href="https://fonts.googleapis.com/css2?family=Jost:wght@400;700&display=swap"
      rel="stylesheet"
    />
    <link rel="stylesheet" href="styles.css" />
    <script src="https://unpkg.com/vue@next" defer></script>
    <script src="app.js" defer></script>
  </head>
  <body>
    <header>
      <h1>FriendList</h1>
    </header>
    <section id="app">
      <!-- These are so called smaller components that can only be recognized by Vue, and the name is created by us -->
      <friend-contact></friend-contact>
      <friend-contact></friend-contact>
      <friend-contact></friend-contact>
    </section>
  </body>
  <script src="https://unpkg.com/vue@3"></script>
  <script src="app.js"></script>
</html>
```

```javascript
const app = Vue.createApp({
  //This is so called main component
  data() {
    return {};
  },
});
//This this the smaller component
app.component('friend-contact', {
  //This is the template for this component, and of course the main component can also have it's own template.
  //Note: In this template, we do not use "v-for" at first li
  template: `
    <ul>
        <li>
          <h2>{{friend.name}}</h2>
          <button @click="toggleDetails">
            {{visibility?"Hide" : "Show"}} Details
          </button>
          <ul v-if="visibility">
            <li><strong>Phone:</strong>{{friend.phone}}</li>
            <li><strong>Email:</strong> {{friend.email}}</li>
          </ul>
        </li>
      </ul>
    `,
  data() {
    return {
      visibility: false,
      friend: {
        id: 'manuel',
        name: 'Manuel Lorenz',
        phone: '01234 5678 991',
        email: 'manuel@gmail.com',
      },
    };
  },
  methods: {
    toggleDetails() {
      this.visibility = !this.visibility;
    },
  },
});
app.mount('#app');
```

But still we foud some other problem here: Alough now all the smaller components works independently, but we now can just create the component of the same content, and we will handle it in the further lecture.

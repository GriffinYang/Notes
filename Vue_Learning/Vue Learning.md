# Vue Learning

Notes for Vue by Rongxin Yang

## ****Interpolation and Data binding****

Vue could be chosen to control all the UI components in the application, and also can be chosen to control just several UI components. So if we want use Vue to control these components we should make an data binding, usally, that is implemented by id, because id is unique in the application

****create a Vue object****

Firstly we need import a link from Vue in html:

`<script src="https://unpkg.com/vue@3"></script>`

After we import it, we could create a Vue object like this:

```jsx
const app = Vue.createApp({
  data() {
    return {
      customProperty: value,
    };
  },
});
app.mount('selector');
```

Please notice that when we mount a component, we usally use id selector, because id selector can ensure it be a unique one.

**Note:1.mout()->In Vue, any vue object has the lifecyclehook:create,mount,update and destroy, when we create a Vue object, this object in fact is stored in virtual DOM memory, so at this moment, the vue object can’t make any change to the DOM, only we mount this vue object, it can make changes to the DOM, so this method can be called only once.**

**2.When Vue controls an element, the nested elements are also being controlled**

As we saw, in this object, **we could add a data function in it, this function is  set to return all the needed data in this component, these data could use*”{{dataName}}”*  to invoke them directly in HTML, or use “this” keyword to invoke in Vue object. But here's the thing: We could not use curly braces when we want to invoke them in tag attributes, instead we need use *“v-bind:”* to make it.** And there are some other functions  and objects can be add to the component:

```html
<div v-bind:link="customedProperty">{{customedProperty}}</div>
```

**Note:In Vue, we could not output html code directly with curly braces:**

```jsx
data(){
return{
htmlText:'<h2>Hello World!</h2>'
}
}
```

```html
<p>{{gtmlText}}</p>   //Output "<h2>Hello World!</h2>"
<p v-html="htmlText"></p>  //Output "Hello World!" With h2 style
Objects:
```

**Note:v-once→ With this attribute, the data in this element will be locked whatever how the initial data has been changed** 

**methods**: With this  object we could add the functions needed in the component.    

```jsx
methods:{
functionName(){
...
}
functionPara(event,para1,...){
...
}
...
}

```

```html
<div @click="functionName">{{customedProperty}}</div>/*Also use parenthesis when you want pass the parameters,*/
/*When you want to pass a event(e) as a parameter you conld add a '$' sign*/
<div @click="functionPara($event,para1...)">{{customedProperty}}</div>
```

**Event modifier:**  A modifier is used to set the extra limit to the event handler, they are directive postfixes denoted by a dot. And there are six of them are included:

- `.stop`
- `.prevent`
- `.self`
- `.capture`
- `.once`
- `.passive`

```jsx
<!-- the click event's propagation will be stopped -->
<a @click.stop="doThis"></a>

<!-- the submit event will no longer reload the page -->
<form @submit.prevent="onSubmit"></form>

<!-- modifiers can be chained -->
<a @click.stop.prevent="doThat"></a>

<!-- just the modifier -->
<form @submit.prevent></form>

<!-- only trigger handler if event.target is the element itself -->
<!-- i.e. not from a child element -->
<div @click.self="doThat">...</div>

<!-- use capture mode when adding the event listener -->
<!-- i.e. an event targeting an inner element is handled here before being handled by that element -->
<div @click.capture="doThis">...</div>

<!-- the click event will be triggered at most once -->
<a @click.once="doThis"></a>

<!-- the scroll event's default behavior (scrolling) will happen -->
<!-- immediately, instead of waiting for `onScroll` to complete  -->
<!-- in case it contains `event.preventDefault()`                -->
<div @scroll.passive="onScroll">...</div>
```

And still there are ****Key Modifiers and Mouse Button Modifiers():****  

- `.left`
- `.right`
- `.middle`

```jsx
<!-- only call `vm.submit()` when the `key` is `Enter` -->
<input @keyup.enter="submit" />
```

***‘v-bind:’ and ‘v-model’: When we could use ‘v-bind’ to get the data value from vue snd send it to the element. But we could not use it to set it’s value. So if when we use it in some form input elements:***

 ******

```html
/*At this time, we use v-bind and a event handler to sync the "name",
one for get and one for set*/ 
<input type:"text" :value"name" @input="setName"></input>
```

***If we want to make them in once, we could use v-model instead:***

```html
<input type:"text" v-model:value="name"></input>
/*Now when we input in it, the data name would be effected , and when 
the data name is changed the value would also be changed, so it's a sho-
rthand for above
*/
```

**computed:** When we use a function to return a value in the curly braces, any time when the any other value in this component  has changed, this function will be executed even if it is not related with it. Because vue does not know about it so it needs to execute  firstly:

```html
<p>{{age}}</p>
<p>Your name:{{outputName()}}</p>
/*Now even when the age has changed the outputName() would also be 
executed */
```

So it’s a waste of efficiency, then we could use computed to solve this problem:

 Technically , in computed , the data are also the functions, but when we want to invoke them, we do not need to add the parenthesis, we need just invoke it’s reference. 

```jsx
...
computed:{
outputName(){
...
}
}
...
```

```jsx
<p>{{age}}</p>
<p>Your name:{{outputName}}</p>
/*Now age and outputName will no longer be effected by age*/
```

**watch:** Like it’s name, this object is ma de to monitor the all the data in this component, when this data has updated, the inner operations will work.   

                                     

```jsx
watch:{
counter(value){
if(value>50){
...
}
}
}
/*Now if counter's value is greater than
50, the function will operate*/
```

**Dynamic styling:**

The traditional syling is Inline Styling. As we saw it’s a lot bit difficult 

```html
<div :style="{{border-color:":judgement?'red':'none'}}">...</div>
```

So in order to simplify the dynamic styling, we could use add a class to make it:

```html
<div :class="{{demo:true,active:judement...}}">...</div>
```

**v-if:** Use this directive we could make the page renders the content conditionally.

```html
<p v-if="condition">...</p>
/*Now only the condition is true the element
will be rendered */
```

If we have **v-if**, we have the **v-else-if** and **v-else** too, they are the same use of ‘**if-else**’, but please they are directly  the neighbors. However the v-if has the alternative which is v-show. Not like the v-if, it is used stand alone , then it means it won’t got the logic like v-if, it can just contains only one logic in ieach of them. And here’s an another difference:  **v-if** will actually remove the element of that, but the **v-show** just choose to hidden them. That means the latter one will not actually impact the html flow.

```html
<p v-show="condition">...</p>
```

**v-for:** This directive is used to loop through the array or object: ****

```html
<li v-for="item in items">{{ item }}</li>  //just get the value
<li v-for="(item,index) in items">{{ item }}</li> // get the value and index
//When loop through the object 
<li v-for="(item[,key[,index]]) in object">{{ item }}</li>
//it can even loop through the number
<li v-for="item in 10">{{ item }}</li> 
```

When we use v-for , we could use :key=”(unique)property” to make the elements  as a whole:

```html
<li v-for="(item,index) in items" :key="item">
<p>{{item}}-{{index}}</p>
<input></input>
</li>
//Now all the content in li will be considered as a whole. they will 
be always synched 
```

**refs:** A ref means the reference to an element. We could use ref at any tags: div, input, textarea whatever. And all the refs we set will be stored in ‘this.$refs’. And then we could invoke them with the key we set. Because the refs are stored with the form of key-value:

```html
<div ref="name"></div>
```

```jsx
this.$refs.name<=><div ref="name"></div>
//The left hand is equal to the right hand
```

**Template:**

A template is the html part where the this app controls:

```html
<p id="componentId">
...
<div>...</div>
...
</p>
/*It's whole html code is called the template of the app which one 
is recognized with ID*/ 
```

```jsx
const app=Vue.createApp({
template:`
...
<div>...</div>
...
`,
data(){
return{
...
}
},
});
app.mount("#componentId")
```

In the latter chapter  we when we use Vue development tool, we will take any of these seperate app as a component, and that time the template  will represent these whole html part.

**The component:**

In fact, all the vue apps are consists with separated components . We could use “app.component(componentName,component)” to create an component. And then invoke them with’<componentName></componentName>’, at this time all the components will work stand alone.

```jsx
app.component('componentName',
{
template:`template`,
data(){
return{
...
}
},
methods:{
...
}
...
});
```

```html
...
<div>
<componentName></componentName>
</div>
...
```

**The certain use of Vue in Vue-CLI tool:**

In Vue-CLI, we have three main folder: pulic, which one store the index or other html files and some resources. And in ‘src’ folder ,there is one folder in it, it’s called components that store all the needed components file with the extended  name ‘**.vue**’. And there is an ‘main.js’ in the src folder.  

The main structure of vue file:

```jsx
//ComponentName.vue
<template>
...(template)
</template>

<script>
export default{  /*At this point, we export these whole component, and 
then we could import it in main.js */
data(){
 return{
 }
},
...
}
</script>
<style>
...(css)
</style>
```

**The main structure of main.js:**

```jsx
import { createApp } from 'Vue',

import App from './App.vue';
import ComponentName from './components/ComponentName.vue'
const app=createApp(App);
app.component('component-name',ComponentName);
app.mount('#app');
```

**In html:**

```html
...
<div id="app">
...
<component-name></component-name> /*Invoke the component*/
...
</div>
...
<script src="https://unpkg.com/vue@3"></script>
<script src="./main.js"></script>
...
```

**props:**

This is an array, which one the data in it can be invoke directly like the data in data method.  And props are actually customed properties of the component.

```jsx
//CustomedComponent.vue
<template>
...
<h2>propName1</h2>
<li>propName2</li>
<p>propName3</p>
...
</template>
export default{
props:[
'propName1',
'propName2',
'propName3',
...
]
...
}
```

```html
<customed-component
propName1="..."
propName2="..."
propName3="..."
>
</customed-component>
```

**Note: The props are not supported to be manipulated after they are defined. So when we want to change the value of it , we could use an another value receive it’s value and then we could change that middle-value instead.** 

As we said the props can be defined  as an array, or we could define it as an object, that we could set the type of that property and if it is required:

```jsx
props:{
propName1:{
type:String,
requred:true,
},
propName2:{
type:Integer,
requred:true,
},
propName3:{
type:Boolean,
requred:false,
default:.../*With this propery we could ensure that this value won't be empty*/
validator:function(value){
return expression
}
/*We could use validator to validate if this value is valid*/
},
...
}
```

**The customed Method:**

This is a way that makes you could communicate both components and main.js. And that customed method is generated by ‘emit()’ method.

```jsx
//CustomedComponent.vue
<template>
...
<li @eventType="emit("customedMethodName",para1,para2...)">...</li>
...
</template>
```

```jsx
...
<CustomedComponent @customedMethodName="functionName(para1,para2...)"></CustomedComponent>
...
```

So from above, we could understand that in fact a customed method is based on the built-in event, firstly we need bind an event listener to trigger the **emit** method, after we done that, when the main component invokes that customed event created by emit method, we could bind it an specified function. So actually , this customed method connects both parent and child components. 

For a better understanding of your components  by your partners , we could set an array ‘emits’:

```jsx
export default{
...
emits:["customedMethodName",...]
...
}
//Or you could make it an object
emits:{
'customedMethodName':function(para1,para2...){...}
} 
```

**The provide and inject:**

When we want pass the data from the parent component to childen components , we need pass through the data with props voer and over again, the methods as well. So in order to solve this problem , we could use **provide** and **inject** instead. Now let’s see a project:

```jsx
//App.vue
<template>
  <div>
    <active-element
      :topic-title="activeTopic && activeTopic.title"
      :text="activeTopic && activeTopic.fullText"
    ></active-element>
    <knowledge-base :topics="topics" @select-topic="activateTopic"></knowledge-base>
  </div>
</template>

<script>
export default {
  data() {
    return {
      topics: [
        {
          id: 'basics',
          title: 'The Basics',
          description: 'Core Vue basics you have to know',
          fullText:
            'Vue is a great framework and it has a couple of key concepts: Data binding, events, components and reactivity - that should tell you something!',
        },
        {
          id: 'components',
          title: 'Components',
          description:
            'Components are a core concept for building Vue UIs and apps',
          fullText:
            'With components, you can split logic (and markup) into separate building blocks and then combine those building blocks (and re-use them) to build powerful user interfaces.',
        },
      ],
      activeTopic: null,
    };
  },
  methods: {
    activateTopic(topicId) {
      this.activeTopic = this.topics.find((topic) => topic.id === topicId);
    },
  },
};
...

//ActiveElement.vue
<template>
  <section>
    <h2>{{ topicTitle }}</h2>
    <p>{{ text }}</p>
  </section>
</template>

<script>
export default {
  props: ['topicTitle', 'text'],
};
</script>

//KnowledgeElement.vue
<template>
  <li>
    <h3>{{ topicName }}</h3>
    <p>{{ description }}</p>
    <button @click="$emit('select-topic', id)">Learn More</button>
  </li>
</template>

<script>
export default {
  props: ['id', 'topicName', 'description'],
  emits: ['select-topic'],
};
</script>

//KnowledgeGrid.vue
<template>
  <li>
    <h3>{{ topicName }}</h3>
    <p>{{ description }}</p>
    <button @click="$emit('select-topic', id)">Learn More</button>
  </li>
</template>

<script>
export default {
  props: ['id', 'topicName', 'description'],
  emits: ['select-topic'],
};
</script>

//KnowledgeBase.vue
<template>
  <li>
    <h3>{{ topicName }}</h3>
    <p>{{ description }}</p>
    <button @click="$emit('select-topic', id)">Learn More</button>
  </li>
</template>

<script>
export default {
  props: ['id', 'topicName', 'description'],
  emits: ['select-topic'],
};
</script> 
```

As we saw, there in App.vue, we got a knowledge-base component, which one is the parent component of KnowledgeGrid that is the parent of KnowledgeElement, so in order to  pass the topics in App.vue to KnowledgeGrid, we set the props of topics in KnowledgeGird then Set the same props in KnowledgeBase to pass through the topics from App.vue to KnowledgeGrid. It’s a sort of complicated. So we could now use provide-inject instead:

```jsx
//App.vue
<template>
  <div>
    <active-element
      :topic-title="activeTopic && activeTopic.title"
      :text="activeTopic && activeTopic.fullText"
    ></active-element>
    <knowledge-base @select-topic="activateTopic"></knowledge-base>
  </div>
</template>

<script>
export default {
  data() {
    return {
      topics: [
        {
          id: 'basics',
          title: 'The Basics',
          description: 'Core Vue basics you have to know',
          fullText:
            'Vue is a great framework and it has a couple of key concepts: Data binding, events, components and reactivity - that should tell you something!',
        },
        {
          id: 'components',
          title: 'Components',
          description:
            'Components are a core concept for building Vue UIs and apps',
          fullText:
            'With components, you can split logic (and markup) into separate building blocks and then combine those building blocks (and re-use them) to build powerful user interfaces.',
        },
      ],
      activeTopic: null,
    };
  },
  provide() {
    return {
      topics: this.topics,
    };
  },
  methods: {
    activateTopic(topicId) {
      this.activeTopic = this.topics.find((topic) => topic.id === topicId);
    },
  },
};
</script>
...

//KnowledgeGrid 
<template>
  <ul>
    <knowledge-element
      v-for="topic in topics"
      :key="topic.id"
      :id="topic.id"
      :topic-name="topic.title"
      :description="topic.description"
      @select-topic="$emit('select-topic', $event)"
    ></knowledge-element>
  </ul>
</template>

<script>
export default {
  inject: ['topics'],
  emits: ['select-topic'],
};
</script>

//KnowledgeBase
<template>
  <section>
    <h2>Select a Topic</h2>
    <knowledge-grid
      @select-topic="$emit('select-topic', $event)"
    ></knowledge-grid>
  </section>
</template>

<script>
export default {
  emits: ['select-topic'],
};
</script> 
```

If we want the same effect to the methods, we could provide-inject at the same way:

```jsx
//App.vue
<template>
  <div>
    <active-element
      :topic-title="activeTopic && activeTopic.title"
      :text="activeTopic && activeTopic.fullText"
    ></active-element>
    <knowledge-base @select-topic="activateTopic"></knowledge-base>
  </div>
</template>

<script>
export default {
  data() {
    return {
      topics: [
        {
          id: 'basics',
          title: 'The Basics',
          description: 'Core Vue basics you have to know',
          fullText:
            'Vue is a great framework and it has a couple of key concepts: Data binding, events, components and reactivity - that should tell you something!',
        },
        {
          id: 'components',
          title: 'Components',
          description:
            'Components are a core concept for building Vue UIs and apps',
          fullText:
            'With components, you can split logic (and markup) into separate building blocks and then combine those building blocks (and re-use them) to build powerful user interfaces.',
        },
      ],
      activeTopic: null,
    };
  },
  provide() {
    return {
      topics: this.topics,
      SelectTopic: this.activateTopic,
    };
  },
  methods: {
    activateTopic(topicId) {
      this.activeTopic = this.topics.find((topic) => topic.id === topicId);
    },
  },
};
</script>

//KnowledgeElement.vue
<template>
  <li>
    <h3>{{ topicName }}</h3>
    <p>{{ description }}</p>
    <button @click="SelectTopic(id)">Learn More</button>
  </li>
</template>

<script>
export default {
  inject: ['SelectTopic'],
  props: ['id', 'topicName', 'description'],
  emits: ['select-topic'],
};
</script>
```

**Global And Local components:**

We could set a global component in **main.js** that can be invoked at at any other components of the main App but if we have so a lot of components in an App, and some components are just used once in a single component , we could also consider to set it a local component in that specific component where is using it. And here;s the how global components are configured:

```jsx
 import { createApp } from 'vue';

import App from './App.vue';
/*These all components can be invoked at any components of the App,but they must all be 
 downloaded at once*/
import ActiveElement from './components/ActiveElement.vue';
import KnowledgeBase from './components/KnowledgeBase.vue';
import KnowledgeElement from './components/KnowledgeElement.vue';
import KnowledgeGrid from './components/KnowledgeGrid.vue';

const app = createApp(App);

app.component('active-element', ActiveElement);
app.component('knowledge-base', KnowledgeBase);
app.component('knowledge-element', KnowledgeElement);
app.component('knowledge-grid', KnowledgeGrid);

app.mount('#app');
```

And here’s how a local component is configured: 

```jsx
<template>
  <ul>
    <knowledge-element
      v-for="topic in topics"
      :key="topic.id"
      :id="topic.id"
      :topic-name="topic.title"
      :description="topic.description"
    ></knowledge-element>
  </ul>
</template>
<script>
import KnowledgeElement from './KnowledgeElement.vue';
export default {
  components: {
    KnowledgeElement,
  },
  inject: ['topics'],
};
</script>
```

**Scoped Styling:**

In Vue components, all the styling in a component are global, it means when we use a style in any sub-component, this style will be applied to all the rest of the components too, but sometimes we we do not want it, so we could add a attribute “**scoped**” to prevent  that happens.

```jsx
//SomeComponent.vue
...
<style scoped>
//Now these styles are just used in this single component 
* {
  box-sizing: border-box;
}
html {
  font-family: sans-serif;
}
body {
  margin: 0;
}
section {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  margin: 2rem auto;
  max-width: 40rem;
  padding: 1rem;
  border-radius: 12px;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
}

li {
  border-radius: 12px;
  border: 1px solid #ccc;
  padding: 1rem;
  width: 15rem;
  margin: 0 1rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

h2 {
  margin: 0.75rem 0;
  text-align: center;
}

button {
  font: inherit;
  border: 1px solid #c70053;
  background-color: #c70053;
  color: white;
  padding: 0.75rem 2rem;
  border-radius: 30px;
  cursor: pointer;
}

button:hover,
button:active {
  background-color: #e24d8b;
  border-color: #e24d8b;
}
</style>
```

**The use of Slots:**

When we want to nest one component in another one, we could not put that one in another one directly, because Vue does not know where should that nested one should be rendered, then we could use slot to make it:

```jsx
//container.vue
<template>
...
<div>
<slot></slot>
</div>
...
</template>
...
<style>
div{
...
}
</style>

//nested.vue
<template>
<container>
...(html code)
</container>
</template>
<script>
...
</script>
```

**The named slot:**

When we want to own more than one slot in a template we could give them names as identifier. But please note that there at most can have one unanmed slot in a template as a default slot:

```jsx
/*container.vue*/
<template>
<header>
<slot name="header"></slot> /*The value of this name attribute will be
 an identifier of this slot so that we could make Vue know where should 
render this slot in the template */
/*If we put something into this slot, when the invoker doesn't use this named slot, these so-called 
default content will be displayed */
</header>
<slot></slot>/*This unnamed slot will be a default slot*/
</template>

```

This is how we use the named slot:

```jsx
//SomeComponent.vue
//We could use '#' to substitute the 'v-slot' 
<template>
<container>
  <template v-slot:header>/*Invoke the named slot with 'v-slot:name'*/
   <header>....</header>
   <other-component></other-component> 
  </template>
  <template v-slot:default>/*Use default slot*/
   ...(Some content)
  </template>
</container>
</template>
```

**Slot communication:**

When we use a slot, if we want pass the essential data , that’s can be a problem. So in order to make it, we could use  **slot props** to get the props in the slot:

```jsx
/*container.vue*/
<template>
<li for="item in items" :key="item">
<slot :propsName="goal" :anotherProp="...">
</slot> 
<li>
</template>
```

```jsx
//SomeComponent.vue
<template>
<container /*[#default='slotProps]*/>
  <template #default="slotProps">//When there's only one slot in it, we 
could set this "#default='slotProps'" directly  in **container**
   //Get prop in object
   <h2>{{slotProps.item}}</h2>
   //Get prop in array
   <p>{{slotProps['anotherProp']}}</p>
  </template>
</container>
</template>
```

Note: there’s an **$slots** object that contains all the slots we set, we could use it to judge if specific slot shold be rendered in the component :

```jsx
//SomeComponent.vue 
<template>
<container>
  /*Here, we use v-if to choose whether should the component render
this header slot, if that component does not use the header slot, this 
header slot will not be rendered in the html
 */
  <template v-slot:header v-if="$slots.header">
   <header>...</header>
   <other-component></other-component> 
  </template>
  <template v-slot:default>/*Use default slot*/
   ...(Some content)
  </template>
</container>
</template>
```

```jsx
//SomeComponent.vue
/*In this comonent, we do not use header slot, so it won't be rendered in
html*/
<template>
<container>
  <template v-slot:default="slotProps">
   //Get prop in object
   <h2>{{slotProps.item}}</h2>
   //Get prop in array
   <p>{{slotProps['anotherProp']}}</p>
  </template>
</container>
</template>
```

**Note:We could use “#” insted of “v-slot”, and if in a compont we use just only one slot, we could  set it in this way:**

```markdown
<container #default="slotProps">
content...
</container>
```

**Dynamic component:**

As we know, we could use v-if to judge if the specific component should be displayed:

```markdown
<target-component1 v-if="selectedcomponent==='target-component1'"></target-component1>
<target-component2 v-if="selectedcomponent==='target-component2'"></target-component2>
```

In the code above, we use selected component this String to judge which component should

 be rendered, but as we saw, if there aren't alot of components we should toggle, use v-if repeatedly can be annoying, so we could use “**:is”** instead.

```markdown
<component :is="selectedcomponent"></component>
```

**Now, the component in the String selectedcomponent will be displayed. So we could change the displayed component by change the String content as before in a better way**

**Keep-Alive component:**

As the example above, if we set a  switcher to switch the components to display, after we switch the component to the another component, the content in the previous component will be erased , so in order to handle that, we could add an keep-alive component to keep the previous alive:

```markdown
<keep-alive>
<component :is="selectedcomponent"></component>
</keep-alive>
```

Now, if we switch among  the different components, we could keep the previous component alive

**The use of Teleport:**

Sometimes we may run into the following scenario: a part of a component's template belongs to it logically, but from a visual standpoint, it should be displayed somewhere else in the DOM, outside of the Vue application.

The most common example of this is when building a full-screen modal, so we could use teleport this component to the body, and the problem solved. And the actual use is below:

```markdown
//Initial MyMpdal.vue
<script>
export default {
  data() {
    return {
      open: false
    }
  }
}
</script>

<template>
  <button @click="open = true">Open Modal</button>

  <div v-if="open" class="modal">
    <p>Hello from the modal!</p>
    <button @click="open = false">Close</button>
  </div>
</template>

<style scoped>
.modal {
  position: fixed;
  z-index: 999;
  top: 20%;
  left: 50%;
  width: 300px;
  margin-left: -150px;
}
</style> [](http://modal.So)
```

**With teleport:**

```markdown
/*Template*/
<button @click="open = true">Open Modal</button>

<Teleport to="body">
  <div v-if="open" class="modal">
    <p>Hello from the modal!</p>
    <button @click="open = false">Close</button>
  </div>
</Teleport>
```

**The difference betweeen v-model & $refs:**

With v-model, we could esaily ge the value of the input, and with ref we could also do that, but there are little differences: When we set an input and set it a type of number, then the v-model will retrive a actual number, but the latter one will return us a string:

```markdown
<input type="number" v-model="getValue" ref="input"/>
/*
With getValue:Return a number
With this.refs.input.value: Return a string
*/
```

**Install the router in the Vue:**

Why should we install the router int the Vue? Because in Vue, actually we are just achieving all the functions in that one page, so we need only one html page in our project, and then to update the page with the the communication of components . So in Vue project, all the manipulates are resolved  in this same page, so when we want to share a specified page of the project, we can only get the that only URL, because whatever we done , we are just in that concrete URL. So when we share that URL, the others will be directed  to the default main page, that not we want to see, so in order to solve this problem we use the router to seperate all the pages individually so that we could ge the dynamic URL when the page has changed.

```jsx
//Install the router in Vue 
npm install --save vue-router
```

After we install the Router in the Vue project, we could make the configuration in the main.js firstly:

```jsx
//Configuration in main.js
/*Import the needed functions createRouter and createWebHistory*/
import {createRouter,createWebHistory} from 'vue-router';
/*Import the components that need the control of router, and here's an 
actual example:
 */
import TeamsList from '@/components/teams/TeamsList';
import UsersList from '@/components/users/UsersList';

//Create the router object
const router=createRouter({
/*With this method we could make browser remember the pages we had 
browsed so that we could back the previous page*/  
history:createWebHistory(),
/*This array is the core of the router, with this array we let the Vue 
know which component should own a router and which the route it shoud
uses
*/ 
routes:[
    { path:'/teams',component:TeamsList},
    { path:'/users',component:UsersList}
  ]
});
```

Actually, the page changes often happens in App.vue, so that component let the page change happen should be substituted by build-in component**‘’router-view’’.**  

```jsx
<template>
<!--  <the-navigation @set-page="setActivePage"></the-navigation>-->
  <the-navigation></the-navigation>
  <main>
<!--    <component :is="activePage"></component>-->
    <router-view></router-view>
  </main>
</template>

<script>
// import TeamsList from './components/teams/TeamsList.vue';
// import UsersList from './components/users/UsersList.vue';
import TheNavigation from './components/nav/TheNavigation.vue';

export default {
  components: {
    TheNavigation,
    // TeamsList,
    // UsersList,
  },
  data() {
    return {
      // activePage: 'teams-list',
      teams: [
        { id: 't1', name: 'Frontend Engineers', members: ['u1', 'u2'] },
        { id: 't2', name: 'Backend Engineers', members: ['u1', 'u2', 'u3'] },
        { id: 't3', name: 'Client Consulting', members: ['u4', 'u5'] },
      ],
      users: [
        { id: 'u1', fullName: 'Max Schwarz', role: 'Engineer' },
        { id: 'u2', fullName: 'Praveen Kumar', role: 'Engineer' },
        { id: 'u3', fullName: 'Julie Jones', role: 'Engineer' },
        { id: 'u4', fullName: 'Alex Blackfield', role: 'Consultant' },
        { id: 'u5', fullName: 'Marie Smith', role: 'Consultant' },
      ],
    };
  },
  provide() {
    return {
      teams: this.teams,
      users: this.users,
    };
  },
  //methods: {
  //  setActivePage(page) {
  //   this.activePage = page;
  //  },
 //},
};
</script>
```

In the example above, initially “**<component :is="activePage"></component>**” **controls where shoud the page should be displayed** when we did not use the router, but as we saw, after we use the router, we commented it then replace it with “**<router-view></router-view>**”.

Then we could edit the TheNavigation.vue that actually **controls which page should be displayed** on the page. And here’s the **TheNavigation.vue** file.

```jsx
<template>
  <header>
    <nav>
      <ul>
        <li>
<!--          <button @click="setActivePage('teams-list')">Teams</button>-->
       <router-link to='/teams'>Teams</router-link>
        </li>
        <li>
<!--          <button @click="setActivePage('users-list')">Users</button>-->
          <router-link to='/users'>Users</router-link>
        </li>
      </ul>
    </nav>
  </header>
</template>

<script>
export default {
  // emits: ['set-page'],
  // methods: {
    // setActivePage(page) {
    //   this.$emit('set-page', page);
    // },
  // },
};
</script>

<style scoped>
header {
  width: 100%;
  height: 5rem;
  background-color: #11005c;
}

nav {
  height: 100%;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

li {
  margin: 0 2rem;
}
//Represents router-link
**a {
  text-decoration: none;
  font: inherit;
  background: transparent;
  border: 1px solid transparent;
  cursor: pointer;
  color: white;
  padding: 0.5rem 1.5rem;
  display: inline-block;
}

a:hover,
a:active {
  color: #f1a80a;
  border-color: #f1a80a;
  background-color: #1a037e;
}**
</style>
```

As we saw, the original tag button has been replaced by `router-link` , and we saw : in this router-link , we have a **to** directive that controls which router we should toggle with this router-link. And here’s the thing:”The router-link returns us a ‘**<a></a>’ that meets** the name of **link,”**

In fact, to change the page, we can not only use the **router-link,** we could also use a button or something else to trigger the eventListener to change the page by using the buid-in object→**$router,**and here’s the UserList.vue:

```jsx
 **** <template>
/*Now, after we click the button, the page will redirect to '/teams' 
router*/
  <button @click='changePage'>Change to Team Page</button>
  <ul>
    <user-item v-for="user in users" :key="user.id" :name="user.fullName" :role="user.role"></user-item>
  </ul>
</template>

<script>
import UserItem from './UserItem.vue';

export default {
  components: {
    UserItem,
  },
  inject: ['users'],
  methods:{
    changePage(){
      this.$router.push("./teams")
    }
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 2rem auto;
  max-width: 20rem;
  padding: 0;
}
</style>
```

We have experienced the **push** method in **$router.** And now we could introduce the another object:

**$route**  (**There is no ‘r’** ), with this object, we could use the property **params** to get the router patameter, so what is the route parameter? The router parameter is the value after colon symbol(**:**) 

in a route:

```jsx
//This teamId is a route parameter of this route
{path:"/teams/:teamId",component:TeamMembers}
```

So now, if we want set the route as **“/teams/t1”,** this **t1** will be considered as the parameter of this route, and at this time, we could use `this.$route.params.teamId` to get the parameter from the route, then we could use this feature to do more things dynamic on route:

```jsx
<template>
  <section>
    <h2>{{ teamName }}</h2>
    <ul>
      <user-item
        v-for="member in members"
        :key="member.id"
        :name="member.fullName"
        :role="member.role"
      ></user-item>
    </ul>
  </section>
</template>

<script>
import UserItem from '../users/UserItem.vue';

export default {
  inject:[
    'teams',
    'users'
  ],
  components: {
    UserItem
  },
  data() {
    return {
      // teamName: 'Test',
      // members: [
      //   { id: 'u1', fullName: 'Max Schwarz', role: 'Engineer' },
      //   { id: 'u2', fullName: 'Max Schwarz', role: 'Engineer' },
      // ],
      teamName:"",
      members:[]
    };
  },
  created() {
    const selectedId=this.$route.params.teamId;
    const selectedTeam=this.teams.find(team=>team.id===selectedId);
    const members=selectedTeam.members;
    const selectedMembers=[];
    for(const member of members){
      const selectedUsers=this.users.find(user=>user.id===member)
      selectedMembers.push(selectedUsers)
    }
    this.members=selectedMembers;
    this.teamName=selectedTeam.name

  }
};
</script>

<style scoped>
section {
  margin: 2rem auto;
  max-width: 40rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 1rem;
  border-radius: 12px;
}

h2 {
  margin: 0.5rem 0;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
```

As we see,  in the initial edition. We could just add a concrete object in this component, bu now , after we use the parameter, we could use this parameter to check out the specified information on this component from the object that provided by the **App.vue:**

```jsx
//App.vue
data() {
    return {
      // activePage: 'teams-list',
      teams: [
        { id: 't1', name: 'Frontend Engineers', members: ['u1', 'u2'] },
        { id: 't2', name: 'Backend Engineers', members: ['u1', 'u2', 'u3'] },
        { id: 't3', name: 'Client Consulting', members: ['u4', 'u5'] },
      ],
      users: [
        { id: 'u1', fullName: 'Max Schwarz', role: 'Engineer' },
        { id: 'u2', fullName: 'Praveen Kumar', role: 'Engineer' },
        { id: 'u3', fullName: 'Julie Jones', role: 'Engineer' },
        { id: 'u4', fullName: 'Alex Blackfield', role: 'Consultant' },
        { id: 'u5', fullName: 'Marie Smith', role: 'Consultant' },
      ],
    };
  },
```

And actually , the **route-link** is also a tag and it can be considered as a simple **a tag,** so we could also use the Vue directive on it, and here’s how we could use the Vue directive on it to make the page redirect to another page:

```jsx
//TeamItem.vue
<template>
  <li>
    <h3>{{ name }}</h3>
    <div class="team-members">{{ memberCount }} Members</div>
    <router-link :to="'/teams/'+id">View Members</router-link>
  </li>
</template>

<script>
export default {
  props: ['id','name', 'memberCount'],
};
</script>

<style scoped>
li {
  margin: 1rem 0;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  border-radius: 12px;
  padding: 1rem;
}

li h3 {
  margin: 0.5rem 0;
  font-size: 1.25rem;
}

li .team-members {
  margin: 0.5rem 0;
}

a {
  text-decoration: none;
  color: white;
  display: inline-block;
  padding: 0.5rem 1.5rem;
  background-color: #11005c;
}

a:hover,
a:active {
  background-color: #220a8d;
}
</style>
```

Here’s teamList.vue

```jsx
<template>
  <ul>
    <teams-item
      v-for="team in teams"
      :key="team.id"
      :id="team.id"
      :name="team.name"
      :member-count="team.members.length"
    ></teams-item>
  </ul>
</template>

<script>
import TeamsItem from './TeamsItem.vue';

export default {
  components: {
    TeamsItem,
  },
  inject: ['teams'],
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 2rem auto;
  max-width: 40rem;
  padding: 0;
}
</style>
```

### The use of **Query:**

We could set a query at a named route object:

```jsx
{name:"team-member",params:{teamId:this.id},**query:{sort:"asc"}**}
```

Then what would we get is after the the initial route **URL,** the **** **“?sort=asc” added:**

```jsx
 ****http://192.168.8.63:8080/teams/t1**?sort=asc**
```

And then we could invoke it with this.”**$route.query”:**

```jsx
this.**$route.query:Object:{sort:"asc"}**
```

### Named Components:

 As we learned: The router-view we declared should be registered in **App.vue,** and if we want to register the **route-view** in the specified component , we could use the nested component.In fact, we could set not only one **router-view** in one component. For example, we could set another compoent **TeamFooter** in the `path:'/teams'` above. We could do like that:

```jsx
{ path:'/teams',components:{
//Now, there are TeamList and TeamFooter components in the teams router.
      default: TeamsList,footer:TeamFooter
},
children:[{name:"team-member", path:"/teams/:teamId",component:TeamMembers,props:true},]
},
```

Then we could add the **TeamFooter** into the **App.vue as** a **router-view:**

```jsx
 **<template>
  <the-navigation></the-navigation>
  <main>
    <router-view></router-view>
//Note:There can be only one unnamed reouter-view as the default router-view
  </main>
  <router-view name='footer'></router-view>
</template>**
```

### Router Guards(functions)

`router.beforeEach`, `router.afterEach`, **beforeRouteEnter**, **beforeRouteLeave**, **beforeRouteUpdate**

**router.beforeEach:** It is used in App.vue, when any router has updated, this function will be triggered, and in this function, there’s arrow function as a parameter, and the parameters in this arrow function is “**to**,**from** and **next**”. The “**to”** parameter indicates the previous router, and the “**from**” parameter indicates the target router. As for the last parameter it is a function actually, we could invoke it to let the router to direct to the target router, also we could input a “**false**” to prevent the router direct to the target router, or we could input a **router path** or **router objec**t to direct the router to another router. 

**router.afterEach:** This function has the same parameters as the previous one except the parameter **next**, because this function will only operate after the router has changed. So after that, this function can not control the router anymore, then the **next** function won’t work as well.

Both functions above are created in the **App.vue** this main component, and they will observe all the router update in the component and get the detail for each update. And still there are some other functions that will only work for a single component, and they are created in each small component that needs it:

**beforeRouteEnter**: This function will work when the router will be updated to the router of this component:

```jsx
beforeRouteEnter(to,from,next){
    console.log("Before Cmp Entered");
    console.log(to,from);  /***to** will indicate to this current router, and **from** will 
    indicate to the previous router*/
    next()
  } 
```

**beforeRouteUpdate**: This function will work when the current will update:

```jsx
beforeRouteUpdate(to,from,next){
    console.log("before Route update ran");
    console.log(to,from);
    next()
}
```

Note:Actually, both **to** and **from** are ****the router object, so we could invoke the property of them.  ****

**Meta data:**

In every router object, we could add a property: **meta,** and the value of that can be, include the object, and we could use the **routerObject.meta.value** to get the value of the meta:

```jsx
  { path:'/teams',
    components:{
    default: TeamsList,footer:TeamFooter
    },
    meta:{
    param1:value1,
    param2:value2,
    }
  },
/*We could get it like this: **routerObject.meta.param1**, then we will get the result:**value1***/
```

### Animation with VUE

Actually, we could complete most of the animations with CSS, and just use Javascript to start it. But here’s a problem: The animation we created can be summarized as one method:”Toggle the class” . When we want to create an animation, we will firstly add or remove a class of the element so that this element can toggle the specified css properties. If this element is always on the screen, that’s easy to control, we could use the **transition** to make it moving smoothly. But when this element initially does not on the screen, only after we use the animation, it will be added to the **DOM.** Yeah, we still could give it a smoothly appearance by using transition, but we could not give it a smoothly disappearance. So in order to give the element a smoothly animation both in appearance and disappearance. We could use the **transition** component in VUE to help us:

**The use of <transition></transition>** 

The transition is used to wrap the specified compoent we want to have the full animation on it. But here’s a basic rule: A transition can only contains one element in it, even this inner component has not only one child in it is also not allowed, only an exception for that is when those components can be guaranteed to add only one of them to the DOM at one time.And here’s some conditions:

```jsx
//It is allowed, cause there is only one element P in it
<transition>
      <p v-if="paragraphVissible">This is the content of the block</p>
</transition>

//It is also been allowed cause v-if and v-else can ensure that there will only be one button displays on the screen
<transition name="button" mode="out-in">
      <button @click="showButton" v-if="!buttonIsVisible">Show</button>
      <button @click="hideButton" v-else>Hide</button>
</transition>
//It is not allowed, because although there is just a base-modal component in transition, but in base-modal there are two elements in it
**<transition>
  <base-modal @close="hideDialog" :open="dialogIsVisible">
    <p>This is a test dialog!</p>
    <button @click="hideDialog">Close it!</button>
  </base-modal>
</transition>** 
//Base-Modal:
<template>
  <div class="backdrop" @click="$emit('close')"></div>
    <dialog open>
      <slot></slot>
    </dialog>
</template>
```

**The lifecycle of Transition:**

The transition has two main states(Enter and Leave), and each state has three states:

![transition.png](Vue%20Learning%20d856b981bada48e0b722abf40b5fdeae/transition.png)

With these six states, we can have a complete animation lifecycle, both **from** and **to** state are the start and final states of these two main states. And **active** is the logical statement of this main state, and here’s two examples:

```jsx
//template:
<transition>
      <p v-if="paragraphVissible">This is the content of the block</p>
</transition>
//state statement:
.v-enter-from {
  opacity: 0;
  transform: translateY(-30px);
}
.v-enter-active {
  transition: all 0.5s ease-out;
}
.v-enter-to {
  opacity: 1;
  transform: translateY(0);
}
.v-leave-from {
  opacity: 1;
  transform: translateY(0);
}
.v-leave-active {
  transition: all 1s ease-in-out;
}
.v-leave-to {
  opacity: 0;
  transform: translateY(-30px);
}

//template;
<transition name="button" mode="out-in">
      <button @click="showButton" v-if="!buttonIsVisible">Show</button>
      <button @click="hideButton" v-else>Hide</button>
</transition>
//state statement:
.button-enter-from {
  opacity: 0;
}
.button-enter-active {
  transition: all 0.3s ease-out;
}
.button-enter-to {
  opacity: 1;
}
.button-leave-from {
  opacity: 1;
}
.button-leave-active {
  transition: all 0.3s ease-in-out;
}
.button-leave-to {
  opacity: 0;
}
```

  

**Named transition:**

As the example above, we found:The transition can be named, and after we named that transition, we could control it’s state with the name:.**button**-enter-from. And when that transition does not be named, we can use .**v**-enter-from to control. Actually, we could also set the state as a specified name we want in the transition component with the directive:

```jsx
<transition **button**-enter-from="p-enter">
      <p v-if="paragraphVissible">This is the content of the block</p>
</transition>
//Then we could uss p-enter instead of **button**-enter-from
```

In this transition, we could set a **mode** directive, this directive controls how the previous element and current element transits on animation. So this directive is used when there are multiple elements in the transition, and there are two values of it:**out-in:**the current element will appear after the previous element disappeared, and **in-out** will take the conversion.

**Javascript Hook on transition:**

We can hook into the transition process with JavaScript by listening to events on the `<Transition>`
component:

```html
//hooks:
<Transition
  @before-enter="onBeforeEnter"
  @enter="onEnter"
  @after-enter="onAfterEnter"
  @enter-cancelled="onEnterCancelled"
  @before-leave="onBeforeLeave"
  @leave="onLeave"
  @after-leave="onAfterLeave"
  @leave-cancelled="onLeaveCancelled"
>
  <!-- ... -->
</Transition>

//explains:

// called before the element is inserted into the DOM.
    // use this to set the "enter-from" state of the element
    onBeforeEnter(el) {},

    // called one frame after the element is inserted.
    // use this to start the animation.
    onEnter(el, done) {
      // call the done callback to indicate transition end
      // optional if used in combination with CSS
      done()
    },

    // called when the enter transition has finished.
    onAfterEnter(el) {},
    onEnterCancelled(el) {},

    // called before the leave hook.
    // Most of the time, you shoud just use the leave hook.
    onBeforeLeave(el) {},

    // called when the leave transition starts.
    // use this to start the leaving animation.
    onLeave(el, done) {
      // call the done callback to indicate transition end
      // optional if used in combination with CSS
      done()
    },

    // called when the leave transition has finished and the
    // element has been removed from the DOM.
    onAfterLeave(el) {},

    // only available with v-show transitions
    onLeaveCancelled(el) {}

```

These hooks can be used in combination with CSS transitions / animations or on their own.

When using JavaScript-only transitions, it is usually a good idea to add the `:css="false"` prop. This explicitly tells Vue to skip auto CSS transition detection. Aside from being slightly more performant, this also prevents CSS rules from accidentally interfering with the transition.

With `:css="false"`, we are also fully responsible for controlling when the transition ends. In this case, the `done` callbacks are required for the `@enter` and `@leave` hooks. Otherwise, the hooks will be called synchronously and the transition will finish immediately:

```jsx
**enter(el, done) {
      console.log('enter');
      console.log(el);
      let round = 1;
      this.enterInterval = setInterval(() => {
        el.style.opacity = round * 0.01;
        round++;
        if (round > 100) {
          clearInterval(this.enterInterval);
          done();
        }
      }, 20);
    },**
```

And here’s some other hooks that used in the component:

```jsx
enterCancelled(el) {
      console.log(el);
      clearInterval(this.enterInterval);
    },
    leaveCancelled(el) {
      console.log(el);
      clearInterval(this.leaveInterval);
    },
    beforeEnter(el) {
      console.log('beforeEnter');
      console.log(el);
      el.style.opacity = 0;
    },
    afterEnter(el) {
      console.log('afterEnter');
      console.log(el);
    },
    beforeLeave(el) {
      console.log('beforeLeave');
      console.log(el);
      el.style.opacity = 1;
    },
    leave(el, done) {
      console.log('leave');
      console.log(el);
      let round = 1;
      this.leaveInterval = setInterval(() => {
        el.style.opacity = 1 - round * 0.01;
        round++;
        if (round > 100) {
          clearInterval(this.leaveInterval);
          done();
        }
      }, 20);
    },
    afterLeave(el) {
      console.log('afterLeave');
      console.log(el);
    },
```

**The use of transition-group:**

Sometimes we want to add the animation to the list, as we know: A list contains a lot of elements, however **a transition component can just contains one element in it**, so we need use **transition-group** insetad. Because although a list will contains al lot of elements, but there would just have one animation there at the same time.And here’s an example for **transition-group;**

```jsx
<template>
  <div class="ul-box">
    <!-- <ul> -->
    <transition-group tag="ul" name="list">
      <li v-for="user in users" :key="user" @click="deleteUser(user)">
        {{ user }}
      </li>
    </transition-group>

    <!-- </ul> -->
    <input type="text" class="newName" ref="newUser" />
    <button @click="addUser">Add User</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      users: ['John', 'Jerry', 'Sarah', 'Sam', 'Peter'],
    };
  },
  methods: {
    addUser() {
      this.users.unshift(this.$refs.newUser.value);
      this.$refs.newUser.value = '';
    },
    deleteUser(user) {
      this.users = this.users.filter((usr) => {
        return usr !== user;
      });
    },
  },
};
</script>

<style scoped>
ul {
  list-style: none;
}
li {
  padding: 5px;
  border: 1px solid #ccc;
  text-align: center;
  margin-bottom: 10px;
}
.list-enter-from {
  opacity: 0;
  transform: translateX(-50px);
}
.list-enter-active {
  transition: all 1s ease-out;
}
.list-enter-to {
  opacity: 1;
  transform: translateX(0px);
}
.list-leave-from {
  opacity: 1;
  transform: translateX(0px);
}
.list-leave-active {
  transition: all 1s ease-in;
  **position: absolute;  //Make leave animation more smoothly** 
}
.list-leave-to {
  opacity: 0;
  transform: translateX(-50px);
}
**.list-move {
  transition: transform 1s ease; //Make add animation more smoothly
}**
</style>
```

Note: We note that when we use the **transition-group** we could set the parent tag in the **tag directive**.

**The animation on router**

In fact, when we use the latest Vue, we will found a warning when we use transition wrap the **router-view.**Even though the function works correctly.Here’s an example:

```jsx
//main.js
import { createApp } from 'vue';
import { createRouter, createWebHistory } from 'vue-router';

import App from './App.vue';
import BaseModal from './components/BaseModal.vue';
import AllUsers from './pages/AllUsers.vue';
import CourseGoals from './pages/CourseGoals.vue';
import UserList from './components/UserList.vue';
const app = createApp(App);
const router = createRouter({
  history: createWebHistory(),
  routes: [
    {
      path: '/',
      component: AllUsers,
      children: [
        {
          name: 'users',
          path: '/',
          component: UserList,
        },
      ],
    },
    { path: '/goals', component: CourseGoals },
  ],
});

app.use(router);
app.component('base-modal', BaseModal);

router.isReady().then(() => {
  app.mount('#app');
});

//AllUsers.vue
<template>
  <div class="container">
    <h2>All Users</h2>
    <router-view></router-view>
    <router-link to="/goals">Course Goals</router-link>
  </div>
</template>
```

And here’s the result;

![final.png](Vue%20Learning%20d856b981bada48e0b722abf40b5fdeae/final.png)

# **An Important Note on Animated Route Changes**

When animating route changes as shown in the previous lecture, there's **one important thing** you have to keep in mind:

Your route components **must NOT have multiple root elements!**

For example, if your route component looked like this:

`1. <template>
2.   <section>
3.     <h2>Section 1</h2>
4.   </section>
5.   <section>
6.     <h2>Section 2</h2>
7.   </section>
8. </template>`

The `<transition>` component would **not be able to animate route changes** and you would get a **warning** in the JS console in your browser dev tools.

**Why?**

Because you must not forget that `<transition>` **needs exactly one child element** (*with the special exceptions you learned about in this module*).

If your route component has **multiple root elements**, `<transition>` in the end **has multiple children** and that is the **problem**.

Hence, if you want to transition between routes, you need to ensure that your route components **have exactly one root element** - for example:

`1. <template>
2.   <div>
3.     <section>
4.       <h2>Section 1</h2>
5.     </section>
6.     <section>
7.       <h2>Section 2</h2>
8.     </section>
9.   </div>
10. </template>`

## Vuex:

**Vuex is a libriary for managing global state:**

This state indicates the **data** we used, and there are two kinds of states:

**Local State**: Affect the local component; **E.g**: entered user input,show/hide the container.

**Global State**:Affect the entire app/multiple components,**E.g:**user auth status, shopping cart items.

**Installing Vuex in the project:** 

```jsx
**npm install —save vuex**
```

**Import createStore:**

```jsx
//main.js
import { createStore } from 'vuex';
...

const store = createStore({
//content...
});
...
app.use(store);
```

### **state:**

The **state method** is is the key component in the **content of the store object**, it can be considered as the global **data** for the entire app. We could invoke the data  in it with **$this.store.state.stateName:**

```jsx
state() {
    return {
      counter: 0,
    };
  }, **** 
```

### mutations:

**This object** can be considered as a **global methods for the state**, we could use these methods with **$this.store.commit(’mutationName’)**:

```jsx
mutations: {
    addOne(state) {
      state.counter++;
    },
    addTwo(state) {
      state.counter += 2;
    },
  },
```

**Actual example:**

```jsx
//mian.js
import { createApp } from 'vue';
import { createStore } from 'vuex';

import App from './App.vue';

const app = createApp(App);
const store = createStore({
  state() {
    return {
      counter: 0,
    };
  },
  mutations: {
    addOne(state) {
      state.counter++;
    },
  },
});
app.use(store);

app.mount('#app');
//TheCounter.vue
<template>
  <h3>{{ counter }}</h3>
</template>

<script>
export default {
  computed: {
    counter() {
      return this.$store.state.counter;
    },
  },
};
</script>
//App.vue
<template>
  <base-container title="Vuex">
    <the-counter></the-counter>
    <button @click="addOne">Add One</button>
  </base-container>
</template>

<script>
import BaseContainer from './components/BaseContainer.vue';
import TheCounter from './components/TheCounter.vue';

export default {
  components: {
    BaseContainer,
    TheCounter,
  },
  computed: {
    counter() {
      return this.$store.state.counter;
    },
  },
  methods: {
    addOne() {
      this.$store.commit('addOne');
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

body {
  margin: 0;
}
</style>
```

### mutations with parameters:

In mutation, we could add the needed parameters into a **payload object** which is the **second argument** of a mutation:

```jsx
//....
increase(state, payLoad) {
      state.counter += payLoad.value;
      console.log(payLoad.text);
    },
//....
```

Then we could use it like that:

```jsx
//....
data() {
    return {
      number: 10,
    };
  },
methods: {
    add() {
      // this.$store.commit('increase', { value: this.number, text: 'Hello World!' });
      this.$store.commit({
        type: 'increase',
        value: this.number,
        text: 'Hello World!',
      });
      this.number += 10;
    },
  },
//....
```

And we found there are **two ways** to add the arguments to the **payLoad and using commit:**

**1: (’mutationName’,{payload arguments properties…})**

**2: ({ type:’mutationName’,payload argument1:value,payload argument2:value…}**

### The use of Getters:

the getters is another object int the **store object** that also based on the **mutations**,the reason why we need getters is because we need more complicated mutations on data, and each getter could use the second parameter to communciate with other getters, then we could also use **this.$store.getters.getterName** to get the specified **getter.** But we should also note ****that each getter shall return an value,so we could also consider the getters as the **global** **Computed.**

```jsx
//main.js
//...
getters: {
    tripleIncrease(state) {
      return state.counter * 3;
    },
    limitedIncrease(state, getters) {
      const finalCounter = getters.tripleIncrease;
      if (finalCounter < 10) {
        return 10;
      } else if (finalCounter > 100) {
        return 100;
      }
      return finalCounter;
    },
  },
//...

//TheCounter.vue
<template>
  <h3>{{ counter }}</h3>
</template>

<script>
export default {
  computed: {
    counter() {
      //   return this.$store.state.counter;
      return this.$store.getters.limitedIncrease;
    },
  },
};
</script>
//AnotherCounter.vue
<template>
  <h3>{{ counter }}</h3>
</template>

<script>
export default {
  computed: {
    counter() {
      return this.$store.getters.tripleIncrease;
    },
  },
};
</script>
```

### **The use of actions:**

Actually, the mutations does not support asynchronous behaviours, one mutation should wait the previous mutation has done it’s work first. So when we use a mutation with **setTimeOut**, it will still work, but may cause some errors since the Vuex does not allow it. So in order to support the asynchronous behaviours we could use the **actions** instead. The actions is also an object as the **getteres**, and because the **actions** is made to  asynchronous actions that the **getters** can not do. So the actions invoke the getters with it’s arguments:

```jsx
//main.js
...
actions:{
  increment(context){
    setTimeout(function(){
     context.commit('increment'),
    2000),
  },
  increase(context,payload){
    context.commit('increase',payload);
  }
},
...

```

If we want to invoke the actions, we could use the **dispatch** instead **commit**. 

The **context** argument in the actions can not only use the **commit this method**, it can invoke all the Vuex features:**dispatch**,**commit**,**getters** and **state.** So a context can use all the Vuex features in fact. 

At before, we use this.$store.getters.getterName to invoke the getter:

```jsx
...
computed: {
    counter() {
      return this.$store.getters.tripleIncrease;
    },
  },
...
```

But in fact, we could use mapGetter instead:

```jsx
import { mapGetter } from 'vuex';
export default {
computed:{
  ...mapGetters(['tripleIncrease'])
}
}
```

With mapGetter, we could use the getterName directly and do not use the computed method to retrive the getter, and in this mapGetter, we could add more than one getter in it. **(Before we use it we need import it first)**

And with the same way , we could use **mapActions** too: 

```jsx
methods: {
    add() {
      this.$store.dispatch({
        type: 'increase',
        value: this.number,
        text: 'Hello World!',
      });
      this.number += 10;
    },

methods: {
   ...mapActions(['increase'])
```

And there the **mapActions** and **mapGetters** can also be put in an object:

```jsx
...mapActions({
  inc:'increase'
})
```

Then we could use **inc** **instead** of the increase. And the **appGetters** can do the same thing.

**Note**:**After we use appActions we could directly invoke the the actions after the event,so if we want to pass the arguments, we need use tan object:**

```jsx
<component @click="inc({arg1:value,arg2:value...})"></component>
```

### **The use of modules**

For a better management of our stores, we could put tthem into different modules, and a module is an object. In each module, we could put the related **state**,**mutations**,**getters** and **actions** into it and then we could invoke it in **createStore** object with **modules objecct**:

```jsx
//initial main.js
import { createApp } from 'vue';
import { createStore } from 'vuex';

import App from './App.vue';

const app = createApp(App);
const store = createStore({
  state() {
    return {
      counter: 0,
      isAuth: false,
    };
  },
  mutations: {
    addOne(state) {
      state.counter++;
    },
    addTwo(state) {
      state.counter += 2;
    },
    increase(state, payLoad) {
      state.counter += payLoad.value;
      console.log(payLoad.text);
    },
    setAuth(state) {
      state.isAuth = !state.isAuth;
    },
  },
  getters: {
    tripleIncrease(state) {
      return state.counter * 3;
    },
    limitedIncrease(state, getters) {
      const finalCounter = getters.tripleIncrease;
      if (finalCounter < 10) {
        return 10;
      } else if (finalCounter > 100) {
        return 100;
      }
      return finalCounter;
    },
   isAuth(state) {
      return state.isAuth;
    },
  },
});
app.use(store);

app.mount('#app');

//new main.js
import { createApp } from 'vue';
import { createStore } from 'vuex';

import App from './App.vue';

const app = createApp(App);
**const counter = {
  state() {
    return {
      counter: 0,
    };
  },
  mutations: {
    addOne(state) {
      state.counter++;
    },
    addTwo(state) {
      state.counter += 2;
    },
    increase(state, payLoad) {
      state.counter += payLoad.value;
      console.log(payLoad.text);
    },
  },
  getters: {
    tripleIncrease(state) {
      return state.counter * 3;
    },
    limitedIncrease(state, getters) {
      const finalCounter = getters.tripleIncrease;
      if (finalCounter < 10) {
        return 10;
      } else if (finalCounter > 100) {
        return 100;
      }
      return finalCounter;
    },
  },
};**
**const auth = {
  state() {
    return {
      isAuth: false,
    };
  },
  mutations: {
    setAuth(state) {
      state.isAuth = !state.isAuth;
    },
  },
  getters: {
    isAuth(state) {
      return state.isAuth;
    },
  },
};**
***const store = createStore({
  modules: {
    numbers: counter,
    authentication: auth,
  },
});***
app.use(store);

app.mount('#app');
```

But after we use the modules, every **state and getters** in each store will be separated,so if we want to use them ,s we need add some extra arguments in the getters:

```jsx
getters:{
  getterName(state,getters,rootState,rootGetters){   //Now this rootState and rootGetters can get the other state and getters from ather modules
  ....
  }
}
```

**Namespaced**:

After we use the modules, maybe we want the the **actions** or **mutations** with the same name. But As we know, the same name will cause the name conflicts so in order to solve this conflict we could add a **namespaced** property and set it as **true** to make this module seperate from the rest of the modules completely so that we could define the same **mutations** and **acttions**.

However, after we set the namespaced as true. We need some marks for the components to identify which namespaced module shou it use. This mark can be a “namespace/getters” in a square notation after we want invoke a getter:

```jsx
counter() {
      return this.$store.getters['numbers/tripleIncrease'];
    },    
```

If there we want to use the mutations we can do the same thing:

```jsx
methods: {
    addTwo() {
      this.$store.commit('numbers/addTwo');
    },
   add() {
      this.$store.commit({
        type: 'numbers/increase',
        value: this.number,
        text: 'Hello World!',
      });
      this.number += 10;
    },
  },
```
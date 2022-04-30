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
      <p>{{ courseGoal }}</p>
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
      vueLink: 'http://vuejs.org/',
    };
  },
});
app.mount('#user-goal');
```

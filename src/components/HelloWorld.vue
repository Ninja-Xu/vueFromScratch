<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2 :title="title" v-if="seen" @click="clickHandler" :class="{ active: true }" class="text-danger">magic seen</h2>
    <h2 v-else>I am hidden...</h2>
    <ul class="tode-list">
      <li is="todo-item" v-for="({todo, id}, index) in todos" :key="id" v-bind="todo" @finish="removeTodo(index)"></li>
    </ul>
    <input type="text" v-model="newtodo">
    <button @click="addTodo">add todo</button>
    <br>
    <input type="text" v-model="textInput">
    <p>Using v-html directive: <span v-html="rawHtml"></span></p>
    <hr>
    <template v-if="loginType === 'username'">
      <label>Username</label>
      <input placeholder="Enter your username" key="11">
    </template>
    <template v-else>
      <label>Email</label>
      <input placeholder="Enter your email address" key="2">
    </template>
    <button @click="exchange">Change login type</button>
    <div v-for="(value, prop, index) in object" :key="index">
      {{ index }}-{{ prop }}: {{ value }}
    </div>
    <div @click.capture="conso($event)">
      <button @click.stop="addone($event)">add 1</button>
    </div>
    <p>{{ count }}</p>
    <hr>
    <select name="fruit" id="fruit" v-model="favorite">
      <option v-for="item of fruit" :value="item" :key="item">{{ item }}</option>
    </select>
    <span>Your favorite fruit is: {{ favorite }}</span>
    <br>
    <select name="fruits" id="fruits" multiple v-model="favorites">
      <option v-for="item of fruit" :value="item" :key="item">{{ item }}</option>
    </select>
    <span>Your favorite fruits are: {{ favorites }}</span>
    <br>
    <input type="checkbox" id="check" v-model="ischecked">
    <label for="check">{{ ischecked }}</label>
    <br>
    <input type="checkbox" v-model="yesorno" true-value="Chiness" false-value="Japaness">
    <span>You choose {{ yesorno }}</span>
    <br>
    <input type="text" v-model.number="age">
    <comp :content.sync="dd"></comp>
    <input type="text" :value="password" @input="password = $event.target.value">
    <br>
    <my-checkbox value="my own initial value" :checked="foo" @change="val => {foo = val}"></my-checkbox>
    <hr>
    <!-- slot -->
    <div>
      <h1>I am parent.</h1>
      <my-slot>
        <p>lalala</p>
      </my-slot>
    </div>
    <app-layout>
      <h1 slot="header">I am header</h1>
      <div>
        <p>I am content</p>
      </div>
      <h3 slot="footer">I am footer</h3>
    </app-layout>
    <hr>
    <div id="demo">
      <button v-on:click="show = !show">
        Toggle
      </button>
      <transition name="slide-fade">
        <p v-if="show">hello</p>
      </transition>
    </div>
    <div id="example-2">
      <button @click="show = !show">Toggle show</button>
      <transition name="bounce">
        <p v-if="show">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris facilisis enim libero, at lacinia diam fermentum id. Pellentesque habitant morbi tristique senectus et netus.</p>
      </transition>
    </div>
    <hr>
    <div v-focus:foo.baz="1 + 1" v-color:color="'red'"></div>
  </div>
</template>

<script>
var mixin = {
  created: function () {
    console.log('混入对象的钩子被调用')
  }
}
export default {
  name: 'HelloWorld',
  components: {
    'todo-item': {
      template: `<div>
        {{ title }}:: {{ content }}
        <span title="finish this item" @click="$emit('finish')">X</span>
      </div>`,
      props: {
        title: Number,
        content: {
          validator: function (str) {
            return str.length < 15
          }
        }
      }
    },
    'comp': {
      template: `<div @click="clickHandler">{{ content }}</div>`,
      props: ['content'],
      methods: {
        clickHandler: function () {
          this.$emit('update:content', 123)
        }
      }
    },
    'my-checkbox': {
      model: {
        prop: 'checked',
        evemt: 'change'
      },
      props: {
        checked: Boolean,
        value: String
      },
      template: '<p><input type="checkbox" :checked="checked"><span>{{checked}}</span></p>'
    },
    'my-slot': {
      template: `<div>
          <h1>I am child component.</h1>
          <slot>
            I am not show if parent has content...
          </slot>
        </div>`
    },
    'app-layout': {
      template: `<div class="container">
        <header>
          <slot name="header"></slot>
        </header>
        <main>
          <slot></slot>
        </main>
        <footer>
          <slot name="footer"></slot>
        </footer>
      </div>`
    }
  },
  directives: {
    focus: {
      bind: function (el, binding, vnode) {
        var s = JSON.stringify
        let { name, value, expression, arg, modifiers } = binding
        el.innerHTML = `
          name: ${s(name)}<br>
          value: ${s(value)}<br>
          expression: ${s(expression)}<br>
          arg: ${s(arg)}<br>
          modifiers: ${s(modifiers)}<br>
          vnode: ${Object.keys(vnode).join('-')}
        `
        console.log('binding')
      },
      inserted: function (el, binding) {
        console.log('inserted')
        console.log(binding)
        el.focus()
      }
    },
    color: function (el, binding) {
      let { arg, value } = binding
      el.style[arg] = value
    }
  },
  mixins: [mixin],
  created: function () {
    console.log('组件钩子被调用')
  },
  data () {
    return {
      show: true,
      foo: true,
      password: '***',
      dd: 'I want ',
      newtodo: '',
      age: 23,
      yesorno: 'Japaness',
      ischecked: false,
      fruit: ['apple', 'pear', 'peach', 'watermelon'],
      favorite: 'peach',
      favorites: [],
      count: 0,
      loginType: 'username',
      rawHtml: '<span style="color:red">This should be red.</span>',
      msg: 'Hello World',
      title: `You visited this page in ${new Date().toLocaleString()}`,
      seen: false,
      textInput: 'hello world',
      todos: [
        {
          todo: {
            title: 1,
            content: 'go shopping'
          },
          id: 1
        },
        {
          todo: {
            title: 2,
            content: 'go fishing'
          },
          id: 2
        }
      ],
      nextTodoId: 3,
      object: {
        firstName: 'John',
        lastName: 'Smith',
        age: 25
      }
    }
  },
  methods: {
    clickHandler: function () {
      alert(this.textInput)
    },
    exchange: function () {
      this.loginType = this.loginType === 'username' ? 'email' : 'username'
    },
    addone: function (event) {
      console.log(event.target.tagName)
      this.count += 1
    },
    conso: function (event) {
      console.log(event.currentTarget.tagName)
    },
    removeTodo: function (index) {
      this.todos.splice(index, 1)
    },
    addTodo: function () {
      this.todos.push({
        todo: {
          title: this.nextTodoId,
          content: this.newtodo
        },
        id: this.nextTodoId
      })
      this.nextTodoId++
      this.newtodo = ''
    }
  },
  watch: {
    textInput: function (curr, prev) {
      console.log(prev, curr)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.active {
  background: red;
}
.text-danger {
  font-style: italic;
}
.fade-enter-active, .fade-leave-active {
  transition: all .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
.slide-fade-enter-active {
  transition: all 2s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active for below version 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}
.bounce-enter-active {
  animation: bounce-in .5s;
}
.bounce-leave-active {
  animation: bounce-in .5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(1);
  }
}
</style>

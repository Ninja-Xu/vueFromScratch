<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2 :title="title" v-if="seen" @click="clickHandler" :class="{ active: true }" class="text-danger">magic seen</h2>
    <h2 v-else>I am hidden...</h2>
    <ul class="tode-list">
      <li v-for="{todo, id} in todos" :key="id">
        {{ id }}: {{ todo }}
        {{ seen ? 'YES' : 'NO'}}
      </li>
    </ul>
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
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
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
          todo: 'go shopping',
          id: 1
        },
        {
          todo: 'go fishing',
          id: 2
        }
      ],
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
</style>

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
    <template v-if="loginType === 'username'">
      <label>Username</label>
      <input placeholder="Enter your username" key="1">
    </template>
    <template v-else>
      <label>Email</label>
      <input placeholder="Enter your email address" key="2">
    </template>
    <button @click="exchange">Change login type</button>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
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
      ]
    }
  },
  methods: {
    clickHandler: function () {
      alert(this.textInput)
    },
    exchange: function () {
      this.loginType = this.loginType === 'username' ? 'email' : 'username'
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

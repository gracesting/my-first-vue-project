<template>
  <div id="app">
    <!-- v-text: h1元素的内容要渲染为msg -->
    <h1 v-text="msg"></h1>
    <input v-model="newItem" v-on:keyup.enter="addNew">
    <!--<input v-model="newItem" v-on:keyup.enter="addNew">-->
    <ul>
      <li v-for="item in items" v-bind:class="{active: item.isFinished}" v-on:click="toggleFinish(item)">{{item.label}}</li>
    </ul>
    <p>child tells me: {{childWords}}</p>
    <component-a msgFromFather = 'from father' v-on:son-say-sth="listenToMyson"></component-a>
  </div>
</template>

<script>
import Store from './store'
import componentA from './components/ComponentA'
console.log(Store)
export default {
  data() {
    return {
      msg: 'msg from main page',
      items: Store.fetch(),
      newItem: '',
      childWords: ''
    }
  },
  components: {componentA},
  watch: {
    items: {
      handler: function (items) {
          Store.save(items)
      },
      deep: false
    }
  },
  methods: {
    toggleFinish(item) {
      console.log(item)
    },
    addNew() {
      this.items.push({
        label: this.newItem,
        isFinished: false
      })
      this.newItem = ''
    },
    listenToMyson(msg) {
      this.childWords = msg
    }
  }
}
</script>

<style>
  .active {
    text-decoration: underline;
  }
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

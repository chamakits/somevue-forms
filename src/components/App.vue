<template>
  <div id="app">
    <header>
      My app
    </header>
    <main>
      <div>
        <textarea class="current-text">{{currentText}}</textarea> 
      </div>
      <div>
        
      </div>
      <input v-model="newTodo" type="text" placeholder="Today I need to..." />
      <button v-on:click="addTodo">Add</button>
      <div v-for="item in items">
        <input type="checkbox" :checked="item.done"/>{{item.todo}}  
      </div>
      <date-picker v-model="dateTimeVal" lang="en" type="datetime" format="YYYY-MM-DD hh:mm:ss a" :minute-step="10" ></date-picker>
    </main>
    <footer></footer>
  </div>
</template>

<script>
import DatePicker from 'vue2-datepicker';
export default {
  components: { DatePicker },
  
  computed: {
    currentText: function() {
      return `abc '${this.dateTimeVal} '`;
    }
  },
  
  data: function() {
    return {
      newTodo: null,
      items: [{
        todo: "Eat cake", done: false
      }],
      dateTimeVal: new Date()
    }
  },
  
  methods: {
    addTodo: function() {
      this.items.push({
        todo: this.newTodo,
        done: false
      })
    }
  }
}
</script>

<style>
  @import "../style/normalize.css";
  
  
  body {
    margin: 0;
  }
  
  main {
    padding: 1rem;
  }
  
  header {
    padding: 1rem;
    background: whitesmoke;
  }
  
  button {
    padding: .5rem 1rem;
    border: 1px solid #ddd;
    background: transparent;
  }
  
  textarea.current-text {
    height: 10em;
    width: 40em;
  }
</style>
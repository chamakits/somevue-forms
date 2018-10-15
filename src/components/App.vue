<template>
  <div id="app">
    <header>
      My app
    </header>
    <main>
      <div>
        <textarea class="current-text" v-model="currentText"></textarea> 
      </div>
      <div>
        <div>
          <input v-model="toPerson" type="text" placeholder="Person to email" required/>
        </div>
        <div>
          <input v-model="introPart" type="text" placeholder="Intro part"/>
        </div>
        <div>
          <input v-model="prefixDayTime" type="text" placeholder="Prefix date time"/>
        </div>
        <div>
          <date-picker 
          v-model="dateTimeVal" lang="en" type="datetime" 
          format="YYYY-MM-DD hh:mm:ss a" :minute-step="30" ></date-picker>
        </div>
        <div>
          <input v-model="postfixDayTime" type="text" placeholder="Postfix date time"/>
        </div>
        <div>
          <input v-model="exitPart" type="text" placeholder="After date time"/>
        </div>
        <div>
          <input v-model="fromPerson" type="text" placeholder="From Person"/>
        </div>
      </div>
    </main>
    <footer></footer>
  </div>
</template>

<script>
import DatePicker from "vue2-datepicker";
import moment from "moment"
export default {
  components: { DatePicker },

  computed: {
    currentText: function() {
      return ` Hi ${this.toPerson},

${this.introPart}

${this.prefixDayTime} ${this.dateTimeValFormat} ${this.postfixDayTime}
${this.exitPart}

Regards,
${this.toPerson}`;
      return `abc '${this.dateTimeVal} '`;
    },
    dateTimeValFormat() {
      const datePrefixFormat = moment(this.dateTimeVal).format("MMM Do, h:mm a");
      const datePostfixFormat = moment(this.dateTimeVal).add(1, 'hours').format("h:mm a")
      return `${datePrefixFormat} - ${datePostfixFormat}`;
    }
  },

  created() {
    this.dateTimeVal = new Date();
  },
  data: function() {
    return {
      toPerson: "",
      introPart: "Thanks for getting back to me!",
      prefixDayTime: "How does",
      dateTimeVal: new Date(),
      postfixDayTime: "sound?",
      exitPart: "Are you available then?",
      fromPerson: ""
    };
  },

  methods: {
    addTodo: function() {
      this.items.push({
        todo: this.newTodo,
        done: false
      });
    }
  }
};
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
  padding: 0.5rem 1rem;
  border: 1px solid #ddd;
  background: transparent;
}

textarea.current-text {
  height: 10em;
  width: 40em;
}
</style>
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
          <!-- <date-picker 
          v-model="dateTimeVal" lang="en" type="datetime" 
          format="YYYY-MM-DD hh:mm:ss a" :minute-step="30" ></date-picker> -->

          <datetime
            type="datetime"
            v-model="dateTimeValString"
            input-class="my-class"
            value-zone="America/New_York"
            :format="{ year: 'numeric', month: 'long', day: 'numeric', hour: 'numeric', minute: '2-digit', timeZoneName: 'short' }"
            :phrases="{ok: 'Continue', cancel: 'Exit'}"
            :hour-step="1"
            :minute-step="30"
            use12-hour
            auto
            ></datetime>
            <!-- <datetime v-model="dateTimeValString"></datetime> -->

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
      <div>
        <button v-on:click="saveValues">Save values</button>
      </div>
      <div>
        <div>
          <button v-on:click="refreshValues">Refresh values</button>
        </div>
        <div>
          <hr>
          <div v-for="pastValue in pastValues" :key="pastValue.timeSaved">
            <hr>
            <button v-on:click="setValue(pastValue)">Load</button>
            <p>{{pastValue}}</p>
          </div>
        </div>
      </div>
    </main>
    <footer></footer>
  </div>
</template>

<script>
import Vue from 'vue'
import DatePicker from "vue2-datepicker";
import moment from "moment";
import PouchDB from "pouchdb";

import {Datetime} from 'vue-datetime'
// You need a specific loader for CSS files
import 'vue-datetime/dist/vue-datetime.css'
Vue.use(Datetime);

export default {
  components: { DatePicker, datetime: Datetime },
  // components: { DatePicker },

  computed: {
    currentText: function() {
      return ` Hi ${this.toPerson},

${this.introPart}

${this.prefixDayTime} ${this.dateTimeValFormat} ${this.postfixDayTime}
${this.exitPart}

Regards,
${this.fromPerson}`;
    },
    dateTimeValFormat() {
      const datePrefixFormat = moment(this.dateTimeValString).format(
        "MMM Do, h:mm a"
      );
      const datePostfixFormat = moment(this.dateTimeValString)
        .add(1, "hours")
        .format("h:mm a");
      return `${datePrefixFormat} - ${datePostfixFormat}`;
    },
    dataValues: function() {
      return {
        toPerson: this.toPerson,
        introPart: this.introPart,
        prefixDayTime: this.prefixDayTime,
        dateTimeValString: this.dateTimeValString,
        postfixDayTime: this.postfixDayTime,
        exitPart: this.exitPart,
        fromPerson: this.fromPerson
      };
    }
  },

  created() {
    this.dateTimeValString = new Date().toString();
    this.db = new PouchDB("data");
    this.db.get("pastValues").then(doc => {
      // return this.db.remove(doc._id, doc._rev);
    }).catch(error => {
      return this.db.put({
        _id: "pastValues",
        data: []
      });
    });
  },
  data: function() {
    return {
      toPerson: "",
      introPart: "Thanks for getting back to me!",
      prefixDayTime: "How does",
      // dateTimeVal: new Date(),
      dateTimeValString: "",
      postfixDayTime: "sound?",
      exitPart: "Are you available then?",
      fromPerson: "",
      db: null,
      pastValues: []
    };
  },

  methods: {
    saveValues: function() {
      this.db.get("pastValues").then(pastValues => {
        if (pastValues.data == undefined) {
          pastValues.data = [];
        }
        // pastValues.data.push(this.dataValues);
        const toInsert = this.dataValues;
        toInsert.timeSaved = new Date();
        pastValues.data.unshift(toInsert);
        return this.db.put({
          _id: "pastValues",
          _rev: pastValues._rev,
          data: pastValues.data
        });
      });
    },
    refreshValues: function() {
      this.db.get("pastValues").then(pastValues => {
        this.pastValues = pastValues.data;
      });
    },
    setValue: function(value) {
      this.toPerson = value.toPerson;
      this.introPart = value.introPart;
      this.prefixDayTime = value.prefixDayTime;
      this.dateTimeValString = value.dateTimeValString;
      this.postfixDayTime = value.postfixDayTime;
      this.exitPart = value.exitPart;
      this.fromPerson = value.fromPerson;
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

input {
  width: 20em;
}
</style>
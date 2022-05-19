<template>
  <div id="app">
    <h1 id="title">{{ title }}</h1>
    <h1 class="list-group-item" >
       <strong></strong>{{ currentDate()}}
    </h1>

    <ul v-if="entries">
      <li class="entry-list" v-for="entry in entries" :key="entry.id">
        <span class="entry-time">
        {{ entry[0] }}, {{ entry[1].replaceAll("/", ".") }}</span>
        <h2 class="entry-titele">{{ entry[2] }}</h2>
        <h3 class="entry-text">{{ entry[3] }}</h3>
        <br />
      </li>
    </ul>
    <h1 v-else>NO EVENTS; SORRY!</h1>

    <div id="footer">
        <img :src="imgLink1" />

        <img v-bind:src="imgLink2" />

        <img :src="imgLink3" />
    </div>
  </div>

</template>

<script>
import axios from "axios"; // axios is a library fo making HTTP request

export default {
  name: "App",
  data() {
    return {
          title: "Welcome to Opportunity",
          sheet_id: "1a81aI0Y8ViZO0tI92h2YSMqVQJ8hmNNMyMylXgvwiU4",
          api_token: "AIzaSyA-qeDXOhEeQDA0vQf7LgkF7DQtGnAtmAU",
          entries: [],
          imgLink1: require("../src/assets/stadt_zürich.png"),
          // logoWidth1: 100,
          imgLink2: require("../src/assets/opportunity.png"),
          // logoWidth2: 100,
          imgLink3: require("../src/assets/sag.png"),
          // logoWidth3: 100, */
    };
  },
  computed: {
    gsheet_url() {
      return `https://sheets.googleapis.com/v4/spreadsheets/${this.sheet_id}/values:batchGet?ranges=A2%3AE100&valueRenderOption=FORMATTED_VALUE&key=${this.api_token}`;
    },
  },
  methods: {
    getData() {
          axios.get(this.gsheet_url).then((response) => {
          this.entries = response.data.valueRanges[0].values;
      });
    },

    currentDate() {
          const current = new Date();
          const day = current.getDate();
          const month = current.getMonth() + 1;
          const year = current.getFullYear();
          const dateTime = day + "." + month + "." + year;

      if (month < 10) {
          return day + "." + "0" + month + "." + year;
      }
          return dateTime;
    },

    refreshData() {
          this.currentDate();
          this.getData();
    },
  },

  mounted() {

          this.refreshData();
          setInterval(() => {
          this.refreshData();
          }, 1800000);
    //this.currentDate = this.printcurrentDate();
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@500&display=swap");

#app {
          font-family: "Inter", Arial, Helvetica, sans-serif;
          margin: 60px;
          list-style-type: none;
}

body {
  background-color: rgb(223, 241, 247);
}

#title {
          font-family: "Inter", Arial, Helvetica, sans-serif;
          font-size: 70px;
          padding: 40px;
          font-weight: bold;
}

/*.list {
  list-style-type: none;
}*/

.list-group-item {
          color: rgb(136, 124, 127);
          text-align: left;
          font-size: 50px;
          padding: 40px;
}

h1 {
          color: rgb(14, 13, 13);
          text-align: auto;
          margin: 50px;
}

.entry-time {
          color: red;
          font-size: 40px;
}

.entry-list {
          list-style-type: none;
          font-family: "inter", Arial, Helvetica, sans-serif;
          color: rgb(244, 136, 183);
          font: medium;
          background-color: rgb(109, 55, 196);
          width: auto;
          border: 0px;
          padding: 20px;
          margin-left: 50px;
          margin-right: 80px;
          margin-top: 30px;
          margin-bottom: 30px;
}

#footer {
          display: flex;
          justify-content: space-between;
          box-sizing: border-box;
          position: fixed;
          bottom: 0;
          left: 0px;
          padding: 15px;
          background-color: #ffffff;
          margin: 0px;
          width: 100%;
}

#footer img {
          height: 50px;
}
</style>

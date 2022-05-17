<template>
  <div id="app">
    <h1 id="title">{{ title }}</h1>

    <ul>
      <h1 class="list-group-item" v-schow="currentDate">
        <strong></strong>{{ currentDate }}
      </h1>
      <li class="entry-list" v-for="entry in entries.slice(1)" :key="entry">
        <span class="entry-time">{{ entry[0] }}, {{ entry[1] }}</span>
        <h2 class="entry-time">{{ entry[2] }}</h2>
        <h3 class="entry-time">{{ entry[3] }}</h3>
        <br />
      </li>
    </ul>

    <div id="footer">
      <img :src="imgLink1" />

      <img v-bind:src="imgLink2" />

      <img :src="imgLink3" />
    </div>
  </div>

  <!-- <h1 class="list-group-item" v-show="date">
        <strong></strong> {{ date }}
      </h1>
      <p>{{ entries.data.valueRanges[0].values }}</p>
    </ul>

    <ul>
      <li class="list">
        <div id="text1">
           <h2>14:00</h2>
            <h3>Basisbeschätigung Besuch</h3>
            <p>Interessierte für den zweiten Kurs werden uns besuchen</p> -->
  <!--<h2>{{ time1 }}</h2>
          <h3>{{ title1 }}</h3>
          <p>{{ descriprion1 }}</p>
        </div>

        <div id="text2">
          <h2>14:00</h2>
          <h3>Basisbeschätigung Besuch</h3>
          <p>Interessierte für den zweiten Kurs werden uns besuchen</p>
          <h2>{{ time2 }}/h2>
            <p>{{ title2 }}</p>
            <p>{{ descriprion2 }}</p> -->
  <!--</div>

        <div id="text3">
          <h2>14:00</h2>
          <h3>Basisbeschätigung Besuch</h3>
          <p>Interessierte für den zweiten Kurs werden uns besuchen</p>
          <h2>{{ time3 }}</h2>
            <p>{{ title3}}</p>
            <p>{{ descriprion3 }}</p> -->
  <!--</div>
      </li>
    </ul>
    <div id="footer">
      <img :src="imgLink1" :width="logoWidth1" />

      <img v-bind:src="imgLink2" :width="logoWidth2" />

      <img :src="imgLink3" :width="logoWidth3" />
    </div>
  </div> -->
</template>

<script>
import axios from "axios"; // axios is a library fo making HTTP request

export default {
  name: "App",
  data() {
    return {
      title: "Welcome to Opportunity",
      currentDate: "",
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
      return `https://sheets.googleapis.com/v4/spreadsheets/${this.sheet_id}/values:batchGet?ranges=A1%3AE100&valueRenderOption=FORMATTED_VALUE&key=${this.api_token}`;
    },
  },
  methods: {
    getData() {
      axios.get(this.gsheet_url).then((response) => {
        this.entries = response.data.valueRanges[0].values;
      });
    },
    /*updateCurrentDate() {},
  },*/
    printcurrentDate: function () {
      return new Date().toLocaleDateString();
    },
  },

  mounted() {
    this.getData();
    this.currentDate = this.printcurrentDate();
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@500&display=swap");

#app {
  font-family: "Inter", Arial, Helvetica, sans-serif;
  margin: 60px;
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

.list {
  list-style-type: none;
}

.list-group-item {
  color: rgb(135, 124, 127);
  text-align: left;
}

h1 {
  color: rgb(0, 0, 0);
  text-align: auto;
  margin: 50px;
}

.entry-list {
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

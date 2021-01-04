<template>
  <div id="app">
    <div class="banner">
      <img
        src="https://vuejs.org/images/logo.png"
        width="100"
        alt="vue"
        class="logo"
      />
      <h1>Welcome to Vue.js</h1>
    </div>
    <h2>Your Visitor Id is: {{ visitorId }}</h2>
    <button v-on:click="callServerAPI">Get Visit History</button>
    <h3>{{ responseSummary }}</h3>
    <pre class="response">{{ serverData }}</pre>
  </div>
</template>

<script>
import FingerprintJS from "@fingerprintjs/fingerprintjs-pro";

// If you would like to hide your API keys, you can put them in a .env file.
// You can also simply whitelist the addresses from which you will be making the calls in your user dashboard
// For the server API key, we reccommend setting up an Auth Header instead, you can read more in the docs:
// https://dev.fingerprintjs.com/docs/server-api
const BROWSERAPIKEY = "P7vQXeD8G1VlmDAwwFir";
const SERVERAPIKEY = "9MLmWT6ziZqePf9JrDZP";

// This will limit the amount of visits the api queries
const limitTo = 10;

export default {
  name: "app",
  data: function () {
    return {
      visitorId: "waiting...",
      serverData: "",
      responseSummary: "",
    };
  },
  // When mounted, this vue instance will look up FPJS
  mounted: function () {
    FingerprintJS.load({
      token: BROWSERAPIKEY,
    })
      .then((fp) => fp.get())
      .then((result) => {
        this.visitorId = result.visitorId;
      });
  },
  methods: {
    // When the button is clicked, this function will
    // run and get visitor history from the server
    callServerAPI: function () {
      fetch(
        `https://api.fpjs.io/visitors/${this.visitorId}?limit=${limitTo}&token=${SERVERAPIKEY}`
      )
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          this.responseSummary = `Received history of ${data.visits.length} visits:`;
          data = JSON.stringify(data.visits, null, 4);
          this.serverData = data;
        });
    },
  },
};
</script>

<!-- CSS libraries -->
<style src="normalize.css/normalize.css"></style>

<!-- Global CSS -->
<style>
code {
  font-family: Menlo, Monaco, Lucida Console, Liberation Mono, DejaVu Sans Mono,
    Bitstream Vera Sans Mono, Courier New, monospace, serif;
  font-size: 0.9em;
  white-space: pre-wrap;
  color: #2c3e50;
}

code::before,
code::after {
  content: "`";
}
</style>

<!-- Scoped component css -->
<!-- It only affect current component -->
<style scoped>
#app {
  text-align: center;
}

#app h1 {
  color: #2c3e50;
  font-weight: 300;
  margin: 0;
}

.banner {
  height: 200px;
  background-color: #f6f6f6;
  padding: 50px 10px;
}

.bottom {
  padding: 80px 10px;
  font-size: 24px;
  font-weight: 300;
}

.fade {
  font-size: 14px;
}

.logo {
  animation: spin 4s 1s infinite linear;
}

.response {
  font-family: inherit;
  text-align: left;
  margin: 5%;
  font-size: 14px;
}

@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>

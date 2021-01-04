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
    <h3>{{responseSummary}}</h3>
    <pre>{{ serverData }}</pre>
  </div>
</template>

<script>
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
    this.visitorId = "123456789";
  },
  methods: {
    // When the button is clicked, this function will 
    // run and get visitor history from the server
    callServerAPI: function () {
      this.responseSummary = "Received history of last visits:";
      this.serverData = "hello there";
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

@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>

<template>
  <div id="app">
    <h2>DalmaChat!</h2>
    <div v-if="!loggedIn">
      <add-text-form
        textRequest="Login"
        @text-added="login"
        :showLabel="true"
      />
    </div>
    <div v-else>
      <div style="display:block">
        <span>Hello, {{ username }} </span>
        <button v-on:click="logout()">Logout</button>
      </div>
      <message-list :messages="messages" />
      <add-message :username="username" :connection="connection" />
    </div>
  </div>
</template>

<script>
import AddTextForm from "./components/AddTextForm.vue";
import MessageList from "./components/MessageList.vue";
import AddMessage from "./components/AddMessage.vue";
export default {
  name: "App",
  components: { MessageList, AddTextForm, AddMessage },
  data: function() {
    return {
      messages: [],
      loggedIn: false,
      connection: null,
      username: "",
    };
  },
  methods: {
    logout() {
      if (this.connection != null) {
        this.connection.close();
        this.loggedIn = false;
        this.username = "";
      }
    },
    login(username) {
      this.username = username;
      this.connection = new WebSocket(
        "ws://localhost:2000/chat?id=" + username
      );

      this.connection.onmessage = (event) => {
        console.log("Message received");
        const message = JSON.parse(event.data);
        this.messages.push(message);
      };
      this.connection.onerror = function(event) {
        console.log("Error");
        console.log(event);
      };

      this.connection.onopen = (event) => {
        console.log(event);
        console.log("Successfully connected to the echo websocket server...");
        this.loggedIn = true;
      };
      this.connection.onclose = (event) => {
        console.log(event);
        console.log("Connection closed");
        this.loggedIn = false;
        this.username = "";
      };
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

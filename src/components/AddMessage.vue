<template>
  <div class="add-text-bar">
    <add-text-form textRequest="Messaggio" @text-added="sendMessage" />
  </div>
</template>

<script>
import AddTextForm from "./AddTextForm.vue";
export default {
  components: { AddTextForm },
  props: { connection: {}, username: {} },
  methods: {
    sendMessage(text) {
      console.log(this);
      const sendMessage = { user: this.username, message: text };
      this.connection.send(JSON.stringify(sendMessage));
    },
  },
};
</script>
<style scoped>
.add-text-bar {
  height: 10%;
  width: 100%;
  position: fixed;
  z-index: 1;
  bottom: 0;
  left: 0;
  background-color: blue;
  display: block;
  justify-content: space-between;
}
</style>

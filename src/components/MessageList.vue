<template>
  <div class="list-messages" ref="messages">
    <message-card v-for="mex in messages" :key="mex.id">
      <template v-slot:header>{{ mex.user }}</template> {{ mex.message }}
    </message-card>
  </div>
</template>

<script>
import MessageCard from "./MessageCard.vue";
export default {
  props: ["messages"],
  components: { MessageCard },
};
</script>

<style scoped>
.list-messages {
  margin-bottom: 10%;
  padding-bottom: 10%;
}
</style>

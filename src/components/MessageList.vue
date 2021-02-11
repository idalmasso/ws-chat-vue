<template>
  <div class="list-messages">
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

<style></style>

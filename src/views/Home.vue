<template>
  <div>
    <h1>Live Code Share</h1>
    <button v-on:click="start">
      Share Now
    </button>
  </div>
</template>

<script>
import { nanoid } from 'nanoid';

export default {
  name: 'Home',
  methods: {
    start() {
      this.$router.push({ name: 'share', params: { shareId:  nanoid(10) } });
    }
  }
}
</script>
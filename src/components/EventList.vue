<template>
  <ul v-if="events.length > 0" class="event-list">
    <li v-for="event in events" :key="event.title">{{ event.title }}</li>
  </ul>
</template>

<script>
export default {
  props: {
    events: {
      type: Array,
      default: () => []
    }
  }
};
</script>

<style scoped>
.event-list {
  margin-top: 20px;
  padding-left: 0;
  list-style: none;
}

.event-list li {
  font-size: 12px;
  color: var(--grey-800);
  background-color: #ffe9ec;
  font-weight: 30;

}
</style>

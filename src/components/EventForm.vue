<template>
  <form @submit.prevent="submitEvent">
    <input v-model="eventTitle" type="text" placeholder="Event Title" required />
    <input v-model="eventDate" type="date" required />
    <button type="submit">Add Event</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      eventTitle: '',
      eventDate: ''
    };
  },
  methods: {
    submitEvent() {
      if (this.eventTitle && this.eventDate) {
        this.$emit('addEvent', { title: this.eventTitle, date: this.eventDate });
        this.eventTitle = '';
        this.eventDate = '';
      }
    }
  }
};
</script>

<style scoped>
form {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

input {
  padding: 5px;
  font-size: 14px;
}

button {
  padding: 5px 10px;
  background-color: #ffe9ec;
  color: var(--grey-800);
  border: none;
  cursor: pointer;
}
</style>

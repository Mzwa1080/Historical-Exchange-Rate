<template>
  <div>
    <input type="date" @change="handleDateChange" :max="maxDate" />
  </div>
</template>

<script>
import dayjs from 'dayjs';

export default {
  data() {
    return {
      maxDate: dayjs().format('YYYY-MM-DD') 
    };
  },
  methods: {
    handleDateChange(event) {
      const selectedDate = event.target.value;
      this.$emit('date-changed', selectedDate);
    }
  }
};
</script>

<style scoped>
/* Add your styles here */
</style>

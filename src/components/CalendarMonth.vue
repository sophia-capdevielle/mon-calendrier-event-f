<template>
  <div class="calendar-month">
    <div class="calendar-month-header">
      <CalendarDateIndicator
        :selected-date="selectedDate"
        class="calendar-month-header-selected-month"
      />
      <CalendarDateSelector
        :current-date="today"
        :selected-date="selectedDate"
        @dateSelected="updateSelectedDate"
      />
    </div>
    <CalendarWeekdays />

    <ol class="days-grid">
      <CalendarMonthDayItem
        v-for="day in days"
        :key="day.date"
        :day="day"
        :events="getEventsForDay(day.date)"
        :isCurrentMonth="day.isCurrentMonth"
        :isToday="day.isToday"
      />
    </ol>
  </div>

  <EventForm @addEvent="addEvent" />
  
</template>

<script>

import dayjs from "dayjs";
import weekday from "dayjs/plugin/weekday";
import weekOfYear from "dayjs/plugin/weekOfYear";
import CalendarMonthDayItem from "./CalendarMonthDayItem";
import CalendarDateIndicator from "./CalendarDateIndicator";
import CalendarDateSelector from "./CalendarDateSelector";
import CalendarWeekdays from "./CalendarWeekdays";
import EventForm from './EventForm.vue';

dayjs.extend(weekday);
dayjs.extend(weekOfYear);

export default {
  components: {
    CalendarDateIndicator,
    CalendarDateSelector,
    CalendarMonthDayItem,
    CalendarWeekdays,
    EventForm,
  },
  data() {
    return {
      today: dayjs().format('YYYY-MM-DD'),
      selectedDate: dayjs(),
      events: [],
    };
  },
  computed: {
    days() {
      let startOfMonth = dayjs(this.selectedDate).startOf('month');
      let endOfMonth = dayjs(this.selectedDate).endOf('month');
      let startOfGrid = startOfMonth.startOf('week');  // Adjust to start at the beginning of the week
      let endOfGrid = endOfMonth.endOf('week');        // Adjust to end at the end of the week
      let days = [];

      for (let day = startOfGrid; day.isBefore(endOfGrid) || day.isSame(endOfGrid); day = day.add(1, 'day')) {
        days.push({
          date: day.format('YYYY-MM-DD'),
          isToday: day.isSame(dayjs(), 'day'),
          isCurrentMonth: day.isSame(startOfMonth, 'month'),
        });
      }
      return days;
    },
  },
  methods: {
    updateSelectedDate(newDate) {
      this.selectedDate = newDate;
    },
    addEvent(event) {
      this.events.push(event);
    },
    getEventsForDay(date) {
      return this.events.filter(event => event.date === date);
    },
  },
};
</script>

<style scoped>
.calendar-month {
  position: relative;
  background-color: var(--grey-200);
  border: solid 1px var(--grey-300);
}

.day-of-week {
  color: var(--grey-800);
  font-size: 18px;
  background-color: #fff;
  padding-bottom: 5px;
  padding-top: 10px;
}

.day-of-week,
.days-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}

.day-of-week > * {
  text-align: right;
  padding-right: 5px;
}

.days-grid {
  height: 100%;
  position: relative;
  grid-column-gap: var(--grid-gap);
  grid-row-gap: var(--grid-gap);
  border-top: solid 1px var(--grey-200);
}
</style>

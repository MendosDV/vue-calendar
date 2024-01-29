<template>
  <div class="calendar mx-auto">
    <section
      :style="colors"
      class="text-center blue-text fw-normal d-flex justify-content-between align-items-center mb-1"
    >
      <font-awesome-icon @click="prev()" :icon="['fas', 'chevron-left']" class="icon" />
      <div class="d-flex gap-2 mt-2">
        <h5>{{ capitalizeFirstLetter(currentMonthName) }}</h5>
        <h5>{{ currentYear }}</h5>
      </div>
      <font-awesome-icon @click="next()" :icon="['fas', 'chevron-right']" class="icon" />
    </section>
    <section
      :style="colors"
      class="d-flex blue-background text-light"
    >
      <p
        class="p-2 my-auto text-center"
        style="width: 14.28%"
        v-for="day in days"
        :key="day"
      >
          {{ day }}
      </p>
    </section>
    <section :style="colors" class="d-flex flex-wrap bg-white" >
      <p
        class="p-2 my-auto text-center"
        style="width: 14.28%"
        v-for="num in startDay()"
        :key="num"
      >
      </p>
      <p
        :class="activeDate(num)"
        class="p-2 my-auto text-center"
        style="width: 14.28%"
        v-for="num in daysInMonth()"
        :key="num"
      >
          {{ num }}
      </p>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentDate: new Date(),
      currentDay: new Date().getDate(),
      currentMonth: new Date().getMonth(),
      currentYear: new Date().getFullYear(),
      blue: '#0F89FF',
      grey: '#D5D5D5',
      days: [ 'Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Frid', 'Sat']
    }
  },
  computed: {
    colors() {
      return {
        '--blue' : this.blue,
        '--grey' : this.grey,
      }
    },
    currentMonthName() {
      return new Date(
        this.currentYear,
        this.currentMonth
      ).toLocaleString('default', { month : 'long'})
    }
  },
  methods: {
    currentDateClass(num) {
      return new Date(this.currentYear, this.currentMonth, num) === new Date()
    },
    activeDate(num) {
      const trueMonth = new Date().getMonth() === this.currentMonth
      const trueYear = new Date().getFullYear() === this.currentYear
      
      if(num < this.currentDay && trueMonth && trueYear) {
        return 'grey-text'
      }
      else if (num === this.currentDay && trueMonth && trueYear ) {
        return 'active-date'
      }
      else { return '' }
    },
    capitalizeFirstLetter(string) {
      return string.charAt(0).toUpperCase() + string.slice(1);
    },
    daysInMonth () {
      return new Date(this.currentYear, this.currentMonth + 1, 0).getDate() // 0 Pour obtenir le dernier jour du mois
    },
    startDay() {
      return new Date(this.currentYear, this.currentMonth, 1).getDay()
    },
    next() {
      if(this.currentMonth === 11){
        this.currentMonth = 1
        this.currentYear++
      }
      else {
        this.currentMonth++
      }
    },
    prev() {
      if(this.currentMonth === 0){
        this.currentMonth = 11
        this.currentYear--
      }
      else {
        this.currentMonth--
      }
    }
  }
}
</script>

<style>
  .calendar {
    max-width: 300px;
  }
  .blue-text {
    color: var(--blue);
  }
  .active-date {
    color: var(--blue);
    font-weight: bold;
  }
  .grey-text {
    color: var(--grey);
  }
  .blue-background {
    background-color: var(--blue);
  }
  .icon {
    color: #7d8697;
    transition: color 0.3s ease;
  }
  .icon:hover {
    color: var(--blue);
    cursor: pointer;
  }
</style>

<template>
  <q-page class="flex flex-center">
    <div class="w-100">
      <template v-for="day in days">
        <day :day="day" :key="day" @append-duration="appendDuration" />
      </template>
    </div>
    <div>
      <h5>Horas totales: {{totalHours}}</h5>
    </div>
  </q-page>
</template>

<script>
import Day from "components/Day";
import moment from 'moment'
export default {
  name: 'PageIndex',
  components: {Day},
  data: () => ({
    day1StartTime: '',
    days: ['Lunes', 'Martes', 'Miercoles', 'Jueves', 'Viernes'],
    countHoursDay: {},
    totalHours: '00:00'
  }),
  methods: {
    appendDuration(day, e){
      this.countHoursDay[day] = e;
      const calculateHours = (prev, curr) => {
        // prev {h, m}
        let hours = prev.hours + curr.hours;
        let minutes = prev.minutes + curr.minutes;
        return {
          hours, minutes
        }
      }
      let res = Object.values(this.countHoursDay).reduce(calculateHours);
      console.log(res)
      let appendHours =  Math.trunc(res.minutes / 60);
      let restMinutes = res.minutes % 60;
      res.hours += appendHours;
      res.minutes = restMinutes;
      console.log(res)
      this.totalHours = `${res.hours} horas y ${res.minutes} minutos`
    }
  }
}
</script>

<style lang="sass">
.w-100
  width: 100%
</style>

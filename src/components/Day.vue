<template>
  <div class="row day">
    <h5>{{ day }}</h5>
    <div class="input-days">
      <q-input filled v-model="startTime" mask="time" :rules="['time']">
        <template v-slot:append>
          <q-icon name="access_time" class="cursor-pointer">
            <q-popup-proxy transition-show="scale" transition-hide="scale">
              <q-time v-model="startTime">
                <div class="row items-center justify-end">
                  <q-btn v-close-popup label="Close" color="primary" flat/>
                </div>
              </q-time>
            </q-popup-proxy>
          </q-icon>
        </template>
      </q-input>
      <q-input filled v-model="endTime" mask="time" :rules="['time']">
        <template v-slot:append>
          <q-icon name="access_time" class="cursor-pointer">
            <q-popup-proxy transition-show="scale" transition-hide="scale">
              <q-time v-model="endTime">
                <div class="row items-center justify-end">
                  <q-btn v-close-popup label="Close" color="primary" flat/>
                </div>
              </q-time>
            </q-popup-proxy>
          </q-icon>
        </template>
      </q-input>
    </div>
    <p>Horas: {{sum}}</p>
  </div>
</template>

<script>
import moment from 'moment'
export default {
  name: "Day",
  props: ['day'],
  data: () => ({
    startTime: '00:00',
    endTime: '00:00',
    sum: '00:00'
  }),
  watch: {
    startTime: function () {
      this.onSelectHour();
    },
    endTime: function () {
      this.onSelectHour();
    },
  },
  methods: {
    onSelectHour(){
      let time1 = moment(this.startTime, "HH:mm");
      let time2 = moment(this.endTime, "HH:mm");
      let diff = time2.diff(time1);
      let duration = moment.duration(diff);
      let hours = duration.get('hour');
      let minutes = duration.get('minute');
      this.sum = `${hours < 0 ?  '-': ''} ${moment.utc(duration.asMilliseconds()).format('HH:mm')}`
      if(hours >= 0) {
        this.$emit('append-duration', this.day, {hours, minutes});
      }
    }
  }
}
</script>

<style scoped lang="sass">
.day
  justify-content: space-around
  border-bottom: 1px solid red
  margin-top: 20px
</style>

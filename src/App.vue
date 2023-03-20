<script>

const timeToMinutes = (time) => {
  const times = time.split(":");
  return times[0] * 60 + times[1] * 1;
}

const minutesToTime = (minutes) => `00${Math.floor(minutes / 60)}`.slice(-2) + ":" + `00${minutes % 60}`.slice(-2);



export default {
  data() {
    return {
      wasSet: false,
      arriveTime: '06:00',
      departmentTime: '18:00',
      remainingTime: '10:00',
      remainingRealTime: '00:00',
      inputTime: '00:00',
      savedTime: 0,
      timeToWrite: "00:00",
    }
  },
  methods: {
    startTimer() {
      if (this.remainingTime === 0) {
        return;
      }
      this.wasSet = true;
      this.remainingRealTime = this.remainingTime;
      this.remainingTime = timeToMinutes(this.remainingTime);
    },
    CalculateTimeBefore() {
      if (this.inputTime === 0)
        return;
      const arriveMinutes = timeToMinutes(this.arriveTime);
      const inputMinutes = timeToMinutes(this.inputTime);
      let diff = inputMinutes - arriveMinutes - Math.floor(Math.random() * 5);
      if (this.remainingTime < diff)
        diff = parseInt(this.remainingTime);
      this.remainingTime -= diff;
      this.remainingRealTime = minutesToTime(this.remainingTime);
      this.savedTime = diff;
      this.timeToWrite = minutesToTime(inputMinutes - diff);
      this.inputTime = '00:00'

    },
    CalculateTimeAfter() {
      if (this.inputTime === 0)
        return;
      const departmentMinutes = timeToMinutes(this.departmentTime);
      const inputMinutes = timeToMinutes(this.inputTime);
      let diff = departmentMinutes - inputMinutes - Math.floor(Math.random() * 10);
      if (this.remainingTime < diff)
        diff = parseInt(this.remainingTime);
      this.remainingTime -= diff;
      this.remainingRealTime = minutesToTime(this.remainingTime);
      this.savedTime = diff;
      this.timeToWrite = minutesToTime(inputMinutes + diff);
      this.inputTime = '00:00'

    },
    Reset() {
      this.wasSet = false;
      this.arriveTime = '06:00';
      this.departmentTime = '18:00';
      this.remainingTime = '10:00';
      this.remainingRealTime = '00:00';
      this.inputTime = '00:00';
      this.savedTime = 0;
      this.timeToWrite = "00:00";
    }
  },
}
</script>

<template>
  <div v-if="wasSet">
    <p>Zbývá nám {{ remainingRealTime }} přesčasu ({{ remainingTime }} minut).</p>
    <div v-if="remainingTime !== 0">
      <p>Zadejte čas</p>
      <input type="time" v-model="inputTime">
      <p>A je to čas?</p>
      <div>
        <button @click="CalculateTimeBefore">Příchodu</button>
        <button @click="CalculateTimeAfter">Odchodu</button>
      </div>
    </div>
    <div v-else>
      <button @click="Reset">
        Začít znova
      </button>
    </div>
      <div v-if="savedTime !== 0">
        <p>Napište si {{ timeToWrite }}.</p>
        <p>Ušetřili jsme tak {{ savedTime }} minut.</p>
      </div>
  </div>
  <div v-else>
    <fieldset>
      <legend>Časy pro vstup a výstup</legend>
      <div>
        <label for="arriveTime">Nejdřívější možný vstup</label>
        <input type="time" name="arriveTime" id="arriveTime" v-model="arriveTime">
      </div>
      <div>
        <label for="departmentTime">Nejpozdější možný odchod</label>
        <input type="time" name="departmentTime" id="departmentTime" v-model="departmentTime">
      </div>
    </fieldset>
    <p>Zadejte kolik máte přesčasu</p>
    <input type="time" v-model="remainingTime">
    <div>
      <button @click="startTimer">Start</button>
    </div>
  </div>
  <p class="footer">by Jakub Indrák 2023</p>
</template>

<style scoped>
fieldset>* {
  margin: 0.5rem;
}

.footer {
  position: absolute;
  bottom: 0;
  right: 0;
  margin: 0.5rem;
}
</style>

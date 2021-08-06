<template>
  <div class="wraper">
    <header class="header">
      <h1>Sorting Training System</h1>
      <button @click="startSorting">Start sorting !</button>
    </header>
    <PeaopleList
      v-if="showPeaopleList"
      :usersCount="usersCount"
      :timer="timer"
      :formattedTime="formattedTime"
      @showSuccessCard="this.showSuccessCard = $event"
      @clearInterval="clearInterval"
    />
    <ModalCard v-if="showModalCard" @howManyUser="handleHowManyUser" />
  </div>
  <ModalSuccess
    :formattedTime="formattedTime"
    v-if="showSuccessCard"
    @endSorting="endSorting"
  />
</template>

<script>
import PeaopleList from './components/PeopleList.vue'
import ModalCard from './components/ModalCard.vue'
import ModalSuccess from './components/ModalSuccess.vue'

export default {
  name: 'App',
  components: { PeaopleList, ModalCard, ModalSuccess },
  data() {
    return {
      usersCount: null,
      showPeaopleList: false,
      showModalCard: false,
      showSuccessCard: false,
      timer: null,
      currentTimer: 0,
      formattedTime: '00:00:00',
      endTimer: '',
    }
  },
  methods: {
    startSorting() {
      this.showModalCard = true
    },
    handleHowManyUser(count) {
      this.usersCount = Number(count)
      this.showPeaopleList = true

      this.timer = this.startTimer()
    },
    // Hiding enter modal and starting timer
    startTimer() {
      this.showModalCard = false

      this.endTimer = setInterval(() => {
        this.currentTimer++
        this.formattedTime = this.foramtTime(this.currentTimer)
      }, 1000)
    },
    foramtTime(seconds) {
      let time = new Date(null)
      time.setSeconds(seconds)
      let MHSTime = time.toISOString().substr(11, 8)
      return MHSTime
    },
    clearInterval() {
      window.clearInterval(this.endTimer)
      this.currentTimer = 0
      // this.formattedTime = '00:00:00'
    },
    // The end of sorting
    endSorting() {
      this.showPeaopleList = false
      this.showSuccessCard = false
    },
  },
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.wraper {
  display: flex;
  flex-direction: column;
  margin: 30px;
}

.header {
  display: flex;
  justify-content: space-between;
}
</style>

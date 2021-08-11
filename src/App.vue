<template>
  <div class="success-wraper" v-if="showSuccessCard">
    <ModalSuccess
      :formattedTime="formattedTime"
      v-if="showSuccessCard"
      @endSorting="endSorting"
    />
  </div>
  <div class="wraper">
    <header class="header d_flex_sp_between">
      <h1>Sorting Training System</h1>
      <button @click="startSorting" v-if="showSortingBtn">
        Start sorting !
      </button>
    </header>

    <PeaopleList
      v-if="showPeaopleList"
      :usersCount="usersCount"
      :timer="timer"
      :formattedTime="formattedTime"
      @showSuccessCard="this.showSuccessCard = $event"
      @clearInterval="clearInterval"
      @cancelPeaopleList="cancelPeaopleList"
    />
    <ModalCard
      v-if="showModalCard"
      @howManyUser="handleHowManyUser"
      @cancel="this.showModalCard = $event"
    />
  </div>
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
      showSortingBtn: true,
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
      this.showSortingBtn = false

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
    },
    // Cancel sorting
    cancelPeaopleList() {
      window.clearInterval(this.endTimer)
      this.currentTimer = 0
      this.formattedTime = '00:00:00'
      this.showPeaopleList = false
      this.showSuccessCard = false
      this.showSortingBtn = true
    },
    // The end of sorting
    endSorting() {
      this.showPeaopleList = false
      this.showSuccessCard = false
      this.showSortingBtn = true
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

/* Reusable classes */

.d_flex_sp_between {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

body {
  background-color: #f5f5f5;
  color: #000000;
  font-weight: 700;
}

#app {
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 1138px;
  padding: 64px 72px;
}

.success-wraper {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: #0000004f;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 3;
}

.wraper {
  display: flex;
  flex-direction: column;
  margin: 30px;
}

.header button {
  background: #ff8d00;
  color: #ffffff;
  padding: 15px 36px;
  border: none;
  border-radius: 5px;
  font-size: 15px;
  cursor: pointer;
}

.header button:hover {
  background-color: #ffae00;
}

.header button:active {
  background-color: #e57f00;
}

/* Table desing */

.header h1 {
  font-size: 32px;
}
</style>

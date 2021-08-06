<template>
  <form @submit.prevent="start" class="modal_card">
    <p>Modal Card</p>
    <p>Enter a number of how many people you want to add to the list.</p>
    <p>Enter number between 20 and 100</p>
    <input
      type="number"
      v-model="usersNum"
      :style="{ border: `2px solid ${inputValid}` }"
    />
    <div>
      <div @click="cancel" type="button" value="cancel">Cancel</div>
      <button>Start</button>
    </div>
  </form>
</template>

<script>
export default {
  data() {
    return {
      usersNum: null,
      inputValid: ['#CCCCCC'],
    }
  },
  methods: {
    start() {
      if (this.usersNum >= 20 && this.usersNum <= 100) {
        this.$emit('howManyUser', this.usersNum)
        this.usersNum = null
      }
    },
    cancel() {
      this.$emit('cancel', false)
    },
  },
  watch: {
    usersNum: function () {
      if (this.usersNum >= 20 && this.usersNum <= 100) {
        this.inputValid = '#1DB44E'
      } else if (this.usersNum == 0) {
        this.inputValid = '#CCCCCC'
      } else {
        this.inputValid = '#B41D1D'
      }
    },
  },
}
</script>

<style>
input:focus {
  outline: none;
}
</style>

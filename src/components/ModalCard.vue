<template>
  <div class="modal_card">
    <p>Modal Card</p>
    <p>Enter a number of how many people you want to add to the list.</p>
    <input type="number" v-model="usersNum" />
    <div>
      <button>Cancel</button>
      <button @click="start">Start</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      usersNum: null,
    }
  },
  methods: {
    start() {
      this.$emit('howManyUser', this.usersNum)
      this.usersNum = null
    },
  },
}
</script>

<style></style>

<template>
  <div class="modal-success">
    <h2>Succes</h2>
    <div>Your result is {{ formattedTime }}</div>
    <div><button @click="endSorting">Close</button></div>
  </div>
</template>

<script>
export default {
  props: ['formattedTime'],
  methods: {
    endSorting() {
      this.$emit('endSorting', false)
    },
  },
}
</script>

<style>
.modal-success {
  margin: 30px;
}
</style>

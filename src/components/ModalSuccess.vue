<template>
  <div class="modal-success">
    <h2>Succes</h2>
    <div class="results">
      <p>Your result is {{ formattedTime }}</p>
    </div>
    <div class="close_btn"><button @click="endSorting">Close</button></div>
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
  margin: 30px 0;
  background-color: #ffffff;
  width: 437px;
  border-radius: 5px;
  box-shadow: 1px 4px 4px rgba(0, 0, 0, 0.2);
}

.modal-success h2 {
  font-size: 18px;
  padding: 20px;
}

.modal-success .results {
  font-size: 13px;
  font-weight: 400;
  color: #555555;
  padding: 20px 0;
  border-top: 1px solid #cccccc;
  border-bottom: 1px solid #cccccc;
}

.modal-success .results p {
  padding: 0 20px;
}

.close_btn {
  display: flex;
  justify-content: flex-end;
  padding: 20px;
}

.close_btn button {
  padding: 12px 23px;
  background-color: #eeeeee;
  color: #555555;
  border-radius: 5px;
  font-size: 14px;
  border: none;
  cursor: pointer;
  font-weight: 700;
}
</style>

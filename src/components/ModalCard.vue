<template>
  <form @submit.prevent="start">
    <div class="form_header d_flex_sp_between">
      <p>How many peaople?</p>
      <i @click="cancel" class="fas fa-times"></i>
    </div>
    <div class="input_div">
      <p>Enter a number of how many people you want to add to the list.</p>
      <input
        type="number"
        v-model="usersNum"
        :style="{ border: `2px solid ${inputValid}` }"
        placeholder="Enter number between 20 and 100"
      />
    </div>
    <div class="form_btns">
      <div @click="cancel" type="button" value="cancel">
        Cancel
      </div>
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
      if (this.usersNum >= 4 && this.usersNum <= 100) {
        this.$emit('howManyUser', this.usersNum)
        this.usersNum = null
      } else {
        this.inputValid = '#B41D1D'
      }
    },
    cancel() {
      this.$emit('cancel', false)
    },
  },

  watch: {
    usersNum: function () {
      if (this.usersNum >= 4 && this.usersNum <= 100) {
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

<style scoped>
form {
  background-color: #ffffff;
  width: 437px;
  border-radius: 5px;
  box-shadow: 1px 4px 4px rgba(0, 0, 0, 0.2);
  margin: 30px 0;
}

.form_header {
  padding: 20px;
}

.form_header p {
  font-size: 18px;
}

.form_header i {
  color: #999999;
  cursor: pointer;
}

.input_div {
  height: 140px;
  padding: 19px 20px;
  border-top: 1px solid #cccccc;
  border-bottom: 1px solid #cccccc;
}

.input_div p {
  font-size: 13px;
  font-weight: 400;
  color: #555555;
  padding-bottom: 14px;
}

.input_div input {
  width: 400px;
  height: 40px;
  font-size: 13px;
  border-radius: 5px;
  padding: 13px 13px 12px 14px;
}

.input_div input:focus {
  outline: none;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
}

.form_btns {
  display: flex;
  justify-content: flex-end;
  padding: 20px;
}

.form_btns div {
  padding: 12px 22px;
  background-color: #eeeeee;
  color: #555555;
  border-radius: 5px;
  margin-right: 10px;
  font-size: 14px;
  cursor: pointer;
}

.form_btns button {
  padding: 12px 23px;
  background-color: #ff8d00;
  color: #ffffff;
  border-radius: 5px;
  font-size: 14px;
  border: none;
  cursor: pointer;
}
</style>

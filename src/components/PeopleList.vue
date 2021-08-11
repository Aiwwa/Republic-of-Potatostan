<template>
  <div class="users_wraper">
    <div class="wraper_header">
      <div class="timer_info">
        <div v-if="cancelBtn">Timer started: {{ formattedTime }}</div>
        <button @click="cancelPeaopleList" v-if="cancelBtn">
          Cancel
        </button>
      </div>
      <div>{{ this.users.length }} people in the list</div>
    </div>
    <table class="customers">
      <tr>
        <th class="th_head_email">Email</th>
        <th class="th_head_patatoes">Potatoes</th>
        <th>Tags</th>
        <th>Full name</th>
        <th>Location</th>
      </tr>
      <draggable
        v-model="users"
        tag="transition-group"
        item-key="id"
        ghost-class="ghost"
        @end="onEnd"
        v-bind="dragOptions"
      >
        <template #item="{element}">
          <tr class="sortable">
            <td class="td_name" :class="{ checkboxBg: element.checked }">
              <label class="container">
                <input @click="checked(element)" type="checkbox" />
                <span class="checkmark"></span>
              </label>
              <div>{{ element.email }}</div>
              <div><i class="fas fa-chevron-right"></i></div>
            </td>
            <td
              class="td_patatoes"
              :class="{ checkboxBg: element.checked }"
              :style="{ cursor: 'move' }"
            >
              {{ element.patatoes }}
            </td>
            <td class="td_roles" :class="{ checkboxBg: element.checked }">
              <span class="pil">{{ element.customerInfo['role'] }}</span>
            </td>
            <td :class="{ checkboxBg: element.checked }">
              {{ element.name['first'] }} {{ element.name['last'] }}
            </td>
            <td :class="{ checkboxBg: element.checked }">
              {{ element.location['country'] }}
            </td>
          </tr>
        </template>
      </draggable>
    </table>
  </div>
</template>

<script>
import draggable from 'vuedraggable'
export default {
  props: ['usersCount', 'timer', 'showSuccessCard', 'formattedTime'],
  components: { draggable },
  data() {
    return {
      users: [],
      sortedArr: [],
      isSorted: '',
      cancelBtn: true,
    }
  },
  methods: {
    checked(user) {
      user.checked = !user.checked
    },
    onEnd() {
      this.sortedArr = []
      this.users.forEach((el) => this.sortedArr.push(el.patatoes))

      let sortingArr = this.sortedArr

      // Returns true when array is sorted
      this.isSorted = !!sortingArr.reduce(
        (n, num) => n !== false && num <= n && num,
      )

      if (this.isSorted) {
        this.cancelBtn = false
        this.$emit('clearInterval')
        this.$emit('showSuccessCard', true)
      }
    },
    cancelPeaopleList() {
      this.$emit('cancelPeaopleList')
    },
  },
  mounted() {
    fetch(`https://randomuser.me/api/?results=${this.usersCount}`)
      .then((data) => data.json())
      .then((res) => {
        this.users = res.results

        // Random number without duplicates
        let nums = []
        for (let i = 1; i < this.usersCount + 1; i++) {
          nums.push(i)
        }
        let ranNums = []
        let i = nums.length
        let j = 0

        while (i--) {
          j = Math.floor(Math.random() * (i + 1))
          ranNums.push(nums[j])
          nums.splice(j, 1)
        }

        // Add values to data from API
        let modifiedUsers = this.users.map((el, index) => {
          return {
            ...el,
            patatoes: ranNums[index],
            customerInfo: {
              role: 'Customer',
            },
            checked: false,
          }
        })

        this.users = modifiedUsers
      })
      .catch((err) => console.log(err.message))
  },
  computed: {
    dragOptions() {
      return {
        animation: 200,
        group: 'description',
        disabled: false,
        ghostClass: 'ghost',
      }
    },
  },
}
</script>

<style>
.checkboxBg {
  background-color: #f5f5f5;
}

.users_wraper {
  border: 1px solid #dddddd;
  background-color: #ffffff;
  width: 1138px;
  height: 1180px;
  overflow: auto;
  border-radius: 5px;
  box-shadow: 1px 4px 4px rgba(0, 0, 0, 0.2);
}

.wraper_header {
  display: flex;
  justify-content: space-between;
  padding: 25px 30px;
  font-size: 14px;
  border-bottom: 1px solid #dddddd;
  background-color: #ffffff;
  position: sticky;
  top: 0;
  z-index: 2;
}

.wraper_header .timer_info {
  display: flex;
}

.timer_info {
  font-weight: 400;
  color: #555555;
  display: flex;
  justify-content: center;
  align-items: center;
}

.timer_info button {
  margin: 0 5px;
  padding: 5px 25px;
  border-radius: 5px;
  background-color: #ff8d00;
  border: none;
  color: #ffffff;
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

.timer_info button:hover {
  background-color: #ffae00;
}

.timer_info button:active {
  background-color: #e57f00;
}

/* Table design */
.customers {
  font-weight: 400;
  color: #555555;
  font-size: 14px;
  width: 100%;
  border-spacing: 0;
}

/*  Draggable desing */

.sortable-drag {
  background-color: #ff8d00;
}

.ghost {
  opacity: 0.5;
  background: #eeeeee;
}

/* Table header */
.customers tr th {
  text-align: left;
  font-weight: 400;
  color: #555555;
  border-bottom: 1px solid #dddddd;
  background-color: #ffffff;
  z-index: 1;
  position: sticky;
  top: 70px;
}

.customers tr .th_head_email {
  width: 350px;
  border-right: 1px solid #dddddd;
  padding: 15px 20px;
}

.customers tr .th_head_patatoes {
  padding-left: 40px;
}

/* Table body */
.customers tr td {
  color: #000000;
  border-bottom: 1px solid #dddddd;
  height: 48px;
}

/* Table body names */
.customers tr .td_name {
  display: flex;
  justify-content: center;
  border-right: 1px solid #dddddd;
  padding: 14px 12px 14px 20px;
}

.customers tr .td_name div:nth-child(2) {
  display: flex;
  align-items: flex-end;
  margin-left: 40px;
  cursor: pointer;
}

.customers tr .td_name div:nth-child(3) {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  width: 100%;
  color: #999999;
  font-size: 10px;
  cursor: pointer;
}

/* Table body patoes */
.customers .td_patatoes {
  width: 236px;
  padding-left: 44px;
}

/* Table body role */
.customers .td_roles {
  width: 236px;
  color: #555555;
}

.customers .td_roles .pil {
  background-color: #eeeeee;
  padding: 6px 10px;
  border-radius: 18px;
  margin-right: 10px;
}

/* Custom checkbox */
.container {
  display: block;
  position: relative;
  margin-bottom: 12px;
  cursor: pointer;
  font-size: 22px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

/* Hide the browser's default checkbox */
.container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

/* Create a custom checkbox */
.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 20px;
  width: 20px;
  border: 1px solid #dddddd;
  border-radius: 5px;
}

/* When the checkbox is checked, add a blue background */
.container input:checked ~ .checkmark {
  background-color: #ff8d00;
  border-radius: 5px;
  border: none;
}

/* Create the checkmark/indicator (hidden when not checked) */
.checkmark:after {
  content: '';
  position: absolute;
  display: none;
}

/* Show the checkmark when checked */
.container input:checked ~ .checkmark:after {
  display: block;
}

/* Style the checkmark/indicator */
.container .checkmark:after {
  left: 7px;
  top: 2px;
  width: 5px;
  height: 10px;
  border: solid #ffffff;
  border-width: 0 3px 3px 0;
  -webkit-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  transform: rotate(45deg);
}
</style>

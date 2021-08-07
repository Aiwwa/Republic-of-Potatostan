<template>
  <div class="users__wraper">
    <div class="users__wraper__header">
      <div>
        <div>Timer started: {{ formattedTime }}</div>
        <button @click="cancelPeaopleList">cancel</button>
      </div>
      <div>{{ this.users.length }} peoples in the list</div>
    </div>
    <table id="customers">
      <tr>
        <th>Email</th>
        <th>Patatoes</th>
        <th>Tags</th>
        <th>Full name</th>
        <th>Location</th>
      </tr>
      <draggable
        v-model="users"
        tag="transition-group"
        item-key="id"
        @end="onEnd"
      >
        <template #item="{element}">
          <tr>
            <td class="ch_input">
              <div><input type="checkbox" /></div>
              {{ element.email }}
            </td>
            <td>{{ element.patatoes }}</td>
            <td>
              <span>{{ element.customerInfo['role'] }}</span>
              <span>{{ element.customerInfo['status'][2] }}</span>
            </td>
            <td>{{ element.name['first'] }} {{ element.name['last'] }}</td>
            <td>{{ element.location['country'] }}</td>
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
    }
  },
  methods: {
    onEnd() {
      this.sortedArr = []
      this.users.forEach((el) => this.sortedArr.unshift(el.patatoes))

      let sortingArr = this.sortedArr

      // Returns true when array is sorted
      this.isSorted = !!sortingArr.reduce(
        (n, num) => n !== false && num >= n && num,
      )

      if (this.isSorted) {
        this.$emit('clearInterval')
        this.$emit('showSuccessCard', true)
      }

      console.log(this.isSorted)
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
              role: 'customer',
              status: ['', 'VIP', 'Oldtimer', 'New'],
            },
          }
        })

        this.users = modifiedUsers
      })
      .catch((err) => console.log(err.message))
  },
}
</script>

<style>
.users__wraper__header {
  display: flex;
  justify-content: space-between;
}
th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #b3b3b3;
  color: white;
  width: 300px;
  padding: 10px;
}

td {
  padding: 10px;
}

.ch_input {
  display: flex;
  align-content: center;
  justify-content: flex-start;
}
.ch_input div {
  margin-right: 10px;
}
</style>

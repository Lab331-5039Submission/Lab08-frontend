<template>
  <div>
    <h1>Create an organizer</h1>
    <form @submit.prevent="saveOrganizer">
      <label>Name</label>
      <input
        v-model="organizer.name"
        type="text"
        placeholder="name"
        class="field"
      />
      <label>Address</label>
      <input
        v-model="organizer.address"
        type="text"
        placeholder="address"
        class="field"
      />
      <button type="submit">Submit</button>
    </form>
    <pre>{{ organizer }}</pre>
  </div>
</template>
<script>
import EventService from '@/services/EventService.js'
export default {
  inject: ['GStore'],
  data() {
    return {
      organizer: {
        name: '',
        address: ''
      }
    }
  },
  methods: {
    saveOrganizer() {
      EventService.saveOrganizer(this.organizer)
        .then((response) => {
          console.log(response)
          this.$router.push({
            name: 'EventList'
          })
          this.GStore.flashMessage =
            'You are successfully add a new organizer for ' +
            response.data.name
          setTimeout(() => {
            // After 3 seconds remove it
            this.GStore.flashMessage = ''
          }, 3000)
        })
        .catch(() => {
          this.$router.push('NetworkError')
        })
    }
  }
}
</script>
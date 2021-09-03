<template>
  <div>
    <h1>Create an event</h1>
    <form @submit.prevent="saveEvent">
      <label>Category</label>
      <input
        v-model="event.category"
        type="text"
        placeholder="Category"
        class="field"
      />
      <h3>Name & describe your event</h3>

      <label>Title</label>
      <input
        v-model="event.title"
        type="text"
        placeholder="Title"
        class="field"
      />

      <label>Description</label>
      <input
        v-model="event.description"
        type="text"
        placeholder="Description"
        class="field"
      />

      <h3>Where is your event?</h3>

      <label>Location</label>
      <input
        v-model="event.location"
        type="text"
        placeholder="Location"
        class="field"
      />
      <button type="submit">Submit</button>
    </form>
    <pre>{{ event }}</pre>
  </div>
</template>
<script>
import EventService from '@/services/EventService.js'
export default {
  inject: ['GStore'],
  data() {
    return {
      event: {
        category: '',
        title: '',
        description: '',
        location: ''
      }
    }
  },
  methods: {
    saveEvent() {
      EventService.saveEvent(this.event)
        .then((response) => {
          console.log(response)
          this.$router.push({
            name: 'EventLayout',
            params: { id: response.data.id }
          })
          this.GStore.flashMessage =
            'You are successfully add a new event for ' + response.data.title
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
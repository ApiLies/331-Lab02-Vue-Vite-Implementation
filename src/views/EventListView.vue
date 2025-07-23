<script setup lang="ts">
import EventCard from '@/components/EventCard.vue'
//import CategoriesCard from '@/components/CategoriesCard.vue'
import type { Event } from '@/types'
import { ref, onMounted } from 'vue'
import EventService from '@/services/EventService'

const events = ref<Event[]>()

onMounted(() => {
  EventService.getEvents()
    .then((response) => {
      //console.log(response.data)
      events.value = response.data
    })
    .catch((error) => {
      console.error('There was an error!', error)
    })
})
</script>

<template>
  <h1>Events for Good</h1>
  <!-- new element -->
  <div class="events">
    <EventCard v-for="event in events" :key="event.id" :event="event" />
    <!--
    <CategoriesCard v-for="event in events" :key="event.id" :event="event" />
    -->
  </div>
</template>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>

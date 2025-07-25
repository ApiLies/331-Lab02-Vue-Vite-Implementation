<script setup lang="ts">
import EventCard from '@/components/EventCard.vue'
import { type Event } from '@/types'
import { ref, onMounted, computed, watchEffect } from 'vue'
import { useRouter } from 'vue-router'
import EventService from '@/services/EventService'

const events = ref<Event[] | null>(null)
const totalEvents = ref(0)

const props = defineProps({
  page: {
    type: Number,
    required: true,
  },
  size: {
    type: Number,
    required: true,
  },
})

const page = computed(() => props.page)
const selectedSize = ref(props.size)

const router = useRouter()

function changeSize() {
  router.push({
    name: 'event-list-view',
    query: {
      page: 1,
      size: selectedSize.value,
    },
  })
}

const hasNextPage = computed(() => {
  const totalPages = Math.ceil(totalEvents.value / selectedSize.value)
  return page.value < totalPages
})

onMounted(() => {
  watchEffect(() => {
    events.value = null
    EventService.getEvents(selectedSize.value, page.value)
      .then((response) => {
        events.value = response.data
        totalEvents.value = parseInt(response.headers['x-total-count'])
      })
      .catch((error) => {
        console.error('There was an error!', error)
      })
  })
})
</script>

<template>
  <h1>Events for Good</h1>

  <!-- new element -->
  <div class="size-selector">
    <label for="size">Events per page:</label>
    <select id="size" v-model.number="selectedSize" @change="changeSize">
      <option :value="2">2</option>
      <option :value="5">5</option>
      <option :value="10">10</option>
    </select>
  </div>

  <div class="events">
    <EventCard v-for="event in events" :key="event.id" :event="event" />

    <div class="pagination">
      <RouterLink
        id="page-prev"
        :to="{ name: 'event-list-view', query: { page: page - 1, size: selectedSize } }"
        rel="prev"
        v-if="page != 1"
        >&#60; Prev Page
      </RouterLink>

      <RouterLink
        id="page-next"
        :to="{ name: 'event-list-view', query: { page: page + 1, size: selectedSize } }"
        rel="next"
        v-if="hasNextPage"
        >Next Page &#62;</RouterLink
      >
    </div>
  </div>
</template>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.pagination {
  display: flex;
  width: 290px;
}

.pagination a {
  flex: 1;
  text-decoration: none;
  color: #2c3e50;
}

#page-prev {
  text-align: left;
}

#page-next {
  text-align: right;
}

.size-selector {
  margin-bottom: 1rem;
}

.size-selector label {
  margin-right: 0.5rem;
}

.size-selector select {
  padding: 4px;
  font-size: 14px;
}
</style>

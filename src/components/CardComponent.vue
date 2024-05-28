<script setup>
import { ref } from 'vue'

const props = defineProps({
  msg: {
    name: String,
    image: String,
    status: String,
    location: String,
    episode: String,
    species: String
  }
})
const { name, image, status, location, episode, species } = props.msg
const todoData = ref(null)
async function fetchData() {
  todoData.value = null
  const res = await fetch(episode[0])
  todoData.value = await res.json()
}
fetchData()
</script>

<template>
  <div class="card">
    <div class="char-img">
      <img :src="image" />
    </div>
    <div class="info">
      <h2 class="green">{{ name || 'null' }}</h2>
      <p>
        <span
          :style="{
            'background-color':
              status == 'unknown'
                ? 'rgb(158, 158, 158)'
                : status == 'Alive'
                  ? 'rgb(85, 204, 68)'
                  : 'rgb(214, 61, 46)'
          }"
          class="status-icon"
        ></span>
        {{ status }} - {{ species }}
      </p>
      <h3>Last known location:</h3>
      <p>{{ location.name }}</p>
      <h3>First seen in:</h3>
      <p>{{ todoData?.name }}</p>
    </div>
  </div>
</template>

<style scoped>
.card {
  color: hsla(0, 0%, 0%, 0.8);
  font-family: -apple-system, 'BlinkMacSystemFont', 'Segoe UI', 'Roboto', 'Helvetica', 'Arial',
    sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol';
  font-weight: normal;
  word-wrap: break-word;
  font-kerning: normal;
  font-feature-settings: 'kern', 'liga', 'clig', 'calt';
  font-variant-ligatures: none;
  text-rendering: optimizelegibility;
  -webkit-font-smoothing: antialiased;
  text-decoration-skip-ink: auto;
  box-sizing: inherit;
  width: 600px;
  height: 220px;
  display: flex;
  overflow: hidden;
  background: rgb(60, 62, 68);
  border-radius: 0.5rem;
  margin: 0.75rem;
  box-shadow:
    rgba(0, 0, 0, 0.1) 0px 4px 6px -1px,
    rgba(0, 0, 0, 0.06) 0px 2px 4px -1px;
}
.status-icon {
  display: inline-block;
  height: 0.5rem;
  width: 0.5rem;
  margin-right: 0.375rem;
  border-radius: 50%;
}

.info {
  -webkit-font-smoothing: antialiased;
  text-decoration-skip-ink: auto;
  box-sizing: inherit;
  flex: 3 1 0%;
  position: relative;
  padding: 0.75rem;
  color: rgb(255, 255, 255);
  display: flex;
  flex-direction: column;
}
.info h2 {
  font-size: 1.5rem;
  font-weight: 700;
}
</style>

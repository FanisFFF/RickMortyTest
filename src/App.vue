<script setup>
import { ref, watch } from 'vue'
import CardComponent from './components/CardComponent.vue'
const currentPage = ref(1)
const data = ref(null)
const name = ref('')
const status = ref('')

function handleApply() {
  currentPage.value = 1
  fetchData()
}

async function fetchData() {
  data.value = null
  const res = await fetch(
    `https://rickandmortyapi.com/api/character/?page=${currentPage.value}&name=${name.value}&status=${status.value}`
  )
  data.value = await res.json()
  console.log(currentPage.value)
}
fetchData()
watch(currentPage, fetchData)
</script>

<template>
  <main>
    <div class="input-container">
      <input v-model="name" placeholder="Search" />
      <div class="radio-button-container">
        <div>
          <input type="radio" id="alive" name="alive" value="alive" v-model="status" />
          <label for="alive">Alive</label>
        </div>

        <div>
          <input type="radio" id="dead" name="dead" value="dead" v-model="status" />
          <label for="dead">Dead</label>
        </div>
        <div>
          <input type="radio" id="none" name="none" value="" v-model="status" />
          <label for="none">None</label>
        </div>
      </div>
      <div>
        <button @click="handleApply()">Apply</button>
      </div>
    </div>
    <div class="wrapper" v-if="data && !data.error">
      <CardComponent v-for="todo in data.results" :key="todo.id" :msg="todo" />
    </div>
    <p v-else>Not found</p>
    <div class="pagination">
      <div class="pagination-buttons">
        <button v-if="data?.info?.prev" @click="currentPage--">prev</button>
        <p>{{ currentPage }}</p>
        <button v-if="data?.info?.next" @click="currentPage++">next</button>
      </div>
    </div>
  </main>
</template>

<style scoped>
main label {
  color: white;
}
p {
  color: white;
}

.wrapper {
  display: grid;
  grid-template-columns: 1fr 1fr;
}

.input-container {
  display: grid;
  gap: 0.2rem;
}
.radio-button-container {
  display: flex;
  gap: 1rem;
}
.pagination {
  display: flex;
  justify-content: center;
}
.pagination-buttons {
  display: flex;
  gap: 1rem;
}

@media (max-width: 1200px) {
  .wrapper {
    grid-template-columns: 1fr;
  }
}
</style>

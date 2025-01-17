<template>
  <section class="section bg-light">
    <div class="container">
      <h2>Search Results</h2>
      <div v-if="loading" class="text-center">
        <p>Loading...</p>
      </div>
      <div v-else>
        <ul v-if="results.length">
          <li v-for="(result) in results" :key="result.id">
            {{ result.name }} — {{ result.price }} USD аналог ({{result.analogs}})
          </li>
      </ul>
        <p v-else>No results found for "{{ query }}".</p>
      </div>
      
    </div>
  </section>
</template>

<script>
export default {
  props: {
    results: {
      type: Array,
      required: true,
    },
    loading: {
      type: Boolean,
      required: true,
    },
    query: {
      type: String,
      required: true,
    },
  },
};
</script>

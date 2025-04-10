<template>
  <div>
    <search @search="onSearch" />
    <results-section :results="results" :loading="loading" :query="query" />
  </div>
</template>

<script>
import Search from "./components/Search";
import ResultsSection from "./components/ResultsSection";

export default {
  name: "components",
  components: {
    Search,
    ResultsSection,
  },
  data() {
    return {
      query: "", // Хранение поискового запроса
      results: [], // Результаты поиска
      loading: false, // Статус загрузки
    };
  },
  methods: {
    async onSearch(query) {
      this.query = query; // Сохраняем текущий запрос
      if (!query.trim()) {
        this.results = []; // Очищаем результаты, если запрос пустой
        return;
      }

      this.loading = true;

      try {
        const response = await fetch(`http://172.20.10.11:3000/api/perfume/search?query=${query}`);
        if (!response.ok) {
          throw new Error("Ошибка запроса");
        }
        const data = await response.json();
        this.results = data || []; // Присваиваем результаты поиска
      } catch (error) {
        console.error("Ошибка:", error);
        this.results = [];
      } finally {
        this.loading = false;
      }
    },
  },
};
</script>

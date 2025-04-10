<template>
  <section class="section pt-0">
    <div class="container">
      <div v-if="loading" class="text-center">
        <p>Loading...</p>
      </div>

      <!-- Секция результатов сразу после поля поиска -->
      <transition name="fade">
        <div
          v-if="query.trim() !== ''"
          class="search-result-box"
        >
          <h3>Результаты поиска</h3>
          <div v-if="results.length" class="mini-cards-container">
            <div
              v-for="(perfume) in results"
              :key="perfume.id"
              class="mini-card"
            >
              <router-link :to="`/product/${perfume.id}`">
                <div class="mini-card-image">
                  <img :src="perfume.images[0] || '/img/default-imag.jpg'" alt="Perfume Image" />
                </div>
                <div class="mini-card-content">
                  <h4 class="mini-title">{{ perfume.name }}</h4>
                  <p class="mini-price">{{ perfume.price_30ml }} USD / 30мл</p>
                </div>
              </router-link>
            </div>
          </div>
          <div v-else class="text-center">
            <p>Парфюмов не найдено</p>
          </div>
        </div>
      </transition>

      <!-- Основной список всех парфюмов -->
      <h2 class="mt-8">Все парфюмы</h2>
      <div class="cards-container">
        <div
          v-for="(perfume) in allPerfumes"
          :key="perfume.id"
          class="card"
        >
          <router-link :to="`/product/${perfume.id}`" class="card-link">
            <div class="card-image">
              <img
                :src="perfume.images[0] || '/img/default-imag.jpg'"
                alt="Perfume Image"
              />
            </div>
            <h3 class="card-title">{{ perfume.name }}</h3>
            <p class="card-price">Цена за 30мл: {{ perfume.price_30ml }} USD</p>
            <p class="card-price">Цена за 50мл: {{ perfume.price_50ml }} USD</p>
            <div class="card-analogs">
              <strong>Аналог:</strong>
              <span v-if="perfume.analog">{{ perfume.analog }}</span>
              <span v-else>Нет аналогов</span>
            </div>
          </router-link>
        </div>
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
  data() {
    return {
      allPerfumes: [],
    };
  },
  watch: {
    results(newResults) {
      if (!newResults.length && this.query.trim() === "") {
        this.fetchAllPerfumes();
      }
    },
  },
  methods: {
    async fetchAllPerfumes() {
      try {
        const response = await fetch("http://172.20.10.11:3000/api/perfume/get-all");
        if (!response.ok) {
          throw new Error("Ошибка при загрузке всех парфюмов");
        }
        const data = await response.json();
        this.allPerfumes = data;
      } catch (error) {
        console.error("Error fetching all perfumes:", error);
      }
    },
  },
  mounted() {
    if (this.query.trim() === "") {
      this.fetchAllPerfumes();
    }
  },
};
</script>

<style scoped>
.container {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 15px;
}

.text-center {
  text-align: center;
}

.cards-container {
  display: grid;
  grid-template-columns: repeat(1, 1fr);
  gap: 1rem;
  margin-top: 1.5rem;
  justify-items: center;
}

/* Планшеты */
@media (min-width: 576px) {
  .cards-container {
    grid-template-columns: repeat(2, 1fr);
    gap: 1.25rem;
  }
}

/* Десктопы */
@media (min-width: 992px) {
  .cards-container {
    grid-template-columns: repeat(3, 1fr);
    gap: 1.5rem;
  }
}

.card {
  background: #ffffff;
  border-radius: 8px;
  padding: 1rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  width: 100%;
  max-width: 100%;
}

@media (min-width: 576px) {
  .card {
    max-width: 300px;
  }
}

.card-link {
  display: block;
  text-decoration: none;
  color: inherit;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.card-title {
  font-size: 1.1rem;
  font-weight: bold;
  margin-top: 0.75rem;
  margin-bottom: 0.5rem;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

@media (min-width: 768px) {
  .card-title {
    font-size: 1.2rem;
  }
}

.card-price {
  color: #666;
  margin-bottom: 0.5rem;
  font-size: 0.9rem;
}

@media (min-width: 768px) {
  .card-price {
    font-size: 1rem;
    margin-bottom: 1rem;
  }
}

.card-analogs {
  font-size: 0.85rem;
  line-height: 1.4;
}

@media (min-width: 768px) {
  .card-analogs {
    font-size: 0.9rem;
  }
}

.card-analogs strong {
  font-weight: bold;
  color: #333;
}

.card-image {
  width: 100%;
  height: 150px;
  overflow: hidden;
  background-color: #f9f9f9;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 8px 8px 0 0;
}

@media (min-width: 576px) {
  .card-image {
    height: 180px;
  }
}

@media (min-width: 992px) {
  .card-image {
    height: 200px;
  }
}

.card-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.search-result-box {
  background-color: #fdfdfd;
  border: 1px solid #ddd;
  border-radius: 6px;
  padding: 1rem;
  margin-top: 1rem;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
}

.mini-cards-container {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
  justify-content: center;
}

@media (min-width: 576px) {
  .mini-cards-container {
    gap: 1rem;
    justify-content: flex-start;
  }
}

.mini-card {
  width: calc(50% - 0.5rem);
  border: 1px solid #ddd;
  border-radius: 6px;
  background-color: white;
  overflow: hidden;
  transition: transform 0.2s ease;
}

@media (min-width: 576px) {
  .mini-card {
    width: 160px;
  }
}

.mini-card:hover {
  transform: translateY(-3px);
}

.mini-card-image {
  width: 100%;
  height: 100px;
  background-color: #f5f5f5;
  display: flex;
  align-items: center;
  justify-content: center;
}

@media (min-width: 576px) {
  .mini-card-image {
    height: 120px;
  }
}

.mini-card-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.mini-card-content {
  padding: 0.5rem;
}

.mini-title {
  font-size: 0.85rem;
  font-weight: bold;
  margin-bottom: 0.3rem;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

@media (min-width: 576px) {
  .mini-title {
    font-size: 0.9rem;
  }
}

.mini-price {
  font-size: 0.75rem;
  color: #777;
}

@media (min-width: 576px) {
  .mini-price {
    font-size: 0.8rem;
  }
}

.mt-8 {
  margin-top: 1.5rem;
}

@media (min-width: 768px) {
  .mt-8 {
    margin-top: 2rem;
  }
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.2s ease;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}

h2 {
  font-size: 1.5rem;
  margin-bottom: 1rem;
  text-align: center;
}

@media (min-width: 768px) {
  h2 {
    font-size: 1.75rem;
    text-align: left;
  }
}

h3 {
  font-size: 1.25rem;
  margin-bottom: 1rem;
}

@media (min-width: 768px) {
  h3 {
    font-size: 1.5rem;
  }
}
</style>
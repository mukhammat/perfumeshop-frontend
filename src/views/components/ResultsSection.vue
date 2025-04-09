<template>
  <section class="section pt-0">
    <div class="container">
      <div v-if="loading" class="text-center">
        <p>Loading...</p>
      </div>
      <div v-else>
        <div v-if="query.trim() !== '' && !results.length" class="text-center">
          <p>Парфюмов не найдено</p>
        </div>
        <div v-else>
          <h2 v-if="query.trim() !== ''">Результаты поиска</h2>
          <h2 v-else>Все парфюмы</h2>
          <div class="cards-container">
            <div
            v-for="(perfume) in query.trim() !== '' ? results : allPerfumes"
            :key="perfume.id"
            class="card"
            >
            <router-link
  :to="`/product/${perfume.id}`"
  class="card"
  tag="div"
>
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
    <span v-if="perfume.analog">
      {{ perfume.analog }}
    </span>
    <span v-else> Нет аналогов</span>
  </div>
</router-link>

            </div>
          </div>
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
.cards-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* 3 карточки в строке */
  gap: 1.5rem;
  margin-top: 1.5rem;
  justify-items: center; /* Центрируем карточки внутри их ячеек */
  max-width: 1200px; /* Ограничиваем максимальную ширину контейнера */
  margin-left: auto; /* Центрируем контейнер */
  margin-right: auto;
}

.card {
  background: #ffffff;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 1rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  width: 100%; /* Карточки занимают 100% доступного пространства */
  max-width: 300px; /* Но не больше 300px */
}

.card {
  cursor: pointer;
  text-decoration: none;
  color: inherit;
}


.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.card-title {
  font-size: 1.2rem;
  font-weight: bold;
  margin-bottom: 0.5rem;
}

.card-price {
  color: #666;
  margin-bottom: 1rem;
}

.card-analogs {
  font-size: 0.9rem;
  line-height: 1.4;
}

.card-analogs strong {
  font-weight: bold;
  color: #333;
}

.card-image {
  width: 100%;
  height: 200px; /* Фиксированная высота для изображения */
  overflow: hidden;
  background-color: #f9f9f9; /* Белый фон по умолчанию */
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 8px 8px 0 0; /* Скругление углов сверху */
}

.card-image img {
  width: 100%;
  height: 100%;
  object-fit: cover; /* Сохранение пропорций изображения */
}

</style>

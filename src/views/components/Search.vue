<template>
    <section class="section section-components pt-0 mt-0">
      <div class="pt-10 py-5 bg-secondary" style="padding-top: 6rem !important;">
        <div class="container">
          <div class="row">
            <div class="col-lg-4 col-sm-6">
              <form @submit.prevent="onSearch">
                <base-input
                  alternative
                  placeholder="Search"
                  addon-left-icon="ni ni-zoom-split-in"
                  v-model="query"
                  @input="onInputChange"
                ></base-input>
                <base-button color="primary" type="submit">Search</base-button>
              </form>
            </div>
          </div>
        </div>
      </div>
    </section>
  </template>
  
  <script>
  export default {
    props: {
      value: String, // Значение поля ввода
    },
    data() {
      return {
        query: this.value, // Локальное состояние для ввода
        debounceTimeout: null, // Таймер для дебаунса
      };
    },
    methods: {
      onInputChange() {
        clearTimeout(this.debounceTimeout);
        this.debounceTimeout = setTimeout(() => {
          this.$emit("update:value", this.query); // Передача значения наверх
          this.$emit("search", this.query); // Отправка запроса после дебаунса
        }, 300); // Дебаунс 300 мс
      },
      onSearch() {
        this.$emit("search", this.query); // Вызов события поиска
      },
    },
    watch: {
      value(newValue) {
        this.query = newValue; // Синхронизация при изменении извне
      },
    },
  };
  </script>
  
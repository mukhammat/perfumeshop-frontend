<template>
    <div class="product-card">
      <div class="product-image-container">
        <img :src="product.image || '/img/default-image.jpg'" alt="product" class="product-image">
        <span v-if="product.discount" class="discount-tag">-{{ product.discount }}%</span>
      </div>
      <div class="product-info">
        <h3 class="product-title">{{ product.name }}</h3>
        <div class="product-rating">
          <span v-for="i in 5" :key="i" class="star" :class="{ 'filled': i <= (product.rating || 0) }">★</span>
          <span class="review-count" v-if="product.reviewCount">({{ product.reviewCount }})</span>
        </div>
        <p class="product-description">{{ product.description }}</p>
  
        <!-- Новая секция с полями от парфюма -->
        <div class="perfume-notes">
          <div class="note-item" v-if="product.perfume_code">
            <strong>Код:</strong> {{ product.perfume_code }}
          </div>
          <div class="note-item" v-if="product.analog">
            <strong>Аналог:</strong> {{ product.analog }}
          </div>
          <div class="note-item" v-if="product.top_note">
            <strong>Верхняя нота:</strong> {{ product.top_note }}
          </div>
          <div class="note-item" v-if="product.middle_note">
            <strong>Средняя нота:</strong> {{ product.middle_note }}
          </div>
          <div class="note-item" v-if="product.base_note">
            <strong>Базовая нота:</strong> {{ product.base_note }}
          </div>
        </div>
  
        <div class="volume-options">
          <span class="volume-label">Объем:</span>
          <div class="volume-selector">
            <button 
              v-for="volume in ['30мл', '50мл']" 
              :key="volume" 
              class="volume-btn" 
              :class="{ 'selected': selectedVolume === volume }"
              @click="selectedVolume = volume">
              {{ volume }}
            </button>
          </div>
        </div>
  
        <div class="price-container">
          <div class="price" v-if="priceWithDiscount < currentPrice">
            <span class="old-price">{{ currentPrice }} USD</span>
            <span class="new-price">{{ priceWithDiscount }} USD</span>
          </div>
          <div class="price" v-else>
            <span class="regular-price">{{ currentPrice }} USD</span>
          </div>
        </div>
  
        <div class="product-actions">
          <button class="add-to-cart-btn" @click="addToCart">
            <i class="fas fa-shopping-cart"></i> Добавить в корзину
          </button>
          <button class="wishlist-btn" @click="toggleWishlist">
            <i class="fas" :class="{ 'fa-heart': isInWishlist, 'fa-heart-o': !isInWishlist }"></i>
          </button>
        </div>
  
        <div class="delivery-info">
          <div class="delivery-item">
            <i class="fas fa-truck"></i>
            <span>Бесплатная доставка от 100 USD</span>
          </div>
          <div class="delivery-item">
            <i class="fas fa-sync-alt"></i>
            <span>Возврат в течение 14 дней</span>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'ProductDetails',
    props: {
      product: {
        type: Object,
        required: true
      }
    },
    data() {
      return {
        selectedVolume: '30мл',
        isInWishlist: false
      };
    },
    computed: {
      currentPrice() {
        switch(this.selectedVolume) {
          case '30мл': return this.product.price_30ml || 0;
          case '50мл': return this.product.price_50ml || 0;
          default: return this.product.price_30ml || 0;
        }
      },
      priceWithDiscount() {
        if (!this.product.discount) return this.currentPrice;
        return Math.round((this.currentPrice * (100 - this.product.discount)) / 100);
      }
    },
    methods: {
      addToCart() {
        this.$emit('add-to-cart', {
          product: this.product,
          volume: this.selectedVolume,
          quantity: 1
        });
      },
      toggleWishlist() {
        this.isInWishlist = !this.isInWishlist;
        this.$emit('toggle-wishlist', {
          product: this.product,
          inWishlist: this.isInWishlist
        });
      }
    }
  }
  </script>
  
  <style scoped>
  .product-card {
    display: flex;
    flex-direction: column;
    max-width: 900px;
    margin: 2rem auto;
    background: white;
    border-radius: 12px;
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
    overflow: hidden;
    margin-top: 100px;
  }
  
  @media (min-width: 768px) {
    .product-card {
      flex-direction: row;
      height: auto;
    }
  }
  
  .product-image-container {
    position: relative;
    width: 100%;
    background: #f8f9fa;
  }
  
  @media (min-width: 768px) {
    .product-image-container {
      width: 40%;
    }
  }
  
  .product-image {
    width: 100%;
    height: 350px;
    object-fit: cover;
    transition: transform 0.3s ease;
  }
  
  .product-image:hover {
    transform: scale(1.03);
  }
  
  .discount-tag {
    position: absolute;
    top: 15px;
    right: 15px;
    background: #e63946;
    color: white;
    padding: 6px 12px;
    border-radius: 20px;
    font-weight: bold;
    font-size: 0.9rem;
  }
  
  .product-info {
    padding: 1.5rem;
    flex: 1;
  }
  
  .product-title {
    font-size: 1.8rem;
    font-weight: 600;
    margin-bottom: 0.8rem;
    color: #212529;
  }
  
  .product-rating {
    margin-bottom: 1rem;
  }
  
  .star {
    color: #d1d1d1;
    font-size: 1.2rem;
    margin-right: 2px;
  }
  
  .star.filled {
    color: #ffc107;
  }
  
  .review-count {
    color: #6c757d;
    font-size: 0.9rem;
    margin-left: 5px;
  }
  
  .product-description {
    margin-bottom: 1rem;
    line-height: 1.6;
    color: #6c757d;
  }
  
  .perfume-notes {
    margin-bottom: 1.5rem;
    line-height: 1.6;
    color: #495057;
  }
  
  .note-item {
    margin-bottom: 0.3rem;
  }
  
  .note-item strong {
    color: #343a40;
  }
  
  .volume-options {
    margin-bottom: 1.5rem;
  }
  
  .volume-label {
    display: block;
    font-weight: 600;
    margin-bottom: 0.5rem;
    color: #343a40;
  }
  
  .volume-selector {
    display: flex;
    gap: 10px;
  }
  
  .volume-btn {
    padding: 8px 15px;
    border: 2px solid #dee2e6;
    background: none;
    border-radius: 5px;
    cursor: pointer;
    font-weight: 500;
    transition: all 0.2s;
  }
  
  .volume-btn:hover {
    border-color: #adb5bd;
  }
  
  .volume-btn.selected {
    border-color: #007bff;
    color: #007bff;
  }
  
  .price-container {
    margin-bottom: 1.5rem;
  }
  
  .old-price {
    color: #6c757d;
    text-decoration: line-through;
    margin-right: 10px;
    font-size: 1.1rem;
  }
  
  .new-price {
    color: #e63946;
    font-size: 1.8rem;
    font-weight: 700;
  }
  
  .regular-price {
    color: #212529;
    font-size: 1.8rem;
    font-weight: 700;
  }
  
  .product-actions {
    display: flex;
    gap: 15px;
    margin-bottom: 1.5rem;
  }
  
  .add-to-cart-btn {
    flex-grow: 1;
    background: #007bff;
    color: white;
    border: none;
    padding: 12px 20px;
    border-radius: 6px;
    font-weight: 600;
    cursor: pointer;
    transition: background 0.3s;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .add-to-cart-btn i {
    margin-right: 8px;
  }
  
  .add-to-cart-btn:hover {
    background: #0069d9;
  }
  
  .wishlist-btn {
    width: 48px;
    background: #f8f9fa;
    border: 1px solid #dee2e6;
    border-radius: 6px;
    cursor: pointer;
    transition: all 0.3s;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .wishlist-btn:hover {
    background: #e9ecef;
  }
  
  .wishlist-btn i.fa-heart {
    color: #e63946;
  }
  
  .wishlist-btn i.fa-heart-o {
    color: #6c757d;
  }
  
  .delivery-info {
    background: #f8f9fa;
    border-radius: 8px;
    padding: 15px;
  }
  
  .delivery-item {
    display: flex;
    align-items: center;
    margin-bottom: 8px;
  }
  
  .delivery-item:last-child {
    margin-bottom: 0;
  }
  
  .delivery-item i {
    color: #007bff;
    margin-right: 10px;
  }
  
  .delivery-item span {
    color: #495057;
    font-size: 0.9rem;
  }
  </style>
  
<template>
  <div id="app">
    <header class="header">
      <h1>Интернет-магазин</h1>
      <div class="cart-info">
        <button @click="showCart = !showCart" class="cart-toggle">
          Корзина ({{ cartItemCount }})
        </button>
        <span class="total-price">Итого: {{ totalPrice }} ₽</span>
      </div>
    </header>

    <main class="main-content">
      <ProductList 
        :products="products" 
        @add-to-cart="addToCart"
      />
      
      <ShoppingCart 
        v-if="showCart"
        :cart="cart"
        @update-quantity="updateQuantity"
        @remove-item="removeItem"
        @close-cart="showCart = false"
      />
    </main>
  </div>
</template>

<script>
import ProductList from './components/ProductList.vue'
import ShoppingCart from './components/ShoppingCart.vue'

export default {
  name: 'App',
  components: {
    ProductList,
    ShoppingCart
  },
  data() {
    return {
      showCart: false,
      products: [
        {
          id: 1,
          name: 'Ноутбук ASUS',
          price: 59990,
          image: '/images/1.jpg', 
          description: 'Мощный ноутбук для работы и игр'
        },
        {
          id: 2,
          name: 'Смартфон iPhone',
          price: 79990,
           image: '/images/2.jpg', 
          description: 'Флагманский смартфон'
        },
        {
          id: 3,
          name: 'Наушники Sony',
          price: 12990,
           image: '/images/3.jpg', 
          description: 'Беспроводные наушники'
        },
        {
          id: 4,
          name: 'Планшет Samsung',
          price: 34990,
           image: '/images/4.jpg', 
          description: '10-дюймовый планшет'
        }
      ],
      cart: []
    }
  },
  computed: {
    cartItemCount() {
      return this.cart.reduce((total, item) => total + item.quantity, 0)
    },
    totalPrice() {
      return this.cart.reduce((total, item) => total + (item.price * item.quantity), 0)
    }
  },
  methods: {
    addToCart(product) {
      const existingItem = this.cart.find(item => item.id === product.id)
      
      if (existingItem) {
        existingItem.quantity += 1
      } else {
        this.cart.push({
          ...product,
          quantity: 1
        })
      }
      
      // Показываем уведомление
      this.showNotification(`${product.name} добавлен в корзину!`)
    },
    
    updateQuantity({ id, quantity }) {
      const item = this.cart.find(item => item.id === id)
      if (item) {
        if (quantity <= 0) {
          this.removeItem(id)
        } else {
          item.quantity = quantity
        }
      }
    },
    
    removeItem(id) {
      this.cart = this.cart.filter(item => item.id !== id)
    },
    
    showNotification(message) {
      // Простое уведомление (можно заменить на toast-библиотеку)
      alert(message)
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Arial', sans-serif;
  background-color: #f5f5f5;
}

#app {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  margin-bottom: 30px;
}

.cart-info {
  display: flex;
  align-items: center;
  gap: 20px;
}

.cart-toggle {
  background: #4CAF50;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
}

.cart-toggle:hover {
  background: #45a049;
}

.total-price {
  font-weight: bold;
  font-size: 18px;
}

.main-content {
  display: grid;
  grid-template-columns: 1fr;
  gap: 30px;
}

@media (min-width: 768px) {
  .main-content {
    grid-template-columns: 2fr 1fr;
  }
}
</style>
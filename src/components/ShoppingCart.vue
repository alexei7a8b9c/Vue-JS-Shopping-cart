<template>
  <div class="cart-overlay" @click.self="$emit('close-cart')">
    <div class="cart-sidebar">
      <div class="cart-header">
        <h2>Корзина покупок</h2>
        <button class="close-btn" @click="$emit('close-cart')">×</button>
      </div>
      
      <div class="cart-content">
        <div v-if="cart.length === 0" class="empty-cart">
          Корзина пуста
        </div>
        
        <div v-else>
          <div class="cart-items">
            <div 
              v-for="item in cart" 
              :key="item.id" 
              class="cart-item"
            >
              <img :src="item.image" :alt="item.name" class="item-image">
              
              <div class="item-details">
                <h4 class="item-name">{{ item.name }}</h4>
                <p class="item-price">{{ item.price }} ₽</p>
              </div>
              
              <div class="quantity-controls">
                <button 
                  @click="updateQuantity(item.id, item.quantity - 1)"
                  class="quantity-btn"
                >-</button>
                
                <span class="quantity">{{ item.quantity }}</span>
                
                <button 
                  @click="updateQuantity(item.id, item.quantity + 1)"
                  class="quantity-btn"
                >+</button>
              </div>
              
              <div class="item-total">
                {{ item.price * item.quantity }} ₽
              </div>
              
              <button 
                @click="removeItem(item.id)"
                class="remove-btn"
              >×</button>
            </div>
          </div>
          
          <div class="cart-summary">
            <div class="summary-row">
              <span>Итого:</span>
              <span class="total-amount">{{ totalAmount }} ₽</span>
            </div>
            
            <button class="checkout-btn">
              Оформить заказ
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ShoppingCart',
  props: {
    cart: {
      type: Array,
      required: true
    }
  },
  computed: {
    totalAmount() {
      return this.cart.reduce((total, item) => total + (item.price * item.quantity), 0)
    }
  },
  methods: {
    updateQuantity(id, quantity) {
      this.$emit('update-quantity', { id, quantity })
    },
    
    removeItem(id) {
      this.$emit('remove-item', id)
    }
  },
  emits: ['update-quantity', 'remove-item', 'close-cart']
}
</script>

<style scoped>
.cart-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0,0,0,0.5);
  z-index: 1000;
}

.cart-sidebar {
  position: absolute;
  top: 0;
  right: 0;
  width: 100%;
  max-width: 400px;
  height: 100%;
  background: white;
  box-shadow: -2px 0 10px rgba(0,0,0,0.1);
  display: flex;
  flex-direction: column;
}

.cart-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
  border-bottom: 1px solid #eee;
}

.close-btn {
  background: none;
  border: none;
  font-size: 24px;
  cursor: pointer;
  color: #666;
}

.cart-content {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.empty-cart {
  text-align: center;
  padding: 40px;
  color: #666;
}

.cart-items {
  flex: 1;
  overflow-y: auto;
  padding: 20px;
}

.cart-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 15px 0;
  border-bottom: 1px solid #eee;
}

.item-image {
  width: 50px;
  height: 50px;
  object-fit: cover;
  border-radius: 5px;
}

.item-details {
  flex: 1;
}

.item-name {
  font-size: 14px;
  margin-bottom: 5px;
}

.item-price {
  font-size: 12px;
  color: #666;
}

.quantity-controls {
  display: flex;
  align-items: center;
  gap: 10px;
}

.quantity-btn {
  width: 30px;
  height: 30px;
  border: 1px solid #ddd;
  background: white;
  border-radius: 5px;
  cursor: pointer;
}

.quantity {
  font-weight: bold;
}

.item-total {
  font-weight: bold;
  min-width: 60px;
  text-align: right;
}

.remove-btn {
  background: #ff4444;
  color: white;
  border: none;
  width: 25px;
  height: 25px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 14px;
}

.cart-summary {
  padding: 20px;
  border-top: 1px solid #eee;
  background: #f9f9f9;
}

.summary-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
  font-size: 18px;
}

.total-amount {
  font-weight: bold;
  color: #4CAF50;
}

.checkout-btn {
  width: 100%;
  background: #4CAF50;
  color: white;
  border: none;
  padding: 15px;
  border-radius: 5px;
  font-size: 16px;
  cursor: pointer;
}

.checkout-btn:hover {
  background: #45a049;
}
</style>
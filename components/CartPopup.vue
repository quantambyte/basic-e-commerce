<template>
  <div v-if="isVisible" class="cart-popup">
    <header class="cart-popup__header">
      <h2>Your Cart ({{ cartItems.length }})</h2>
      <img :src="CloseIcon" alt="Close icon" @click="closePopup" />
    </header>

    <div v-if="cartItems.length > 0" class="cart-popup__items">
      <CartItem
        v-for="(product, index) in cartItems"
        :key="index"
        :product="product"
        :productIndex="index"
        @remove-item="handleRemoveItem"
      />
    </div>

    <div v-if="cartItems.length > 0" class="cart-popup__total">
      <div>
        <p>Total</p>
        <p>${{getTotal}}</p>
      </div>
      <button>
        Continue to checkout
      </button>
    </div>
  </div>
</template>

<script>
import CloseIcon from "@/assets/icons/close-icon.svg";

export default {
  name: "CartPopup",
  props: {
    isVisible: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      CloseIcon,
      cartItems: [],
    };
  },
  mounted() {
    this.loadCartItems();
    window.addEventListener("cart-updated", this.loadCartItems);
  },
  beforeDestroy() {
    window.removeEventListener("cart-updated", this.loadCartItems);
  },
  methods: {
    closePopup() {
      this.$emit("close");
    },
    loadCartItems() {
      const storedItems = JSON.parse(localStorage.getItem("cart")) || [];
      this.cartItems = storedItems;
    },
    handleRemoveItem(index) {
      this.cartItems.splice(index, 1);
      localStorage.setItem("cart", JSON.stringify(this.cartItems));
    },
  },
  computed:{
    getTotal() {
      return this.cartItems.reduce((total, item) => total + item.price, 0);
    }
  }
};
</script>

<style lang="scss" scoped>
.cart-popup {
  position: fixed;
  top: 0;
  right: 0;
  width: 320px;
  height: 100%;
  padding: 24px 32px 32px 32px;
  background-color: white;
  box-shadow: -2px 0 5px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease-in-out;
  transform: translateX(0);

  @media (max-width: 480px) {
    width: 100%;
    padding:unset !important; ;
  }

  &__header {
    color: #1A1A1A;
    display: flex;
    font-weight: 600;
    justify-content: space-between;
    align-items: center;
    border-bottom: 1px solid rgba(26, 26, 26, 0.1);

    img{
      cursor: pointer;
    }
  }

  &__items {
    margin-block: 16px;
    max-height: calc(100% - 100px);
    overflow-y: auto;
    display: flex;
    flex-direction: column;
    gap: 16px;
  }

  &__total{
    display: flex;
    flex-direction: column;
    width: 100%;
    div{
      font-weight: 600;
      font-size: 22px !important;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    button{
      background: #462DDF;
      color:white;
      padding: 16px;
      border: none;
      font-size: 16px;
      font-weight: 600;
      outline:none;
      border-radius: 8px;
    }
  }
  &.hidden {
    transform: translateX(100%);
  }
}
</style>

<template>
  <div class="cart-item">
    <div class="cart-item-image">
      <img :src="product.image" alt="Product Image" />
    </div>
    <div class="cart-item-description">
      <h3 class="cart-item-description-title">{{ product.title }}</h3>
      <div>
        <p class="cart-item-description-price">${{ product.price.toFixed(2) }}</p>
        <button @click="removeFromCart">Remove</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CartItem",
  props: {
    product: {
      type: Object,
      required: true,
    },
    productIndex: {
      type: Number,
      required: true,
    },
  },
  methods: {
    removeFromCart() {
      this.$emit('remove-item', this.productIndex);
    },
  },
};
</script>

<style lang="scss" scoped>
.cart-item {
  display: flex;
  align-items: center;
  gap: 16px;

  .cart-item-image {
    width: 102px;
    height: 102px;
    padding: 16px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 8px;
    border: 1px solid rgba(26, 26, 26, 0.1);

    img {
      max-width: 100%;
      max-height: 100%;
      object-fit: contain;
    }
  }

  .cart-item-description {
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: space-between;

    .cart-item-description-title {
      font-size: 16px;
      font-weight: 600;
    }

    div {
      display: flex;
      justify-content: space-between;
      align-items: center;

      .cart-item-description-price {
        font-size: 1rem;
        color: #333;
      }

      button {
        border: none;
        outline: none;
        color: #1A1A1A80;
        background: transparent;
        font-size: 16px;
        font-weight: 500;
        cursor: pointer;
      }
    }
  }
}
</style>

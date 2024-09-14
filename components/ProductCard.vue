<template>
  <div class="product-card">
    <img :src="product.image" alt="Product Image" class="product-card__image" />
    <div class="product-card__details">
      <h3
        class="product-card__name"
        :title="product.title"
      >
        {{ trimmedTitle }}
      </h3>
      <p class="product-card__price">${{ product.price.toFixed(2) }}</p>
      <button class="add-button" @click="addToCart" :disabled="isLoading">
        <img v-if="!isLoading" :src="AddIcon" alt="Add icon" />
        <span v-if="isLoading" class="loader"></span>
        <span v-if="!isLoading">Add to cart</span>
      </button>
    </div>
  </div>
</template>


<script>
import AddIcon from "@/assets/icons/add-icon.svg";

export default {
  name: "ProductCard",
  props: {
    product: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      AddIcon,
      isLoading: false,
    };
  },
  computed: {
    trimmedTitle() {
      return this.product.title.length > 26
        ? this.product.title.substring(0, 26) + "..."
        : this.product.title;
    },
  },
  methods: {
    addToCart() {
      this.isLoading = true; // Start loader

      setTimeout(() => {
        const currentCart = JSON.parse(localStorage.getItem("cart")) || [];

        const productExists = currentCart.find(
          (item) => item.id === this.product.id
        );

        if (!productExists) {
          currentCart.push({
            id: this.product.id,
            title: this.product.title,
            price: this.product.price,
            image: this.product.image,
          });

          localStorage.setItem("cart", JSON.stringify(currentCart));
          const cartUpdatedEvent = new Event("cart-updated");
          window.dispatchEvent(cartUpdatedEvent);

        } else {
          console.log(`${this.product.title} is already in the cart`);
        }

        this.isLoading = false;
      }, 1000);
    },
  },
};

</script>

<style lang="scss" scoped>
.product-card {
  font-family: "Instrument Sans", sans-serif;
  height: 503px;
  border: 1px solid #1A1A1A1A;
  border-radius: 12px;
  padding: 24px;
  background-color: #fff;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  gap: 32px;

  &__image {
    max-width: 100%;
    height: 280px;
  }

  &__details {
    margin-top: 12px;

    &__name {
      font-size: 16px;
      font-weight: 600;
      color: #1A1A1A;
    }

    &__price {
      font-weight: 500;
      font-size: 16px;
      color: #1A1A1A80;
    }

    .add-button {
      cursor: pointer;
      font-weight: 600;
      font-size: 16px;
      width: 100%;
      padding: 16px 20px 16px 16px;
      background: #F4F4F4;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      border-radius: 12px;
      border: none;
      outline: none;
      position: relative;

      &:disabled {
        cursor: not-allowed;
        background: #e0e0e0;
      }

      .loader {
        border: 4px solid #f3f3f3;
        border-top: 4px solid #555;
        border-radius: 50%;
        width: 20px;
        height: 20px;
        animation: spin 1s linear infinite;
      }
    }
  }

  @keyframes spin {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
}

</style>


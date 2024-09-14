<template>
  <header class="main-header">
    <div class="container">
      <h1>
        Voxel<span> Store</span>
      </h1>
      <img :src="CartIcon" alt="Cart icon" @click="toggleCartPopup" />
    </div>
    <CartPopup :isVisible="isCartPopupVisible" @close="toggleCartPopup" />
  </header>
</template>

<script>
import CartIcon from "@/assets/icons/cart-icon.svg";
import CartPopup from "@/components/CartPopup.vue";

export default {
  name: "MainHeader",
  components: {
    CartPopup,
  },
  data() {
    return {
      CartIcon,
      isCartPopupVisible: false,
    };
  },
  methods: {
    toggleCartPopup() {
      this.isCartPopupVisible = !this.isCartPopupVisible;
    },
  },
};
</script>

<style lang="scss" scoped>
$font-family: 'Instrument Sans', sans-serif;
$primary-color: #1A1A1A;
$secondary-color: rgba(26, 26, 26, 0.5);
$header-height: 64px;
$border-color: #ccc;
$container-width: 90%;
$icon-size: 24px;

@mixin flex-align($justify: space-between, $align: center) {
  display: flex;
  justify-content: $justify;
  align-items: $align;
}

.main-header {
  font-family: $font-family;
  background-color: white;
  height: $header-height;

  .container {
    @include flex-align;
    width: $container-width;
    margin-inline: auto;
    height: 100%;

    h1 {
      font-weight: 600;
      font-size: 18px;
      color: $primary-color;

      span {
        font-weight: 400;
        color: $secondary-color;
      }
    }

    img {
      height: $icon-size;
      width: $icon-size;
      cursor: pointer;
    }
  }
}
</style>

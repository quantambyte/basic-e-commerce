<template>
  <div class="welcome-hero">
    <div class="welcome-hero__title">
      <h1>
        Welcome to Voxel
      </h1>
      <p>
        Discover our wide range of products
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "WelcomeHero",
};
</script>

<style lang="scss" scoped>
$font-family: 'Instrument Sans', sans-serif;
$primary-color: #1A1A1A;
$secondary-color: rgba(26, 26, 26, 0.50);
$hero-background: #f4f4f4;
$hero-padding: 80px 20px;
$title-font-size: 48px;
$paragraph-font-size: 18px;
$text-max-width: 600px;

@mixin center-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.welcome-hero {
  background-color: $hero-background;
  padding: $hero-padding;
  text-align: center;
  @include center-content;

  &__title {
    max-width: $text-max-width;
    h1 {
      font-family: $font-family;
      font-size: $title-font-size;
      font-weight: 600;
      color: $primary-color;
      margin-bottom: 16px;
    }

    p {
      font-weight: 500;
      font-family: $font-family;
      font-size: $paragraph-font-size;
      color: $secondary-color;
    }
  }
}
</style>

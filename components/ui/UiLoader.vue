<template>
  <div class="background">
    <nuxt-icon name="loader" class="loader" filled />
  </div>
</template>

<style lang="scss" scoped>
@import "styles/variables";
@import "styles/functions";
@import "styles/mixins";
@import "styles//animations";

.background {
  @include position();
  background-color: rgba($light-grey, 0.3);

  :deep(.loader.nuxt-icon svg) {
    width: px(200);
    height: auto;
    position: fixed;
    left: calc(50% - px(100));
    top: calc(50vh - px(100));
    animation: rotate 1.5s infinite linear;
    margin: 0;

    path {
      fill: $main;
    }
  }
}
</style>

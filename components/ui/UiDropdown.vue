<template>
  <div
    class="dropdown relative"
    @mouseover="isOpen = true"
    @mouseleave="isOpen = false"
  >
    <slot name="title"></slot>

    <div :class="[!isOpen ? 'none' : '', 'dropdown-content']">
      <slot name="content"></slot>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  data() {
    return {
      isOpen: false,
    };
  },
};
</script>

<style lang="scss" scoped>
@import "styles/variables";
@import "styles/functions";

.dropdown {
  cursor: pointer;

  &-content {
    position: absolute;
    top: 100%;
    right: px(-16);
    z-index: $z-regular;
    width: 100%;
    min-width: px(180);
    display: flex;
    flex-direction: column;
    background-color: $background;
    border: 1px solid $light-grey;
    border-radius: $border-radius;

    :deep(> *) {
      cursor: pointer;
      display: block;
      font-size: px(18);
      text-align: left;
      padding: px(4);
    }

    :deep(> *:not(:last-child)) {
      border-bottom: 1px solid $light-grey;
    }
  }
}
</style>

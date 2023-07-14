<template>
  <h2 v-if="type === 'title'" :class="typographyClasses">
    <slot></slot>
  </h2>
  <span v-else-if="type === 'text'" :class="typographyClasses">
    <slot></slot>
  </span>
</template>

<script lang="ts">
export default {
  props: {
    type: {
      type: String as () => "title" | "text",
      required: true,
    },
    classes: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      typographyClasses: `${this.$props.type} ${this.$props.classes}`,
    };
  },
};
</script>

<style lang="scss" scoped>
@import "styles/functions";

.title {
  font-size: px(22);
}

.text {
  font-size: px(16);
}
</style>

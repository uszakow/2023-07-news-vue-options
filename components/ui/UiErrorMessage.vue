<template>
  <div :class="['error-message', classes, 'text-center']">
    {{ formattedMessage }}
  </div>
</template>

<script lang="ts">
export default {
  props: {
    message: {
      type: [String, Array],
      required: true,
    },
    classes: {
      type: String,
      default: "",
    },
  },
  computed: {
    formattedMessage(): string {
      if (Array.isArray(this.$props.message)) {
        return this.$props.message.join(" ");
      }
      return this.$props.message;
    },
  },
};
</script>

<style lang="scss" scoped>
@import "styles/variables";
@import "styles/functions";

.error-message {
  position: absolute;
  right: 0;
  left: 0;
  font-size: px(12);
  color: $error;
}
</style>

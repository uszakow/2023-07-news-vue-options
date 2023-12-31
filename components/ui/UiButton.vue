<template>
  <button
    :class="['button', type, 'flex-center', classes]"
    :disabled="disabled || loading"
    @click="$emit('clickButton')"
  >
    <div v-if="type === 'adding'" class="plus relative mr-1"></div>

    <nuxt-icon v-if="loading" name="loader" class="loader" />
    <template v-else>
      {{ label }}
    </template>
  </button>
</template>

<script lang="ts">
export default {
  props: {
    type: {
      type: String as () =>
        | "primary"
        | "secondary"
        | "danger"
        | "inline"
        | "adding",
      default: "primary",
    },
    label: {
      type: String,
      required: true,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    loading: {
      type: Boolean,
      default: false,
    },
    classes: {
      type: String,
      default: "",
    },
  },
  emits: ["clickButton"],
};
</script>

<style lang="scss" scoped>
@import "styles/variables";
@import "styles/functions";
@import "styles//animations";

.button {
  cursor: pointer;
}

.primary,
.secondary,
.danger {
  width: px(176);
  min-height: px(32);
  font-size: px(14);
  border-radius: $border-radius;

  :deep(.loader.nuxt-icon svg) {
    width: px(24);
    height: auto;
    margin: 0;
  }
}

.primary,
.secondary,
.danger,
.inline {
  text-transform: uppercase;
}

.primary,
.danger {
  color: $white;
  border: none;
  box-shadow: inset 0px 5px 10px 0px rgba($white, 0.5),
    0 4px 10px 0 rgba($black, 0.5);
  transition: box-shadow 0.2s ease-in-out;

  .loader path {
    fill: $white;
  }

  &:active {
    box-shadow: inset 0px 10px 20px 2px rgba($white, 0.25),
      0px 2px 5px 0 rgba($black, 0.5);
  }
}

.primary {
  background-color: $main;
}

.danger {
  background-color: $error;
}

.secondary {
  color: $main;
  background-color: $white;
  border: 2px solid $main;
  box-shadow: inset 0px 5px 10px 0px rgba($black, 0.2),
    0 4px 10px 0 rgba($black, 0.5);
  transition: box-shadow 0.2s ease-in-out;

  .loader path {
    fill: $main;
  }

  &:active {
    box-shadow: inset 0px 10px 20px 2px rgba($black, 0.1),
      0px 2px 5px 0 rgba($black, 0.5);
  }
}

.inline {
  font-size: px(16);
  color: $main;
  text-decoration: underline;
  text-underline-offset: px(4);

  &:hover {
    color: $error;
  }
}

.adding {
  font-size: px(22);
  color: $main;

  .plus {
    width: px(24);
    height: px(24);

    &::before,
    &::after {
      content: "";
      position: absolute;
      top: px(12);
      left: 0;
      width: px(24);
      height: px(2);
      background-color: $main;
    }

    &::before {
      transform: rotate(90deg);
    }
  }
}

.loader {
  animation: rotate 1.5s infinite linear;
}
</style>

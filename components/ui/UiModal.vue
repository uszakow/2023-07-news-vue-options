<template>
  <div v-if="isOpen" class="modal flex-center">
    <div class="modal-curtain" @click="$emit('closeModal')"></div>
    <div class="modal-overlap">
      <div :class="['modal-wrap', classes, 'relative p-2']">
        <div class="modal-title flex flex-justify-between pb-1 mb-2">
          <h2 class="modal-title-text">{{ title }}</h2>
          <button
            class="modal-title-button relative ml-2"
            @click="$emit('closeModal')"
          ></button>
        </div>
        <div>
          <slot></slot>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  props: {
    title: {
      type: String,
      required: true,
    },
    isOpen: {
      type: Boolean,
      required: true,
    },
    classes: {
      type: String,
      default: "",
    },
  },
  emits: ["closeModal"],
};
</script>

<style lang="scss" scoped>
@import "styles/variables";
@import "styles/functions";
@import "styles/mixins";

.modal {
  @include position(fixed);
  z-index: $z-modal;

  &-curtain {
    @include position(fixed);
    background-color: rgba($light-grey, 0.9);
  }

  &-overlap,
  &-wrap {
    max-height: 80vh;
    max-width: 80vw;
  }

  &-overlap {
    z-index: $z-modal;
    overflow: hidden;
    background-color: $white;
    border: 2px solid $main;
    border-radius: $border-radius;
  }

  &-wrap {
    overflow: auto;
    scrollbar-width: auto;

    &::-webkit-scrollbar {
      width: px(6);
    }

    &::-webkit-scrollbar-track {
      background: $light-grey;
    }

    &::-webkit-scrollbar-thumb {
      background: $main;
      border-radius: $border-radius;
    }

    &::-webkit-scrollbar-thumb:hover {
      background: $error;
    }
  }

  &-title {
    border-bottom: 1px solid $light-grey;

    &-text {
      font-size: px(22);
      color: $main;
    }

    &-button {
      cursor: pointer;
      width: px(32);
      height: px(32);
      border: px(2) solid $main;
      border-radius: $border-radius;
      transition: all 0.4s ease-in-out;

      &::before,
      &::after {
        content: "";
        position: absolute;
        top: px(14);
        left: px(1.5);
        z-index: $z-modal;
        height: 2px;
        width: px(26);
        background-color: $main;
        transition: all 0.4s ease-in-out;
      }

      &::before {
        transform: rotate(45deg);
      }

      &::after {
        transform: rotate(-45deg);
      }
    }

    &-button:hover {
      border-color: $error;

      &::before,
      &::after {
        background-color: $error;
      }

      &::before {
        transform: rotate(-135deg);
      }

      &::after {
        transform: rotate(135deg);
      }
    }
  }
}
</style>

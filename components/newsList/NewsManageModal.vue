<template>
  <ui-modal
    classes="news-modal"
    :title="modalTitle"
    :is-open="isOpen"
    @close-modal="$emit('closeModal')"
  >
    <ui-input
      :value="newsTitle"
      label="Tytuł wiadomości"
      placeholder="Wpisz tytuł wiadomości"
      :required="true"
      @value-change="$emit('changeNewsTitle', $event)"
    />

    <ui-input
      type="textarea"
      :value="newsContent"
      label="Treść wiadomości"
      placeholder="Wpisz treść wiadomości"
      :required="true"
      @value-change="$emit('changeNewsContent', $event)"
    />

    <ui-button
      classes="mt-2 m-auto"
      label="Opublikuj"
      :loading="loading"
      @click-button="$emit('manageNews')"
    />

    <ui-error-message v-if="error" :message="error" />
  </ui-modal>
</template>

<script lang="ts">
export default {
  props: {
    isOpen: {
      type: Boolean,
      required: true,
    },
    modalTitle: {
      type: String,
      required: true,
    },
    newsTitle: {
      type: String,
      required: true,
    },
    newsContent: {
      type: String,
      required: true,
    },
    loading: {
      type: Boolean,
      required: true,
    },
    error: {
      type: [String, Array],
      required: true,
    },
  },
  emits: ["changeNewsTitle", "changeNewsContent", "manageNews", "closeModal"],
};
</script>

<style lang="scss" scoped>
@import "styles/functions";

:deep(.news-modal) {
  width: px(800);
}
</style>

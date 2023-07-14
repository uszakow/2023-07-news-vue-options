<template>
  <div class="input" :title="required ? 'To pole jest obowiązkowe' : ''">
    <label v-if="label" :for="label">
      {{ label }}{{ required ? " *" : "" }}
    </label>

    <textarea
      v-if="type === 'textarea'"
      :id="label"
      :value="value"
      :placeholder="placeholder"
      :required="required"
      :rows="rowsCount"
      @change="(event: Event) => $emit('valueChange', (event.target as HTMLInputElement).value)"
    ></textarea>

    <input
      v-else
      :id="label"
      :type="type"
      :value="value"
      :placeholder="placeholder"
      :required="required"
      @input="(event: Event) => $emit('valueChange', (event.target as HTMLInputElement).value)"
    />
  </div>
</template>

<script lang="ts">
export default {
  props: {
    value: {
      type: String,
      required: true,
    },
    label: {
      type: String,
      default: "",
    },
    type: {
      type: String as () => "text" | "password" | "textarea",
      default: "text",
    },
    placeholder: {
      type: String,
      default: "",
    },
    required: {
      type: Boolean,
      default: false,
    },
    rowsCount: {
      type: Number,
      default: 10,
    },
  },
  emits: ["valueChange"],
};
</script>

<style lang="scss" scoped>
@import "styles/variables";
@import "styles/functions";

.input {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin-top: px(8);

  label {
    font-size: 12px;
    font-style: italic;
    color: $dark-grey;
    padding-bottom: px(4);
  }

  input,
  textarea {
    width: 100%;
    font-size: px(14);
    padding: px(8) px(12);
    border: 1px solid $dark-grey;
    border-radius: $border-radius;

    &::placeholder {
      font-style: italic;
      color: $light-grey;
    }
  }

  textarea {
    resize: none;
  }
}
</style>

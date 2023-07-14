<template>
  <nav class="tabs">
    <button
      v-for="tab of tabs"
      :key="tab.id"
      :class="{ active: activeTab === tab.id }"
      @click="$emit('tabChange', tab.id)"
    >
      {{ tab.label }}
    </button>
  </nav>
</template>

<script lang="ts">
import { TabItemInterface } from "@/interfaces/TabItem.interface";

export default {
  props: {
    tabs: {
      type: Array as () => TabItemInterface[],
      required: true,
    },
    activeTab: {
      type: String,
      required: true,
    },
  },
  emits: ["tabChange"],
};
</script>

<style lang="scss" scoped>
@import "styles/functions";
@import "styles/variables";

.tabs {
  display: flex;
  min-height: px(44);

  button {
    cursor: pointer;
    flex-grow: 1;
    font-size: px(20);
    border: 2px solid $main;
    color: $main;

    &.active {
      color: $white;
      background-color: $main;
    }
  }

  button:first-child {
    border-top-left-radius: px(22);
  }

  button:last-child {
    border-top-right-radius: px(22);
  }
}
</style>

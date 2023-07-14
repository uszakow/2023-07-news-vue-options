<template>
  <app-navbar />
  <main class="content">
    <slot></slot>
    <ui-loader v-show="isLoading" />
  </main>
  <app-footer />
</template>

<script lang="ts">
export default {
  watch: {
    // redirect for logged/unlogged users
    user() {
      if (!this.isLoading && !this.user && this.route.path === "/profile") {
        navigateTo("/login");
      }
      if (!this.isLoading && this.user && this.route.path === "/login") {
        navigateTo("/profile");
      }
    },
  },
  mounted() {
    this.setUserState();
  },
  setup() {
    useHead({
      title: "News app",
      meta: [
        {
          name: "description",
          content: "Application for adding and managing news.",
        },
      ],
    });

    const route = useRoute();
    const { isLoading, user, setUserState } = useAppState();

    return {
      route,
      isLoading,
      user,
      setUserState,
    };
  },
};
</script>

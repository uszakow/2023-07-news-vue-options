<template>
  <nav class="navbar flex flex-justify-between px-3">
    <nuxt-link to="/" class="navbar-item">Wiadomości</nuxt-link>

    <ui-dropdown v-if="user">
      <template v-slot:title>
        <div class="navbar-item user-name">
          {{ user?.name }}
        </div>
      </template>

      <template v-slot:content>
        <nuxt-link to="/profile" class="dropdown-item">
          Strona użytkownika
        </nuxt-link>
        <button class="dropdown-item" @click="logout()">Wyloguj się</button>
      </template>
    </ui-dropdown>
    <nuxt-link v-else to="/login" class="navbar-item">Zaloguj się</nuxt-link>
  </nav>
</template>

<script lang="ts">
export default {
  methods: {
    logout(): void {
      localStorage.removeItem("token");
      this.setUserState();
    },
  },
  setup() {
    const { user, setUserState } = useAppState();

    return {
      user,
      setUserState,
    };
  },
};
</script>

<style lang="scss" scoped>
@import "styles/variables";
@import "styles/functions";

.navbar {
  border-bottom: 1px solid $light-grey;

  &-item {
    font-size: px(22);
    font-weight: 500;
    text-transform: uppercase;
    color: $main;
    padding: px(4) px(12);
  }

  .user-name {
    text-align: right;
  }
}
</style>

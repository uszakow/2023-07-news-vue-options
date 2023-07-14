<template>
  <ui-typography
    type="title"
    classes="flex flex-justify-between flex-align-end"
  >
    <span>Nazwa użytkownika: {{ user?.name }}</span>
    <ui-button type="inline" label="Wyloguj się" @click-button="logout()" />
  </ui-typography>
  <ui-typography type="text" classes="block">
    {{ user?.name }} stworzył {{ newsCount }}
  </ui-typography>

  <profile-data-change />
</template>

<script lang="ts">
export default {
  computed: {
    newsCount(): string {
      const count = this.user?.news.length ?? 0;
      const text = count === 1 ? "wiadomość" : "wiadomości";
      return `${count} ${text}`;
    },
  },
  methods: {
    logout(): void {
      localStorage.removeItem("token");
      this.setUserState();
    },
  },
  mounted() {
    this.setUserState();
  },
  setup() {
    useHead({
      title: "Strona użytkownika",
    });

    const { user, setUserState } = useAppState();

    return {
      user,
      setUserState,
    };
  },
};
</script>
